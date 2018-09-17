#web应用安全

**Servlet安全：认证authentication，授权authorization，机密性confidentiality和数据完整性**

**1）启用认证**

在DD中配置
```
<login-config>
    <auth-method>BASIC</auth-method>
</login-config>
```

**四种认证类型：**

1.基本认证(BASIC)

2.摘要（DIGEST）

3.客户证书（CLENT-CERT)

4.表单(FORM)


**2）授权**

##1.定义角色

**tomcat-users.xml中的<role>元素：**
```
<tomcat-users>
    <role rolename="Admin"/>
    <role rolename="Member"/>
    <role rolename="Guest">
    <user username="John" password="admin" roles="Admin,Member,Guest" />
    <user username="Ted" password="coder" roles="Member" />
</tomcat-users>
```

**web.xml中的DD<security-role>元素**
```
<security-role>
    <role-name>Admin</role-name>
</security-role>
<security-role>
    <role-name>Member</role-name>
</security-role>
<security-role>
    <role-name>Guest</role-name>
</security-role>
```

##2.定义资源/方法约束

**DD中的<security-constraint>元素：**
```
<web-app...>
    ...
    <security-constraint>
        <web-resource-collection>
            //必要的名字
            <web-resource-name>UpdateRecipes</web-resource-name>
            
            //定义要收约束的资源
            <url-pattern>/Beer/AddRecipe/*</url-pattern>
            //描述哪些方法是受约束的（如果没有指定，那么所有方法都是受约束的）
            <http-method>GET</http-method>
        <web-resource-collection>
        
        //列出了哪些角色可以调用受约束的http方法
        //没有<auth-constraint>意味着所有人都能访问
        //<auto-constrant/>则意味者没有人能访问
        <auth-constraint>
            <role-name>Admin</role-name>
            <role-name>Member</role-name>
        </auth-constraint>
    </security-constraint>
</web-app>
```

定制方法：

在HttpServletRequest中，有3个方法和程序式安全有关：

1）getUserPrincipal()，用于EJB

2）getRemoteUser(),用于检查认证状态

3）isUserInRole()，根据用户的角色对方法中的某些部分建立授权

以声明方式保守地实现数据完整性和机密性：
```
<security-constraint>
    ...
    <user-data-constraint>
        <transport-guarantee>    </transport-guarantee>
    </user-data-constraint>
</security-constraint>
```
<transport-guarantee>的合法值：

NONE：意味着没有数据保护

INTEGRAL：数据在传输过程中不能更改

CONFIDENTIAL：数据在传输过程中不能被别人看到
