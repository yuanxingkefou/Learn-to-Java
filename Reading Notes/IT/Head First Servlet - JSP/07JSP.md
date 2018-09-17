#作为JSP

**JSP中的元素**：scriptlet，指令，声明，Java表达式，EL（expression Language）表达式。

**JSP中的动作：**

1）标准动作：   ` <jsp:include  page="wickedFooter.jsp" />`

2）其他动作：    `<c:set var="rate"  value="32" />`

元素          | 符号
-----------   |-------------
scriptlet:    |<%.......%>
指令:         | <%@.....%>
表达式:        | <%=......%>
JSP声明:      |<%!......%>

（声明在类中定义，而且置于服务方法之外，可以声明静态及实例变量和方法）

表达式和指令最后没有分号

表达式会成为out.print()的参数

使用page指令导入包：
`
<%@ page import="foo.*,java.util.*" %>`

Container根据JSP生成一个类，这个类实现了HttpJspPage接口，有3个关键方法

jspInit()

jspDestroy()

_jspService()

**初始化JSP：**

在JSP中完成servlet初始化工作：

1）配置servlet初始化参数：在<servlet>中增加一个<jsp-file>元素

2）覆盖jspInit()

JSP中的属性：

![image](
https://github.com/yuanxingkefou/Learn-to-Java/blob/master/Reading%20Notes/IT/Head%20First%20Servlet%20-%20JSP/image/AttributeInJSP.png)

可以使用PageContext引用得到任意作用域的属性

**指令：**

1）page指令

定义页面特定的属性，如字符编码，页面响应的内容类型

2）taglib指令

定义JSP可以使用的标记库

3）include指令

定义在转换是增加到当前页面的文本和代码，这样就能建立可重用的块，就不必在每个JSP中重复代码

在DD中放一个
`<scripting-invalid>true</scripting-invalid>`
就可以让JSP禁用脚本元素
