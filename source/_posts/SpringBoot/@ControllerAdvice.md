---
title: ControllerAdvice 三大用处
copyright: true
copyright_author: wuy
copyright_info: 此文章版权归wuy所有，如有转载，请注明来自原作者
date: 2024-02-03
tags:
categories: 
- java
- springboot
top_img:
cover: 
---



- 全局异常处理（见统一异常处理文章）
    - `@ExceptionHandler`
- 全局数据绑定
    - `@ModelAttribute`
- 全局数据预处理
    - `@InitBinder`



## 全局数据绑定

定义方法

```java
@ModelAttribute("currentUser")
public UserDetails modelAttribute() {
    return (UserDetails) SecurityContextHolder.getContext().getAuthentication().getPrincipal();
}
```



使用在参数中引用

```java
public ResponseEntity<String> saveSomeObj(@ModelAttribute("currentUser") UserDetails operator) {
    // 保存操作，并设置当前操作人员的ID（从UserDetails中获得）
    return ResponseEntity.success("ok");
}
```



## 全局数据预处理（时间）

```java
@InitBinder
public void handleInitBinder(WebDataBinder dataBinder){
    dataBinder.registerCustomEditor(Date.class,
            new CustomDateEditor(new SimpleDateFormat("yyyy-MM-dd"), false));
}
```



时间对象自动处理

```java
public Date processApi(Date date) {
    return date;
}
```

