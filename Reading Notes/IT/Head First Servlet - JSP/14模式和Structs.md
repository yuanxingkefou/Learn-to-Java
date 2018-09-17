#模式和Structs

code to interfaces遵循接口编写代码

separation of Concerns & cohesion关注点分离和内聚

Hide Complexity隐藏复杂性

**设计原则：**

loose coupling松耦合

remote proxy远程代理

increase declarative control增强声明性控制

**如何处理远程对象：**

1）JNDI（Java Naming and Directory Interface）

通过向JNDI注册，就可以通过JNDI查找想要访问的对象，而不需要知道对象的具体位置

2）RMI（Remote Method Invocation)

**用服务定位器简化业务委托**
伪代码：
```
得到一个InitialContext对象
完成远程查找
处理远程问题
还可能会缓存引用
```

**业务层模式：**

1）向JNDI注册你的服务

2）使用业务委托和服务定位器从JNDI得到管理顾客桩

3）使用业务委托和桩得到“顾客bean”，在这里会得到一个传输对象，把这个传输对象的引用返回给控制器

4）把bean的引用增加到请求

5）控制器转发到视图JSP，JSP从请求对象得到顾客传输对象bean的引用

6）视图JSP使用EL得到所需的顾客传输对象bean的性质，来满足最初的请求

**Structs关键组件：**

1)Action servlet

2)表单bean

3)Action对象

4）struct-config.xml
