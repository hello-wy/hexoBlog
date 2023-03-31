# 配置全局逻辑删除

在application.yml中配置

```yml
mybatis-plus:
  mapper-locations: classpath:/mapper/**/*.xml
  global-config:
    db-config:
      id-type: auto
      logic-delete-value: 1		#主要设置这里，如果你是默认的，那么可以不用配置，
      logic-not-delete-value: 0 
```



# 在实体类中配置

找到具体字段使用注解@TableLogic

```java
/**
 * 是否显示[0-不显示，1显示]
 * value 是逻辑未删除值， deval是逻辑删除值
 */
@TableLogic(value = "1",delval = "0")
private Integer showStatus;
```





# 使用

调用deleteBatchIds，批量逻辑删除元祖