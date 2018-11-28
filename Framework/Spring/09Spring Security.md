#Spring Security

基于Spring的应用程序提供声明式安全保护的安全性框架。

最新版本从两个角度来解决安全性问题。一是使用Servlet规范中的Filter保护web请求并限制url级别的访问。二是使用Spring AOP保护方法调用，能够确保只有具备
适当权限的用户才能访问安全保护的方法

Spring Security的模块

![image]()

1)过滤web请求:DelegatingFilterProxy把Filter的处理逻辑委托给Spring应用

```
//在传统的web.xml中配置Servlet和Filter
<filter>
    <filter-name>springSecurityFilterChain</filter-name>
    <filter-class>
        org.springframework.web.filter.DelegatingFilterProxy
    </filter-class>
</flter>

//用Java的方式配置
public class SecurityWebInitializer extends AbstractSecurityWebApplicationInitializer {}

```

编写简单的安全性配置
```
//使用@EnableWebSecurity注解启用Web安全性
@Configuration
@EnableWebSecurity
public class SecutiryConfig extends WebSecurityConfigureAdapter {}

//使用@EnableWebMvcSecurity注解启用SpringMVC安全性
@Configuration
@EnableWebMvcSecurity
public class SecutiryConfig extends WebSecurityConfigureAdapter {}

```

##选择查询用户详细信息的服务

```
//使用基于内存的用户存储
@Configuration
@EnableWebMvcSecurity
public class SecutiryConfig extends WebSecurityConfigureAdapter 
{
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception
    {
        auth
          .inMemoryAuthentication()
              .withUser("user").password("password").roles("USER").and()
              .withUser("admin").password("password").roles("USER","ADMIN");
    }
}
//基于数据库表进行认证
@Autowired
DataSource datasource

@Override
protected void configure(AuthenticationManagerBuilder auth) throws Exception
{
    auth
      .jdbcAuthentication()
        .dataSource(datasource);
}

//基于LDAP进行认证
@Override
protected void configure(AuthenticationManagerBuilder auth) throws Exception
{
    auth
      .ldapAuthentication()
        .userSearchFilter("(uid={0})")
        .groupSearchFilter("member={0}");
}

```

![image]()

##拦截请求

##认证用户

启用HTTP Basic认证

启用Remember-me功能

##保护视图

