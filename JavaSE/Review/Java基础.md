#Java基础

**1.Java中的几种基本数据类型是什么，各自占用多少字节？**

boolean(1),byte(8),short(16),char(16),int(32),float(32),long(64),double(64)

**2.String类能被继承吗？**

不能，String被声明为final，所以不可被继承

**3.String，Stringbuffer，StringBuilder的区别。**

String和Stringbuffer（内部使用synchronized进行同步)是线程安全的,StringBuilder不是。String不可变，Stringbuffer和StringBuilder可以修改
String可以赋空值，后两者不行

**4.ArrayList和LinkedList有什么区别。**

两者都实现了List()接口，有以下不同点：
* 1）ArrayList是基于索引的数据接口，底层是数组，它可以以O(1)时间复杂度对元素进行随机访问；LinkedList是以元素列表的形式存储它的数据，每一个元素和
    它的前一个，后一个元素链接在一起，查找某个元素的时间复杂度为O(n)
* 2)LinkedList的插入，添加，删除操作速度更快
* 3）LinkedList比ArrayList更占内存，因为它一个节点存储了两个引用，分别指向前一个和后一个元素

**5.讲讲类的实例化顺序，比如父类静态数据，构造函数，字段，子类静态数据，构造函数，字段，当new的时候，他们的执行顺序。**

父类静态数据，子类静态数据，父类字段，父类构造函数，子类字段，子类构造函数

**6.用过哪些Map类，都有什么区别，HashMap是线程安全的吗,并发下使用的Map是什么，他们内部原理分别是什么，
比如存储方式，hashcode，扩容，默认容量等。**

HashMap，ConcurrentHashMap，TreeMap，EnumMap，LinkedHashMap，WeakHashMap，IdentityHashMap

HashMap不是线程安全的。并发下使用的Map是ConcurrentHashMap

HashMap是数组+链表+红黑树（Java8加入了红黑树部分）

**7.JAVA8的ConcurrentHashMap为什么放弃了分段锁，有什么问题吗，如果你来设计，你如何设计。**


**8.有没有有顺序的Map实现类，如果有，他们是怎么保证有序的。**

TreeMap和LinkedHashMap是有序的，

**9.抽象类和接口的区别，类可以继承多个类么，接口可以继承多个接口么,类可以实现多个接口么。**

* 1）抽象类里面可以有方法实现，接口里只能有声明（在Java8里，已经可以有方法实现）
* 2）抽象类只能继承一个，接口可以实现多个
* 3）接口的方法必须默认为public，抽象类没有要求
* 4）接口的字段必须是static和final，抽象类没有要求
* 5）从设计层面看，抽象类是一种“IS-A”关系，需满足里式替换原则，即子类对象的访问权限必须大于或等于父类对象，以便能够替换掉父类对象；接口更像
    是一种Like-A关系

**10.继承和聚合的区别在哪。**

继承指的是一个类继承另一个类的功能，并可以增加它自己的新功能的能力。

聚合是关联关系的一种特例，是has-a的关系

**11.IO模型有哪些，讲讲你理解的nio ，他和bio，aio的区别是啥，谈谈reactor模型。**

IO是面向流的，NIO是面向缓存区的

**12.反射的原理，反射创建类实例的三种方式是什么。**

反射可以提供运行时的类信息

**13.反射中，Class.forName和ClassLoader区别 。**

Class.forName在loadClass后必须初始化，在执行过此方法后，目标对象的static块代码已经被执行，static参数也被初始化

ClassLoader.loadClass()方法，目标对象装载后不进行链接，不会执行该类静态块中的内容

**14.描述动态代理的几种实现方式，分别说出相应的优缺点。**

jdk底层是利用反射机制，需要基于接口方式，

cglib基于asm框架，实现了无反射机制进行代理，利用空间换来了时间，代理效率更高

**15。动态代理与cglib实现的区别。**

**16.为什么CGlib方式可以对接口实现代理。**

**17.final的用途。**

* 1）final声明类，类不能被继承
* 2）final声明方法，不能被子类重写
* 3）final声明字段，如果为基本类型，则该字段不能被改变，如果为引用类型，则该引用不能变，但引用的值可以变

**18.写出三种单例模式实现 。**

懒汉式单例，饿汉式单例，双重检查等

**19.如何在父类中为子类自动完成所有的hashcode和equals实现？这么做有何优劣。**

同时复写hashcode和equals方法，优点：可以添加自定义逻辑，且不必调用超类的实现

**20.请结合OO设计理念，谈谈访问修饰符public、private、protected、default在应用设计中的作用。**

作用域   | 当前类   |   同一包| 子孙类| 其他包
--------|---------|-------|-----|------
public  |是       |是     |是    |是
protected|是       |是    |是    |否
private |是         |否    |否    |否

default是在Java8中引入的，用来在接口中修饰方法，这样方法就可以有具体实现

**21.深拷贝和浅拷贝区别。**

浅拷贝：拷贝对象和原来的对象的引用类型相同，指向同一个对象

深拷贝：拷贝对象和原来的对象的引用类型不同

**22.数组和链表数据结构描述，各自的时间复杂度。**


**23.error和exception的区别，CheckedException，RuntimeException的区别。**

error描述了Java运行时系统的内部错误和资源耗尽错误，是JVM无法处理的错误；Exception

CheckedException是在

RuntimeException是在try-catch块中抛出的异常

**24.请列出5个运行时异常。**

IndexOutOfBoundsException，IllegalArgumentException，ArrayOutOfBoundsException，ArithmeticException，NumberFormatException

**25.在自己的代码中，如果创建一个java.lang.String类，这个类是否可以被类加载器加载？为什么。

不能，

**26.说一说你对java.lang.Object对象中hashCode和equals方法的理解。在什么场景下需要重新实现这两个方法。**


**27.在jdk1.5中，引入了泛型，泛型的存在是用来解决什么问题。**

泛型程序设计意味着编写的代码可以被多种不同类型的对象重用。泛型的本质是参数化类型，也就是说所操作的数据类型被指定为一个参数，好处是在编译的时候检查类型安全，并且所有的强制转换都是自动和隐式的，可以提供代码的重用率

**28.这样的a.hashcode() 有什么用，与a.equals(b)有什么关系。**

a.hashcode()可以用一个数字来标识对象

如果a.equals(b)为true，说明a和b的引用相同，那么它们的hashcode则也相同

**29.有没有可能2个不相等的对象有相同的hashcode。**

有可能，有可能2个不相等的对象有相同的引用，则有相同的hashcode

**30.Java中的HashSet内部是如何工作的。**

底层是基于HashMap实现的

**31.什么是序列化，怎么序列化，为什么序列化，反序列化会遇到什么问题，如何解决。**


**32.java8的新特性。**


