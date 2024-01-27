---
title: 常用的git命令
date: 2023-04-22T01:29:30.845Z
tags:
  - git
categories:
  - git
keywords: git 代理
description: git代理问题
copyright_author: wuy
copyright_info: 此文章版权归wuy所有，如有转载，请注明来自原作者
---

# git与代理问题

使用代理的时候经常会遇到这样的情况，无法使用命令`git push`上传到远程代码仓库



> 原因：git默认不走代理接口



## 解决：设置代理服务器:

第一种方法：在.gitconfig加上
```
http.proxy=127.0.0.1:7890
http.sslVerify=false
```
第二种方法：直接在命令行敲
```
git config --global http.proxy 127.0.0.1:7890
git config --global http.sslVerify false
```



