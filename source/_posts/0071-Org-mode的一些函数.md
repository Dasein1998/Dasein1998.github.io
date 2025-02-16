---

title: 对笔记的思考
date: 2024-11-17 11:14:14
tags: 
- 折腾
categories: 
- 工具
my: Org-mode—function
---

把多个org文件合成一个org文件：
```lisp
;;; some function
;;move headline to a single file
;;https://emacs.stackexchange.com/questions/22078/how-to-split-a-long-org-file-into-separate-org-files
(defun my/org-move-tree (buffer-file-name)
  "move the sub-tree which contains the point to a file,
and replace it with a link to the newly created file"
  (interactive "F")
  (org-mark-subtree)
  (let*
      ((title    (car (last (org-get-outline-path t))))
       (dir      (file-name-directory buffer-file-name))
       (filename (concat dir title ".org"))
       (content  (buffer-substring (region-beginning) (region-end))))
    (delete-region (region-beginning) (region-end))
    (insert (format "** [[file:%s][%s]]\n" filename title))
    (with-temp-buffer
      (insert content)
      (write-file filename))))
```
把一个org文件拆成多个org文件：


```lisp
;;split one big org file to multi single org file by first headline
;;https://emacs.stackexchange.com/questions/66828/split-org-file-into-smaller-ones
(defun my/org-export-each-level-1-headline-to-org (&optional scope)
  (interactive)
  (org-map-entries
   (lambda ()
     (let* ((title (car (last (org-get-outline-path t))))
            (dir (file-name-directory buffer-file-name))
            (filename (concat dir title ".org"))
            content)
       (org-narrow-to-subtree)
       (setq content (buffer-substring-no-properties (point-min) (point-max)))
       (with-temp-buffer
         (insert content)
         (write-file filename))
       (widen)))
   "LEVEL=1" scope))
```

把一个org文件中的标题反向
```lisp
(defun org-reverse-headers ()
  "Reverse headers of current org file"
  (interactive)
  (let (str
    (content (nthcdr 2 (org-element-parse-buffer 'headline)))) ;; `org-element-parse-buffer' returns (ORG-DATA PROPERTIES CONTENT), CONTENT contains the headlines
    (setq content (nreverse content)) ;; reversal of sections
    (insert
     (with-output-to-string
       (dolist (header content)
     (princ (buffer-substring (plist-get (cadr header) :begin) (plist-get (cadr header) :end))))
       (delete-region (point-min) (point-max))))))
```


删除当前的文件和buffer
```lisp
;;delete current buffer and file
;; based on http://emacsredux.com/blog/2013/04/03/delete-file-and-buffer/
(defun my/delete-file-and-buffer ()
  "Kill the current buffer and deletes the file it is visiting."
  (interactive)
  (let ((filename (buffer-file-name)))
    (if filename
        (if (y-or-n-p (concat "Do you really want to delete file " filename " ?"))
            (progn
              (delete-file filename)
              (message "Deleted file %s." filename)
              (kill-buffer)))
      (message "Not a file visiting buffer!"))))
```
