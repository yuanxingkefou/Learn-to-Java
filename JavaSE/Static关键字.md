#java中static关键字的作用：

1）**static 成员变量**

Java类提供了两种类型的变量：用static修饰的静态变量和不用static关键字修饰的实例变量

静态变量属于类，在内存中只有一个复制，只要静态变量所在的类被加载，这个静态变量就会被分配空间。对静态变量的引用有两种方式，分别是“类 . 静态变量”，“对象 . 静态变量”

实例变量属于对象，只有对象被创建后，实例变量才会被分配空间，才能被使用，在内存中存在多个复制，只有用“对象 . 实例变量”的方式来引用

注：不能在成员函数内部定义static变量

2）**static 成员方法**

static方法是类的方法，不需要创建对象就可以被调用；非static方法是对象的方法，只有对象被创建出来后才可以被调用

static方法不能使用this和super关键字，不能调用非static方法，不能访问非static类型的变量

3）**static 代码块**

static代码块在类中是独立与成员变量和成员函数的代码块的。这些static代码块只会被执行一次

4）**static 与final结合使用：**

对于变量，用static final修饰，表示一旦赋值不能修改，并且可以通过类名访问

对于方法，用static final修饰，表示该方法不可以被覆盖，并且可以通过类名直接访问
