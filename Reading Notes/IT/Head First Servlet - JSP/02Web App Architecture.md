#Web App Architecture

servlets没有main方法，它们是由一种叫做容器(Container)的java应用控制

Tomcat是容器的一个例子

**Container的作用：**

1）communications support

container为servlets和web服务器之间提供了一个简单的沟通方式，不需要去建一个ServerSocket

2）lifecycle management

container可以控制servlets的创建和删除

3）multithreading support

container动态地为每一个servlet请求创建一个新的Java进程

4）declaration security

通过使用XML可以在不接触Java核心源码的情况下解决声明问题和一些安全问题

5）Jsp support

container可以将jsp翻译成真正的java语言

**一个servlet可以有三个名称（可以在部署和应用时更加灵活，没有必要知道每个servlet的具体位置）**

1）class name

包括类的名称和包的名称，有一个完整的路径和文件名字，取决与包目录结构在服务器中的位置

2）deployment name

开发这在部署servlet时用的一个名称

3）public URL name

客户所知道的名称

Deployment Descriptor（部署描述符）DD
是一个XML文件，告诉container怎样运行servetls和jsps

一个J2EE应用服务器包括一个web Container和一个EJB（enterprise javaBean) Container
