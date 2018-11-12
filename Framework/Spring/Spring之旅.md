#Spring之旅

**##简化Java开发**

* 激发POJO的潜能
  在基于Spring构建的应用中，它的类通常没有任何痕迹表明你使用了Spring规范的类

* [依赖注入（DI）](https://github.com/yuanxingkefou/Learn-to-Java/new/master/Framework/Spring/依赖注入.md)  
  让相互协作的软件组件保持松散耦合

* 应用切面（AOP）
  运行把遍布应用各处的功能分离出来形成可重用的组件，注入日志，安全等

* 使用模块消除样板样式代码
  如Spring的JdbcTemplate可以避免传统的JDBC样板代码

**##容纳你的bean**

使用应用上下文（Application Context）：

ApplicationContext |作用
-------------------|--------------------
AnnotationConfigApplicationContext|从一个或多个基于Java的配置类中加载Spring上下文
AnnotationConfigWebApplicationContext|从一个或多个基于Java的配置类中加载Spring Web应用上下文
ClassPathXmlApplicationContext|从类路径下的一个或多个XML配置文件中加载上下文
FileSystemXmlapplicationcontext|从文件系统下的一个或多个XML配置文件中加载上下文
XmlWebApplicationContext|从web应用下的一个或多个XML配置文件中加载上下文


bean的生命周期：

![image]()

**##Spring模块**

![image]()

**##Spring的新特性**

* Spring 3.1新特性

* Spring 3.2新特性

* Spring 4.0新特性
