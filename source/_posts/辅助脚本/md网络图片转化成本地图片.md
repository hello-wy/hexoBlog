---
title: RabbitMQ 高级
copyright: true
copyright_author: wuy
copyright_info: 此文章版权归wuy所有，如有转载，请注明来自原作者
date: 2024-02-15
tags:
categories: 
- java
- rabbit
top_img:
cover:

---

## 问题：

本地的markdown文档中的图片在云端图床，不能一次性全部自动下载到本地备份，只能手动一个一个下载，不方便管理

## 使用方法：

在当前目录下分别创建文件名为 `old` 和 `new` 的文件夹；
将自己的 md 文件或者包含 md 文件的文件夹 放在 `old` 文件夹下；

## 效果：

该脚本会将`old`中的所有文件复制到`new`文件夹中,`old`文件夹内容不做修改，`new`文件夹中的内容中的图片链接变为本地图片的相对路径；
会根据 `markdown` 文档中的`http`或者`https`图片链接，将图片下载到与该 `md` 文件路径同级目录的 `asset` 文件夹中。
使用前提：需要对应的图床能够支持 `空 Referer`，同时能够支持批量下载，无额外限制



## 代码如下：

```python
import os
import re
import requests
import shutil

def download_images(path):
    with open(path, 'r', encoding='utf-8') as f:
        content = f.read()
        matches = re.findall(r"!\[.*\]\((.+)\)", content)
        for url in matches:
            if url.startswith("http") or url.startswith("https"):
                asset_path = os.path.join(
                    os.path.dirname(path), 'assets', os.path.basename(url))
                if not os.path.exists(os.path.dirname(asset_path)):
                    os.makedirs(os.path.dirname(asset_path))
                if not os.path.exists(asset_path):
                    with requests.get(url, stream=True) as r, open(asset_path, 'wb') as f:
                        shutil.copyfileobj(r.raw, f)
                content = content.replace(
                    url, f"./assets/{os.path.basename(url)}")

        with open(path, 'w', encoding='utf-8') as f:
            f.write(content)


def batch_download_images(rootpath):
    for subdir, _, files in os.walk(rootpath):
        for file in files:
            if file.endswith('.md'):
                path = os.path.join(subdir, file)
                download_images(path)
                print(f"Images in {path} have been downloaded.")


# 定义旧文件夹和新文件夹
old_dir = './old'
new_dir = './new'
delete_file_in_new = 'README.md'

# 检查文件夹是否存在
if os.path.exists(new_dir):
    # 删除文件夹及其内容
    shutil.rmtree(new_dir)
    os.makedirs(new_dir)
else:
    os.makedirs(new_dir)

shutil.copytree(old_dir, new_dir, dirs_exist_ok=True)
batch_download_images(new_dir)

# 删除不需要转换的README.md文件
file_path = os.path.join(new_dir, delete_file_in_new)
if os.path.exists(file_path):
    os.remove(file_path)
print("all images was extract successfully in new/assets/ directory！")

```

