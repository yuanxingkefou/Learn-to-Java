#反射机制：

定义：指的是可以于运行时加载，探知，使用编译期间完全未知的类。

程序在运行状态中，可以动态加载一个只有名称的类，对于任意一个已加载的类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意

一个方法和属性

`Class c=Class.forName("com.test.User")`

加载完类后，在堆内存中，就产生了一个Class类型的对象（一个类只有一个Class对象），这个对象就包含了完整的类的结构信息。

**能够分析类能力的程序称为反射**

**反射机制可以用来：**

1）在运行中查看类的能力

2）在运行中查看对象，

3）实现通用的数组操作代码

4）利用Method对象，这个对象很向C++中的函数指针

Class类

利用反射分析类的能力：

在java.lang.reflect包中有三个类Filed,Method,Constructor分别用于描述类的域，方法和构造器

这三个类还有一个**getModifiers**的方法，它将返回一个整形数值，用不同的位开关描述public和static这样的修饰符使用状况

```
package reflection;

import java.util.*;
import java.lang.reflect.*;

/**
 * This program users reflection to print all features of a class.
 * @author MQ
 * @version 2018-9-21
 */
public class ReflectionTest {
	public static void main(String[] args)
	{
		String name;
		if(args.length>0)
			name=args[0];
		else
		{
			Scanner in=new Scanner(System.in);
			System.out.println("Enter class name(eg: java.util.Date):");
			name=in.next();
		}
		
		try{
			Class cl=Class.forName(name);
			Class supercl=cl.getSuperclass();
			
			String modifiers=Modifier.toString(cl.getModifiers());
			if(modifiers.length()>0)
				System.out.print(modifiers+" ");
			
			System.out.print("class "+name);
			
			if(supercl!=null&&supercl!=Object.class)
				System.out.print(" extends "+supercl.getName());
			
			System.out.println("\n{\n");
			
			System.out.println("  Constructors:");
			printConstructors(cl);
			System.out.println();
			
			System.out.println("  Methods:");
			printMethods(cl);
			System.out.println();
			
			System.out.println("  Fields:");
			printFields(cl);
			System.out.println("}");
		}catch(ClassNotFoundException e)
		{
			e.printStackTrace();
		}
		System.exit(0);
	}
	/**
	 * Prints all constructors of a class
	 */
	public static void printConstructors(Class cl)
	{
		Constructor[] constructors=cl.getDeclaredConstructors();
		
		for(Constructor c: constructors)
		{
			String name=c.getName();
			System.out.print("  ");
			String modifiers=Modifier.toString(c.getModifiers());
			if(modifiers.length()>0)
				System.out.print(modifiers+" ");
			System.out.print(name+"(");
			
			Class[] paramTypes=c.getParameterTypes();
			for(int j=0;j<paramTypes.length;j++)
			{
				if(j>0)
					System.out.print(", ");
				System.out.print(paramTypes[j].getName());
			}
			System.out.println(");");
		}
	}
	/**
	 * Prints all methods of a class
	 */
	public static void printMethods(Class cl)
	{
		Method[] methods=cl.getDeclaredMethods();
		
		for(Method m:methods)
		{
			Class retType=m.getReturnType();
			String name=m.getName();
			
			System.out.print("  ");
			String modifiers=Modifier.toString(m.getModifiers());
			if(modifiers.length()>0)
				System.out.print(modifiers+" ");
			System.out.print(retType.getName()+" "+name+"(");
			
			Class[] paramTypes=m.getParameterTypes();
			for(int j=0;j<paramTypes.length;j++)
			{
				if(j>0)
					System.out.print(", ");
				System.out.print(paramTypes[j].getName());
			}
			System.out.println(");");
		}
	}
	/**
	 * Prints all fields of a class
	 */
	public static void printFields(Class cl)
	{
		Field[] fields=cl.getDeclaredFields();
		
		for(Field f:fields)
		{
			Class type=f.getType();
			String name=f.getName();
			System.out.print("  ");
			String modifiers=Modifier.toString(f.getModifiers());
			if(modifiers.length()>0)
				System.out.print(modifiers+" ");
			System.out.println(type.getName()+" "+name+";");
		}
	}
}
```

输入及运行结果：
```
Enter class name(eg: java.util.Date):
java.lang.Double
public final class java.lang.Double extends java.lang.Number
{

  Constructors:
  public java.lang.Double(double);
  public java.lang.Double(java.lang.String);

  Methods:
  public boolean equals(java.lang.Object);
  public static java.lang.String toString(double);
  public java.lang.String toString();
  public int hashCode();
  public static int hashCode(double);
  public static double min(double, double);
  public static double max(double, double);
  public static native long doubleToRawLongBits(double);
  public static long doubleToLongBits(double);
  public static native double longBitsToDouble(long);
  public volatile int compareTo(java.lang.Object);
  public int compareTo(java.lang.Double);
  public byte byteValue();
  public short shortValue();
  public int intValue();
  public long longValue();
  public float floatValue();
  public double doubleValue();
  public static java.lang.Double valueOf(java.lang.String);
  public static java.lang.Double valueOf(double);
  public static java.lang.String toHexString(double);
  public static int compare(double, double);
  public static boolean isNaN(double);
  public boolean isNaN();
  public static boolean isFinite(double);
  public static boolean isInfinite(double);
  public boolean isInfinite();
  public static double sum(double, double);
  public static double parseDouble(java.lang.String);

  Fields:
  public static final double POSITIVE_INFINITY;
  public static final double NEGATIVE_INFINITY;
  public static final double NaN;
  public static final double MAX_VALUE;
  public static final double MIN_NORMAL;
  public static final double MIN_VALUE;
  public static final int MAX_EXPONENT;
  public static final int MIN_EXPONENT;
  public static final int SIZE;
  public static final int BYTES;
  public static final java.lang.Class TYPE;
  private final double value;
  private static final long serialVersionUID;
}
```



