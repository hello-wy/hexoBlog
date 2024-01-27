---
title: Hello World
cover:https://upload.wikimedia.org/wikipedia/commons/3/32/OpenCV_Logo_with_text_svg_version.svg
copyright:
  author: wuy
  info: 此文章版权归wuy所有，如有转载，请注明来自原作者
date: 2023-05-3 18:50:38
tags: macos环境安装
categories:
  - python
  - macos
  - opencv
top_img:
---



# macos安装opencv

## 卸载现有的opencv

```bash
pip uninstall opencv-contrib-python opencv-python
```



## 下载opencv安装包

https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple/opencv-python/

https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple/opencv-contrib-python/

需要注意的是opencv 版本需要跟你的python版本相同 cp38就是python版本3.8，还有需要注意opencv-python以及opencv-contrib-python版本尽量相同



## 离线安装

```bash
pip install /home/xxx/opencv_contrib_python-4.1.0.25-cp36-cp36m-manylinux1_x86_64.whl
```

