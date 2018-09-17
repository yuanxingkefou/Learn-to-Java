#使用JSTL（JSP Standard Tag Library）JSP定制标记

##1）**使用<c:forEach>完成循环**
```
<c:forEach var="movie" items="${movieList}" varStatus="movieLoopCount">
    <tr>
        <td>Count: ${movieLoopCount.count}</td>
    </tr>
    <tr>
        <td>${movie}</td>
    </tr>
</c:forEach>
```

等价与

```
String[] items=(String[]) request.getAttribute("movieList");
int movieLoopCount.count=0;
for(int i=0;i<items.length;i++)
{
    String movie=items[i];
    out.println(movieLoopCount.count);
    out.println(movie);
}
```

##2）**使用<c:if>完成条件包含**
```
<c:if test="">
</c:if>
```

##3）使用
```
<c:choose>
    <c:when>
    </c:when>
    <c:when>
    </c:when>
    <c:otherwise>
    </c:otherwise>
</c:choose>
```
**完成if-else语句**

三个体（when,when,otherwise）只能运行其中一个

##4）**使用<c:set>设置属性变量var**

没有体：
`<c:set var="userLevel" scope="session" value="Cowboy" />`

有体：
```
<c:set var="userLevel" scope="session">
    Sheriff,Bartender,Cowgirl（计算体，并用作变量的值）
</ c:set>
```
如果值计算为null，变量会被删除

##5）**使用<c:set>设置一个目标性质或值（Map值和bean性质)**

没有体：
`<c:set target="${PetMap}" property="dogName" value="Clover" />`

有体：
```
<c:set target="${person}" property="name" >
    ${foo.name}
</c:set>
```

target必须是一个对象，而不是bean或Map属性的id名

<c:set>中scope是可选的，默认为page

##6）**使用<c:remove>删除一个属性**
`
<c:remove var="userStatus" scope="request" />`

##7）**使用`<c:import url="  " />`将一个资源导入到一个JSP中**

可以应用到Web应用之外

##8）**使用`<c:url value='/inputComments.jsp' />`进行url重写**

可以满足所有超链接需求

在DD中使用<error-page>标记可以为整个web应用声明错误页面

可以根据<exception-type>或http状态码<error-code>声明具体的错误页面
```
<error-page>
    <exception-type>java.lang.Throwable</exception-type>
    <location>/errorPage.jsp</location>
</error-page>
```

##9）**使用<c:catch>捕获异常，类似于try/catch块**
```
<c:catch var="myException">
    <% int x=10/0; %>
</c:catch>
```

可以通过${myException.message}得到异常具体信息

理解TLD

如果<rtexprvalue>为false，或者未定义，那么属性值只能是一个String直接量
