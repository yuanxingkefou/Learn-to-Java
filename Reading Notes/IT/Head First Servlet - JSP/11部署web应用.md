#部署web应用

WAR文件（web Archive）是web应用结构的一个快照，采用了一种可移植的压缩形式（实际上就是一个JAR文件)。

把文件放在WEB-INF下就能避免直接访问，也可以在部署为一个WAR文件时，将不允许直接访问的文件放在META-INF下

**在DD中配置欢迎文件：**
```
<welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welocme-file>default.jsp</welcome-file>
</welcome-file-list>
```

在DD中配置错误页面：

**在DD中配置servlet初始化：**

servlet会默认地在第一个请求到来时初始化，为了减少第一个用户的开销，可以使用
`<load-on-startup>1</load-on-startup>`
来在部署时加载servlet，中间的非零数决定servlet加载的顺序

建立一个XML兼容的JSP：

![image](https://github.com/yuanxingkefou/Learn-to-Java/blob/master/Reading%20Notes/IT/Head%20First%20Servlet%20-%20JSP/image/JSPDocumnet.png)

**本地bean的引用：**
```
<ejb-local-ref>
    <ejb-ref-name>ejb/Customer</ejb-ref-name>
    <ejb-ref-type>Entity</ejb-ref-type>
    <local-home>com.wickedlysmart.CustomerHome</local-home>
    <local>com.wickedlysmart.Customer</local>
<ejb-local-ref>
```

**远程bean的引用**
```
<ejb-ref>
    <ejb-ref-name>    </ejb-ref-name>
    <ejb-ref-type>    </ejb-ref-type>
    <home>    </home>
    <remote>    </remote>
</ejb-ref>
```

**声明引用的JNDI环境项：**
```
<env-entry>
    <env-entry-name>rates/discountRate</env-entry-name>
    <env-entry-type>java.lang.Integer</env-entry-type>
    <env-entry-value>10</env-entry-value>
</env-entry>

<mime-mapping>
    <extension>mpg</extension>
    <mime-type>video/mpeg</mime-type>
<mime-mapping>
```
