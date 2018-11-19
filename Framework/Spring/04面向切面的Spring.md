#面向切面的Spring

定义：切面提供了取代继承和委托的另一种可选方案，在使用面向切面编程时，仍然在一个地方定义通用功能，但是可以通过声明的方式定义这个功能要以何种方式在
何处应用，而无需修改受影响的类。横切关注点可以被模块化为特殊的类，被称为切面。

**##AOP术语**

**通知(Advice)**：切面的工作，定义了切面是什么以及何时使用。

前置通知(Before)，后置通知(After)，返回通知(After-returning)，异常通知(After-throwing)，环绕通知(Around)

**切点(pointcut)**:切点的定义会匹配通知所要织入的一个或多个连接点，有助于缩小切面所通知的连接点的范围

**连接点(joinpoint)**:在应用执行过程中能够插入切面的一个点。可以是调用方法时，抛出异常时等。

**切面(Aspect)**:通知和切点的结合。是什么，在何时和何处完成其功能

**引入(Introduction)**:允许我们在不修改现有类的情况下，向其中添加新方法或属性

**织入(Weaving)**：把切面应用到目标对象并创建新的代理对象的过程

在目标对象的生命周期里有多个点可以进行织入：编译期，类加载期，运行期

**##通过切点来选择连接点**

![image](https://github.com/yuanxingkefou/Learn-to-Java/blob/master/Framework/Spring/AspectJ.png)

切点表达式：

![image](https://github.com/yuanxingkefou/Learn-to-Java/blob/master/Framework/Spring/pointcut.png)

使用within()指示器限制切点范围：
`execution(* concert.Performance.perform(..)) && within(concert.*)`

'&&'可以用and替换；'||'可以用or替换；'!'可以用not替换

Spring还引入了一个新的bean()指示器:`execution(* concert.Performance.perform() ) and bean('beanName/beanId')`

**##使用注解创建切面**

通知注解：@After，@AfterReturning，@AfterThrowing，@Around，@Before

使用 **@Pointcut** 注解声明频繁使用的切点表达式

在JavaConfig中使用 **@EnableAspectJAutoProxy** 启用自动代理功能

在XML中使用`<aop:aspectj-autoproxy>`元素

创建环绕通知：

```
@Aspect
public class Audience
{
    @Pointcut("")
    public void performance()
    
    @Around("performance()")
    public void watchPerformance(ProceedingJoinPoint jp)
    {
        try
        {
            System.out.println("Before");
            jp.proceed();
            System.out.println("After");
        }catch(Exception e)
        {
            System.out.println("AfterThrowing");
        }
    }
```

通过注解引入新功能：

使用 **@DeclareParents**注解，有三部分组成：
* value属性指定了哪种类型的bean要引入该接口
* defaultImpl属性指定了为引入功能提供实现的类
* 所标注的静态属性指明了要引入的接口

**##在XML中声明切面**

![image](https://github.com/yuanxingkefou/Learn-to-Java/blob/master/Framework/Spring/SpringAop.png)

```
<aop:config>
  <aop:aspect ref="audience">
  
    <aop:pointcut
         id="performance"
         expression="execution("" ...)" />
         
     //通过切面引入新的功能
    <aop:declare-parents
        type-matching="concert.Performance+"
        implement-interface="concert.Encoreable"
        default-impl="concert.DefaultEncoreable"  />
        
    <aop:before
          pointcut=""
          method="" />
    
  </aop:aspect>
</aop:config>
```

**##注入AspectJ切面**

AspectJ提供了SpringAOP所不能支持的许多类型的切点。

在bean中使用了属性factory-method属性：`<bean class="" factory-method="aspectOf">
