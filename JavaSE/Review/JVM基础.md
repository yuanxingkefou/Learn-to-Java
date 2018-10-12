#JVM基础

**1.什么情况下会发生栈内存溢出。**

**2.JVM的内存结构，Eden和Survivor比例。**

**3.JVM内存为什么要分成新生代，老年代，持久代。新生代中为什么要分为Eden和Survivor。**

**4.JVM中一次完整的GC流程是怎样的，对象如何晋升到老年代，说说你知道的几种主要的JVM参数。**

**5.你知道哪几种垃圾收集器，各自的优缺点，重点讲下cms和G1，包括原理，流程，优缺点。**

**6.垃圾回收算法的实现原理。**

**7.当出现了内存溢出，你怎么排错。**

**8.JVM内存模型的相关知识了解多少，比如重排序，内存屏障，happen-before，主内存，工作内存等。**

**9.简单说说你了解的类加载器，可以打破双亲委派么，怎么打破。**

**10.讲讲JAVA的反射机制。**

**11.你们线上应用的JVM参数有哪些。**

**12.g1和cms区别,吞吐量优先和响应优先的垃圾收集器选择。**

**13.怎么打出线程栈信息。**

**14.请解释如下jvm参数的含义：**
```
-server -Xms512m -Xmx512m -Xss1024K
-XX:PermSize=256m -XX:MaxPermSize=512m -
XX:MaxTenuringThreshold=20XX:CMSInitiatingOccupancyFraction=80 -
XX:+UseCMSInitiatingOccupancyOnly。
```
