---
title: git多账号管理
date: 2023-1-1
tags:
  - git
categories:
  - git
copyright_author: wuy
copyright_info: 此文章版权归wuy所有，如有转载，请注明来自原作者
---



# git多账号管理

## 清空默认账号

首先在管理账号的时候确保电脑的 git 未配置全局 git 账号。若配置了全局账号则按照如下操作进行清空默认的账号:

```bash
git config --global --unset user.name
git config --global --unset user.email
```



## 生成 ssh-key 

现在需要在自己使用的电脑上生成一个 ssh-key 

```bash
ssh-keygen -t id_ras_github -C "xxx@xx.com"
```

会在 `~/.ssh` 目录下生成两个文件，一个是`XX.pub`存放公钥的文件，另一个是私钥的文件。



## 将 `ssh-key` 分别添加到 `ssh-agent` 信任列表

```bash
ssh-add ~/.ssh/id_ras_github
```







## config 文件中配置多个 ssh-key
