#Groovy

**##语法和语义**

* 默认导入

  Groovy会默认导入一些语言包和工具包，以提供基本的语言支持，也可以使用import语句
  
* 数字处理

  Groovy在底层使用Java的BigDecimal表示浮点数
  
* 变量，动态与静态类型，作用域

  绑定域：脚本的全局作用域
  
  本地域：变量的作用域局限于它们的代码块
  
  ```
  //这里会抛出异常，由于作用域的问题
  String hello="Hello";
  void checkHello()
  {
      System.out.println(hello);
  }
  checkHello();
  ```

* 列表和映射语法

  底层是由Java ArrayList和LinkedHashMap实现的，用[]指定和使用列表结构
  ```
  array=["Java","Groovy","Scala"];
  println(array[0]);
  println(array.size());
  arr=[];
  println(arr);
  输出：
  Java
  3
  []
  
  array=[Java:100,Groovy:99,Clojure:"N/A"];
  println(array["Java"]);
  println(array.Clojure);
  array["Clojure"]=75;
  println(array.Clojure);
  arr=[:];
  println arr;
  
  输出：
  100
  N/A
  75
  [:]
  
  array=["Java","Scala","Groovy"];
  println(array[0..2]);
  println(array[-1]);

  输出：
  [Java,Scala,Groovy]
  Groovy
  ```
  
**##与Java的差异**

* 可选的分号和返回语句  Groovy会自动返回最后一个表达式的计算结果

* 可选的参数括号
  `println "Hello,world"`
  
* 访问限定符   
  Groovy的默认访问限定符是public

* 异常处理    
  
  不区分已检查异常和未检查异常，会忽略方法签名中的所有throws语句

* 相等    
  
  Groovy把==当做Java中的equals()方法

* 内部类   
  
  Groovy支持内部类，但是大多数情况下，用函数字面值替代它。

**##Java不具备的Groovy特性**

* GroovyBean    
  
  省略了JavaBean显示声明的获取和设置方法，提供了自动构造方法，并可以用点号(.)引入成员变量

* 安全解引用操作符    
  
  用?.操作符去掉一些套路化的“如果对象为null”检查代码，实现对null对象的安全访问
  ```
  class Person
  {
     private String name;
    
  }
  void test()
  {
    Person person=null;
    def name=person?.name
    //下面这种表达会出现NullPointerException
    //def name=person.name
    println('person:'+person+',name:'+name)
  }
  test()
  ```
  
* ?:操作符    
  可以把带有默认值的if/else结构写得及其短小，不用检查null,也不用重复变量。其实就是三元操作符的简写
```
String agentStatus="Active";
//Java中的三元操作符写法
//String status=agentStatus!=null?agentStatus:"Inactive"
String status=agentStatus ?: "Inactive"
输出：
Active
```

* 增强型字符串
  
在Groovy中，普通字符串String一般是用单引号定义的，双引号也可以；而GString必须用双引号定义，它可以包含可在运行时计算的表达式（用${}），转为普通字符
串后，表达式会被替换为其计算结果

注：String用双引号来定义时就是GString，不一定非要使用GString关键字
```
String name='John'
def dist=3*2
GString crawling1="${name} is crawling ${dist} feet!"
String crawling2="${name} is crawling ${dist} feet!"
String crawling3='${name} is crawling ${dist} feet!'
println crawling1+"\n"+crawling2+"\n"+crawling3
//三引号String可以在源码中定义跨行字符串
"""This GString
     wraps over two lines!"""
     
输出：
John is crawling 6 feet!
John is crawling 6 feet!
${name} is crawling ${dist} feet!
Result: This GString
     wraps over two lines!
```

* 函数字面值
```
def sayHello=
{    
    name->
        if(name=="Martijn"||name=="Ben")
            "Hello author "+name
        else
            "Hello reader "+name
}
println(sayHello("Martijn"))
```

* 内置的集合操作

Groovy中的部分集合函数

方法        |描述
------------|-----------------------
each    |遍历集合，对表中的每一项应用函数字面值
collect |收集在集合中每一项上应用函数字面值的返回结果
inject  |用函数字面值处理集合并构建返回值
findAll |找到集合中所有与函数字面值匹配的元素
max     |返回集合中的最大值
min     |返回集合中的最小值

