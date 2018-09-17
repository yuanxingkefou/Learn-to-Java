#Being a web APP

<init-param>作用于一个servlet

<context-param>作用于整个web App

![image]()

ServletConfig is one per Servlet

ServletContext is one per web app

HttpSessionAttributeListener类只是想知道会话中何时增加，删除或替换了某种类型的属性。

HttpSessionBindingListener类使得属性本身知道它何时增加到一个会话中

**属性attribute和参数parameter的区别**

![image]()

**三个作用域：**

上下文context(应用中每一部分都能访问）

请求request（能访问特定ServletRequest的部分能访问）

会话session（能访问特定HttpSession的部分能访问）

上下文属性是非线程安全的（解决方法：对上下文进行加锁）
会话也是非线程安全的

只有请求属性和局部变量是线程安全的

RequestDispatcher只有两个方法：

forward()

include()

如果属性被替换，getValue方法返回的是原来的属性
