---
title: 整合junit
copyright: true
copyright_author: wuy
copyright_info: 此文章版权归wuy所有，如有转载，请注明来自原作者
date: 2024-01-27 18:50:38
tags:
categories: 
- java
- ssm
top_img:
cover:
---



# 单元测试

## 引入xml



```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```





## 测试文件

ide 添加测试文件快捷键 command + shift + T

@SpringBootTest 声明测试

![image-20240121125233149](assets/image-20240121125233149.png)