---
title: 服务器运行 selenium
description: centos 运行selenium
date: 2023-04-22T01:11:09.567Z
draft: true
copyright:
  author: wuy
  info: 此文章版权归wuy所有，如有转载，请注明来自原作者
type: default
category: 服务器
tags: ['服务器', '环境配置']
---

# 服务器运行 selenium

## 下载 chromeDrive

从[这里](https://sites.google.com/chromium.org/driver/downloads)下载对应你的 chrome 版本的 chromeDrive。

## 下载 chrome

### 首先需要安装 chrome 依赖

在 CentOS 中，使用 yum 包管理器来安装这些依赖项。

1. 打开终端，以 root 用户身份登录。

2. 更新 yum 包缓存：

```
yum update
```

3. 安装依赖项：

```
yum install -y libX11 GConf2 fontconfig libXfont Xvfb
```

4. 安装 chrome 浏览器

```bash
sudo yum localinstall google-chrome-stable_current_x86_64.rpm
```

## 开启虚拟显示器

CentOS 默认没有安装图形用户界面（GUI），所以在服务器上运行 Chrome 时需要启动一个虚拟显示器。您可以使用 Xvfb（虚拟帧缓存）来实现这个目的。

以下是一些可能的解决方案：

1. 安装 Xvfb

```
sudo yum install -y xorg-x11-server-Xvfb
```

2. 启动 Xvfb

```
Xvfb :99 -screen 0 1280x1024x24 &
```

这会在后台启动一个虚拟显示器，您可以在 Python 代码中设置 DISPLAY 环境变量来使用它：

```python
from selenium import webdriver

option = webdriver.ChromeOptions()
option.add_argument('--headless')  # 开启无界面模式
option.add_argument('--disable-gpu')  # 禁用显卡
option.add_argument("--user-agent=Mozilla/5.0 HAHA")  # 替换UA
option.add_argument('--no-sandbox')
option.add_argument('--disable-dev-shm-usage')
option.add_argument('--disable-gpu')
option.add_argument('--headless')
option.add_argument('--disable-infobars')
option.add_argument('--remote-debugging-port=9222')
option.add_argument('--start-maximized')
option.add_argument('--disable-extensions')
option.add_argument('--disable-popup-blocking')
option.add_argument('--disable-default-apps')
option.add_argument('--disable-web-security')
option.add_argument('--disable-logging')
option.add_argument(
    '--user-agent=Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36')
option.add_argument('--lang=zh-CN,zh')
option.add_argument(
    '--disable-blink-features=AutomationControlled')

os.environ['DISPLAY'] = ':99'
driver = webdriver.Chrome(options=options)
```

## 运行代码

​ 图书馆代码

```python
import time
import os
import json
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.by import By

option = webdriver.ChromeOptions()
option.add_argument('--headless')  # 开启无界面模式
option.add_argument('--disable-gpu')  # 禁用显卡
option.add_argument("--user-agent=Mozilla/5.0 HAHA")  # 替换UA
option.add_argument('--no-sandbox')
option.add_argument('--disable-dev-shm-usage')
option.add_argument('--disable-gpu')
option.add_argument('--headless')
option.add_argument('--disable-infobars')
option.add_argument('--remote-debugging-port=9222')
option.add_argument('--start-maximized')
option.add_argument('--disable-extensions')
option.add_argument('--disable-popup-blocking')
option.add_argument('--disable-default-apps')
option.add_argument('--disable-web-security')
option.add_argument('--disable-logging')
option.add_argument(
    '--user-agent=Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36')
option.add_argument('--lang=zh-CN,zh')
option.add_argument(
    '--disable-blink-features=AutomationControlled')

os.environ['DISPLAY'] = ':99'
# proxy_str = '127.0.0.1:7890'
# option.add_argument('--proxy-server=https://{}'.format(proxy_str))  # 使用代理ip


def init():
    account = {}
    with open("account.json", "r") as f:
        account = json.load(f)

        if account == {}:
            return {
                'username': '2020283307',
                'password': 'WY010617!',
                'ic-cookie': ''
            }
        return account


account = init()
# ser = Service("/home/library/chromedriver")
driver = webdriver.Chrome("/home/library/chromedriver", options=option)

driver.get('http://icspace.lib.zjhu.edu.cn/')

wait = WebDriverWait(driver, 20)
element = wait.until(EC.presence_of_element_located((By.NAME, 'username')))

username = driver.find_element(By.NAME, 'username')
username.send_keys(account['username'])
ppassword = driver.find_element(By.ID, 'ppassword')
driver.execute_script(
    "arguments[0].value=arguments[1];", ppassword, account['password'])

submit = driver.find_element(By.ID, 'dl')
submit.click()

cookies = driver.get_cookies()
for cookie in cookies:
    if cookie["name"] == "ic-cookie":
        print(cookie["value"])
        account['ic-cookie'] = cookie["value"]
        break

with open("account.json", "w") as f:
    json.dump(account, f, indent=4)

driver.implicitly_wait(10)

driver.quit()

```
