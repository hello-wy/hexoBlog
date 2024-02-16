---
title: AOP实现接口签名
copyright: true
copyright_author: wuy
copyright_info: 此文章版权归wuy所有，如有转载，请注明来自原作者
date: 2024-02-04
tags:
categories: 
- java
- springboot
- AOP
top_img:
cover: 
---



[实例链接](https://pdai.tech/md/spring/springboot/springboot-x-interface-jiami.html)



定义注解

```
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface Signature {
}
```

首先使用AOP实现一个 sign 校验的类



再使用拦截器实现，第一次请求的话那就生成一个`X-SIGN`放到 header 中



直接在接口上使用注解@Signature