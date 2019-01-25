[image](https://github.com/yuanxingkefou/Learn-to-Java/blob/master/JavaSE/String.gif)

**#String**

String是不可变字符串，因为String类被final修饰，所以不可被继承。

String类既是一个对象，也是一个常量，一旦被初始化，就不能被修改。String类的方法对字符进行操作后返回的新字符串与原字符串是两个串不同的对象，因此比较
浪费空间。

"==":如果比较的是基本类型，则是判断它们的值是否相等，如果比较的是两个对象变量，则是判断它们引用的是否指向同一个对象

"equals"：判断的是对象引用的值是否相等，所以当要比较两个字符串是否相同时，使用equals.

**构造**

* 直接赋值

```
String s1="abc";
String s2="abc";
```
先在栈中创建一个对String类的对象引用变量s1，然后去字符串常量池找有没有"abc",如果没有，则将”abc"存放进字符串常量池，并将s1指向“abc”，如果已经有
“abc”，则直接令其指向"abc"

* 用new创建对象

```
String s3="abc";
String s4="abc";
```

存放于堆中，每调用一次就会创建一个新的对象，不论其字符串的值是否已经在堆中存在

**#StringBuffer**

StringBuffer是一种长度可变的容器，也称为“字符串缓冲区”，是Java中另一种字符串类，也被final修饰，同样不能被继承

StringBuffer的对象是字符串变量，它可以扩充和修改，是动态可变的

StringBuffer在进行字符串处理时，不生成新的对象，在内存上要优于String类

**构造**

* StringBuffer();

* StringBuffer(int length);

* StringBuffer(String str);

**#StringBuilder**

具有与StringBuffer兼容的API，是StringBuffer的一个简易替换，但不保证同步，因此速度要更快一点

**三者区别：**

* 运行速度：
  StringBuilder>StringBuffer>String
  
* String和StringBuffer是线程安全的，StringBuilder是线程不安全的

