#Java访问指示符：friendly,public ,private,protected

1）**friendly**：包中有效，即当前包内的所有方法可以访问，但包外的不可以

如果不指定访问指示符的话，默认就是friendly

2）**public**：接口访问

在public后面的成员声明适用于所有地方的类内部，方法内部访问此对象方法

3）**private**：除非那个特定的类，而且从那个类的方法里，否则没有人能访问那个成员

4）**protected**：包内和子类可以访问

protected和friendly的区别：

若子类和父类不在同一个包中，子类只会继承public和protected类型，不会继承friendly类型

![image](https://github.com/yuanxingkefou/Learn-to-Java/blob/master/JavaSE/JavaAccessModifiers.png
)

**下列修饰符不能在方法体中使用：**

访问修饰符：private,protected,public

静态修饰符：static

抽象：abstract


