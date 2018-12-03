#借助Spring Boot简化Spring开发

**##Spring Boot简介**

特点：

* Spring Boot Starter ：它将常用的依赖分组进行了整合，将其合并到一个依赖中，这样就可以一次性添加到项目构建中，可以减少依赖列表的长度
  
* 自动配置：利用了Spring4对条件化配置的支持，合理地推测应用所需的bean并自动化配置它们
  
* 命令行接口（CLI）：发挥了Groovy编程语言的优势，并结合自动配置进一步简化Spring应用的开发
```
@RestController
class Hi
{
  @RequestMapping("/")
  String hi()
  {
      "Hi!"
  }
}
```

* Actuator：为Spring Boot添加了一定的管理特性
