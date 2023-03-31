title: 常用git命令
author: 远方
tags:
  - LeetCode
  - 算法
categories:
  - LeetCode破局攻略
date: 2016-01-01 19:20:00
---
---
title: 常用的git命令
date: {{ date }}
tags: 
    - git
categories:
    - git
keywords:
description:git常用命令
top_img: https://www.linuxidc.com/upload/2015_10/151004070281631.jpg
comments: true
cover: https://cdn.vox-cdn.com/thumbor/A4_6e24biy8bp4ahrL-TNfaircI=/0x0:2040x1360/1200x800/filters:focal(1287x538:1613x864)/cdn.vox-cdn.com/uploads/chorus_image/image/63739082/git.0.jpg
toc:
toc_number:
toc_style_simple:
copyright: true
highlight_shrink:
copyright_author: 'wuy'
copyright_author_href: https://xxxxxx.com
copyright_info: '此文章版权归wuy所有，如有转载，请注明来自原作者'
---



# 强制上传文件

```bash
# origin 仓库地址 
# 后面是分支名称
git push -u origin feature/20220630_commodity -f 
```



# 拉取指定分支的文件

```bash
git pull origin feature/20220630_commodity  
```



# 查看本地仓库的文件

```bash
git ls-files |grep <文件名称>
```



# 查看所有的分支

```bash
git branch -a

==============
  a
  feature/20220630_commodity
  master
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev
  remotes/origin/feature/20220630_commodity
  remotes/origin/master
```

远程仓库都是含有remotes。



# 切换分支

```bash
git checkout remotes/origin/feature/20220630_commodity
```

