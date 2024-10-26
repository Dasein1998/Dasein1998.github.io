---
title: Zotero 使用Zotfile（Attanger) 和Better BibTex
date: 2024-10-02 12:12:12
tags: 
- 折腾 
categories: 
my: Zotfile_and_betterbibtex_in_Zotero7
---
## Zotero-attanger使用

在 Zotero7 中，Zotfile无法使用（开发者说只支持到 Zotero6 ）。使用的是国内开发者开发的[MuiseDestiny/zotero-attanger: Attachment Manager for Zotero](https://github.com/MuiseDestiny/zotero-attanger)。

它调用的重名命方式为 Zotero 内置的文件名格式。
![](https://cdn.sa.net/2024/10/02/uLWaUiVTbwet1hI.png)
为保证文件名和 BibTex 的 citekey 保持一致，并保持一定的可读性，将其写为：

```
{{authors editors max="1"suffix="_"}}{{year suffix="_"}}{{ title truncate="1000" case="hyphen"}}
```

既“第一作者_年份_文章名”，文章名的单词用-连接。
使用 zotero-attanger，既可将 pdf 附件重名命为“第一作者_年份_文章名.pdf”。比如图中所示的“Liu_2020_12.6-w-single-frequency-continuous-wave-671-nm-laser-with-an-external-second-harmonic-generation-cavity.pdf”。
其它设置参数可参考[file\_renaming \[Zotero Documentation\]](https://www.zotero.org/support/file_renaming)。

其设置的重点在于，用`{{}}` 包起来的 Variables 和 Parameters，形式如`{{Variables  Parameters=Value}}`。

## Better BibTex使用

 [Better BibTex](https://github.com/retorquere/zotero-better-bibtex)设置如下图：
![](https://cdn.sa.net/2024/10/02/p7OWGEekNhmHbic.png)

```
[Auth]_[year]_[Title:nopunct:condense,-:lower]
```

这种设置下，它输出的 BibTex 中的 citekey 则为“第一作者_年份_文章名”，与重命名后的pdf文件名相同。
有一个需要注意的点，Better BibTex设置中有一项为：强制引用为纯文本。
默认是勾选的，所有的中文作者、中文题目，都会被转为拼音。
但为和 Zotero 的文件名称保持一致，应取消勾选。（如上图所示）
其余 citekey 可参考[Customize the citation key generator | JabRef](https://docs.jabref.org/setup/citationkeypatterns)和[Customize the citation key generator | JabRef](https://docs.jabref.org/setup/citationkeypatterns)


## Reference

[【从零搭建Emacs个人知识库】Zotero：简单实用的科研文献管理器\_哔哩哔哩\_bilibili](https://www.bilibili.com/video/BV1Lc411J7gQ/)
