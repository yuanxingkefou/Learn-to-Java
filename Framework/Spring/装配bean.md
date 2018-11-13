#装配bean

**##自动化装配bean**

**组件扫描**

创建可被发现的bean

**@Component** 注解表明该类会作为组件类，并告知Spring要为这个类创建bean

**@Component("idName")** 注解为组件扫描的bean命名为name；也可以使用 **@Named("idName")**

**@ComponentScan** 注解启用了组件扫描；如果没有配置的话，会默认扫描与配置类相同的包以及包下的所有子包

**@ComponentScan("packageName")** 注解可以扫描packageName中的bean

**@ComponentScan(basePackages={"packageName1","packageName2"})**

**@ComponentScan(basePackageClasses={class1.class,class2.class}** 该注解会扫描class1,class2类所在的包中的bean

**自动装配**

**@Autowired**注解会实现自动装配，在Spring应用上下文中寻找匹配某个bean需求的其他bean

**@Autowired(required=false)** 将required属性设为false时，Spring会尝试执行自动装配，但是如果没有匹配的bean的话，
Spring会让这个bean处于未匹配的状态，可能会出现NullPointerException

**@Inject** 注解来源于Java依赖注入规范，可以和@Autowired在大多数情况下相互替换

**##通过Java代码装配bean（JavaConfig）**

**@Bean**注解会告诉Spring这个方法将会返回一个对象，该对象要注册为Spring引用上下文中的bean
```
该方法声明了CompactDisc bean:
@Bean
public CompactDisc sgtPeppers()
{
    return new SgtPeggers();
}
```
也可以通过name属性指定一个不同的名字@Bean(name="")

```
@Bean
实现注入
public CDPlayer cdPlayer()
{
    return new CDPlayer(sgtPeppers());
}
或者
public CDPlayer cdPlayer(CompactDisc compactDisc)
{
    return new CDPlayer(compactDisc);
}
```

**通过XML装配bean**
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean class="soundsystem.SgtPeppers" />   
</beans>
```
**借助构造器注入初始化bean**
1）<constructor-arg>元素

2）c-命名空间
无法装配集合

```
//构造器注入bean引用
<bean id="cdPlayer" class="soundsystem.CDPlayer">
    <constructor-arg ref="compactDisc" />
</bean>

<bean id="cdPlayer" class="soundsystem.CDPlayer"
        c:cd-ref="compactDisc" />
        
//将字面量注入到构造器中
<bean id="cdPlayer" class="soundsystem.CDPlayer">
        <constructor-arg value="Sgt..." />
        <constructor-arg value="The Beatles" />
</bean>

<bean id="cdPlayer" class="soundsystem.CDPlayer"
        c:_title="Sgt..."  
        c:_artit="The Beatle" />
        
//装配集合
<bean id="cdPlayer" class="soundsystem.CDPlayer">
        <constructor-arg>
            <list>
                <ref bean="">
                <ref bean="">
            </list>
        </constructor-arg>
</bean>

//设置属性
<bean id="cdPlayer" class="soundsystem.CDPlayer">
    <propert name="compactDisc" ref="compactDisc" />
</bean>

//p-命名空间
<bean id="cdPlayer" class="soundsystem.CDPlayer"
    p:compactDisc-ref="compactDisc"  />
```
![image]()

**导入和混合配置**
1）在JavaConfig中引用XML配置
使用@Import注解导入
使用@ImportResource注解

2）在XML配置中引用JavaConfig
<import resource="" />

