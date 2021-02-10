---
title: 配置HEXO
date: 2020-01-26 16:55:51
tags: 折腾
---

## HEXO安装

1. 安装nodejs
2. 'npm install -g hexo-cli' 安装hexo  
3. hexo init
4. npm install
5. npm install hexo-deployer-git  

## Git登陆

```
git config --global user.name "你的GitHub用户名"
git config --global user.email "你的GitHub注册邮箱"

ssh-keygen -t rsa -C "你的GitHub注册邮箱"
将公钥添加到GitHub
```

## cmd - SOCKS5 代理设置

> set http_proxy=socks5://127.0.0.1:7891
> set https_proxy=socks5://127.0.0.1:7891

## Git代理

>git config --global http.proxy socks5://127.0.0.1:7891
>git config --global https.proxy socks5://127.0.0.1:7891

## HEXO上传到github
```
git add .
git commit -m "..."
git push

发布到GitHub：
hexo g -d 
```

## 从GitHub恢复HEXO


```
从GitHub拉取:
git clone git@github.com:GitHubname/GitHubname.github.io.git
```
Git bash依次执行下列指令：
>npm install hexo
>npm install
>npm install  hexo-deployer-git