```
movie=["Seven","SnowWhite","Die Hard"]
movie.each({x->println x})
//单参的函数字面值中隐含的it变量
movie.each({println it})
```

* 对正则表达式的内置支持

Groovy正则表达式语法

方法      |描述               |Java中的对等物
---------|-------------------|-----------
~ |创建一个模式 |创建一个编译的Java pattern对象
=~|创建一个匹配器|创建一个Java Matcher对象
==~|计算字符串 |相当于在Pattern上调用Java match()方法

* 简单的XML处理
```
可以用Groovy产生XML
def writer=new StringWriter()
def xml=new groovy.xml.MarkupBuilder(writer)
xml.person(id:2)
{
    name 'Gweneth'
    age 1
}
println writer.toString()
输出：
<person id='2'>
  <name>Gweneth</name>
  <age>1</age>
</person>

//解析XML
class XMLExample
{
    static def PERSON=
    """
    <person id='2'>
        <name>Gweneth</name>
        <age>1</age>
    </person>
    """
 }
 class Person
 {
     def id;
     def name;
     def age;
 }
 
 def xmlPerson=new XmlParser().parseText(XMLExample.PERSON)
 Person p=new Person(id:xmlPerson.@id,
                     name:xmlPerson.name.text(),
                     age:xmlPerson.age.text())
 println "${p.id},${p.name},${p.age}"
 输出：
 2,Gweneth,1
```

**##Groovy与Java的合作**

从Java调用Groovy

需要把Groovy及其相关的JAR放到这个程序的CLASSPATH下

* 使用Bean Scripting Framework（BSF），即JSR 223

* 使用GroovyShell

  在临时性快速调用Groovy并计算表达式或类似于脚本的代码时，可以用GroovyShell
  ```
  import groovy.lang.GroovyShell;
  import groovy.lang.Binding;
  import java.math.BigDecimal;
  public class UseGroovyShell 
  {
	  public static void main(String[] args)
	  {
		  Binding binding=new Binding();
		  binding.setVariable("x", 2.4);
		  binding.setVariable("y", 8);
		
		  GroovyShell shell=new GroovyShell(binding);
		  Object value=shell.evaluate("x+y");
		  System.out.println(value.toString());
		  assert value.equals(new BigDecimal(10.4));
	  }
  }

  ```
* 使用GroovyClassLoader
  
  先创建一个Groovy类文件，然后通过GroovyClassLoader获取这个类的实例来使用。在调用Groovy中的一些实用类时使用
  ```
  public class UseGroovyClassLoader 
  {
	  public static void main(String[] args)
	  {
		  //准备GroovyClassLoader
		  GroovyClassLoader loader=new GroovyClassLoader();
		
		  try
		  {
			  //得到Groovy类
			  Class<?> groovyClass=loader.parseClass(new File("CalculateMax.groovy"));
			  //得到Groovy类的实例
			  GroovyObject groovyObject=(GroovyObject)groovyClass.newInstance();
			
			  //准备参数
			  ArrayList<Integer> numbers=new ArrayList<>();
			  numbers.add(new Integer(1));
			  numbers.add(new Integer(10));
			
			  Object[] arguments={numbers};
			  Object value=groovyObject.invokeMethod("getMax", arguments);
			  assert value.equals(new Integer(10));
			  System.out.println(value.toString());
		  }catch(CompilationFailedException | IOException | InstantiationException 
				  | IllegalAccessException e)
		  {
			  e.printStackTrace();
		  }
	  }
  }

  ```
* 使用GroovyScriptEngine
  
  要指明Groovy代码的URL或所在目录。Groovy脚本引擎会加载那些脚本，并在必要时进行编译，包括其中的依赖脚本。使用大量Groovy代码时使用
  ```
  public class UseGroovyScriptEngine 
  {
	  public static void main(String[] args)
	  {
		  try
		  {
			  String[] roots=new String[]{"D:\\ecplise\\java\\GroovyTest\\src\\groovy"};
			  GroovyScriptEngine gse=new GroovyScriptEngine(roots);
			
			  Binding binding=new Binding();
			  binding.setVariable("name", "John");
			
			  Object output=gse.run("Hello.groovy", binding);
			  assert output.equals("Hello John");
			  System.out.println(output.toString());
		  }catch(IOException | ResourceException | ScriptException e)
		  {
			  e.printStackTrace();
		  }
	  }
  }
  ```
  


