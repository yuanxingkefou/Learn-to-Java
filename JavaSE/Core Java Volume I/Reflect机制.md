#反射机制：

定义：指的是可以于运行时加载，探知，使用编译期间完全未知的类。

程序在运行状态中，可以动态加载一个只有名称的类，对于任意一个已加载的类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意

一个方法和属性

`Class c=Class.forName("com.test.User")`

加载完类后，在堆内存中，就产生了一个Class类型的对象（一个类只有一个Class对象），这个对象就包含了完整的类的结构信息。

**能够分析类能力的程序称为反射**

**反射机制可以用来：**

1）在运行中查看类的能力，动态加载类，获取类的信息
```
/**
 * 应用反射的API，获取类的信息（类的名字，方法，属性，构造器等）
 * @author ASUS
 *
 */
public class ReflectionDemo 
{
	public static void main(String[] args) 
	{
		String path="com.test.User";
		
		try {
			Class user=Class.forName(path);
			
			//获取类的名字
			/*
			 * 输出：com.test.User
			 *		User
			 */
			System.out.println("--------获取类的名字-----------");
			System.out.println(user.getName());
			System.out.println(user.getSimpleName());
			
			/**
			 * 获取属性信息
			 */
			System.out.println("--------获取类的属性信息-----------");
			Field[] fields=user.getFields();	//只能获得public的field
			for(Field temp:fields)
			{
				System.out.println("public属性："+temp);
			}
			Field[] fields2=user.getDeclaredFields();	//获得所有field
			for(Field temp:fields2)
			{
				System.out.println("全部属性："+temp);
			}
			/**
			 * 获取方法(Method)信息（类似与属性信息）
			 */
			try {
				//如果方法有参数，则必须传递参数类型所对应的Class对象，用来识别重载的方法
				Method m=user.getDeclaredMethod("setName", String.class);
			} catch (NoSuchMethodException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			} catch (SecurityException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
			/**
			 * 获取构造器(Constructor)信息(与方法，属性类似)
			 */
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		}
		
	}

}

```

2）动态构造对象，调用类和对象的任意方法，调用和处理属性
```
public class RefletionDemo02 
{
	public static void main(String[] args) 
			throws ClassNotFoundException, InstantiationException, 
			IllegalAccessException, NoSuchMethodException, SecurityException 
	{
		String path="com.test.User";

		Class c=Class.forName(path);
		
		/**
		 * 调用构造方法，构造对象
		 */
		User u1=(User)c.newInstance(); 			//相当于使用无参构造器
		
		try {
			Constructor<User> c1=c.getDeclaredConstructor(int.class,int.class,String.class);
			//调用有参数的构造器
			User u2=c1.newInstance(1001,15,"John");
			//输出：John
			System.out.println(u2.getName());
		} catch (NoSuchMethodException | SecurityException e1) {
			// TODO Auto-generated catch block
			e1.printStackTrace();
		} catch (IllegalArgumentException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (InvocationTargetException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
		/**
		 * 通过反射API，调用普通方法
		 */
		User u3=(User)c.newInstance();
		Method method=c.getDeclaredMethod("setName", String.class);
		try {
			method.invoke(u3, "Tom");			//u3.setName("Tom");
			System.out.println(u3.getName());
		} catch (IllegalArgumentException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (InvocationTargetException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}			
		/**
		 * 利用反射的API，修改类的属性
		 */
		try {
			User u4=(User)c.newInstance();
			Field f=c.getDeclaredField("name");
			/**
			 * 报错：不能访问私有属性
			 * f.set(u4, "Tim");
			 */
			//解决方法：使用setAccessible(true);使该属性不需要进行安全检查，可以直接访问
			f.setAccessible(true);
			f.set(u4, "test4");
			System.out.println(u4.getName());
		} catch (NoSuchFieldException e2) {
			// TODO Auto-generated catch block
			e2.printStackTrace();
		}
	}

}

```

3）反射操作泛型

Java采用泛型擦除的机制来引入泛型，但是，一旦编译完成，所有的和泛型有关的数据类型全部擦除。

为了通过反射操作这些类型以迎合实际开发的需要，Java就新增了以下几种类型来代表不能归一到Class中的类型但是又和原始类型齐名的类型

* ParameterizedType:表示一种参数化的类型，比如Collection<String>

* GenericArrayType:表示一种元素类型是参数化类型或者类型变量的数据类型

* TypeVariable:是各种类型变量的公共父接口

* WildcardType:代表一种通配符表达式,如? extends Number等
```
/**
 * 通过反射获取泛型信息
 * @author ASUS
 *
 */
public class ReflectionDemo03 
{
	public static void main(String[] args) 
	{
		try {
			//获得指定方法参数泛型信息
			Method m=ReflectionDemo03.class.getMethod("test01", Map.class,List.class);
			Type[] t=m.getGenericParameterTypes();
			
			for(Type paramType:t)
			{
				System.out.println("#"+paramType);
				
				if(paramType instanceof ParameterizedType)
				{
					Type[] genericTypes=((ParameterizedType) paramType).getActualTypeArguments();
					for(Type genericType: genericTypes)
					{
						System.out.println("泛型类型："+genericType);
					}
				}
			}
			System.out.println("---------------------------");
			//获得指定方法返回值泛型信息
			Method m2=ReflectionDemo03.class.getMethod("test02", null);
			Type returnType=m2.getGenericReturnType();
			if(returnType instanceof ParameterizedType)
			{
				Type[] genericTypes=((ParameterizedType) returnType).getActualTypeArguments();
				for(Type genericType: genericTypes)
				{
					System.out.println("返回值的泛型类型："+genericType);
				}
			}
			
			
		} catch (NoSuchMethodException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (SecurityException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
		
	}
	
	public void test01(Map<String,User> map,List<User> list)
	{
		System.out.println("Demo03.test01");
	}
	
	public Map<Integer,User> test02()
	{
		System.out.println("Demo03.test02");
		return null;
	}
}
输出：
#java.util.Map<java.lang.String, com.test.User>
泛型类型：class java.lang.String
泛型类型：class com.test.User
#java.util.List<com.test.User>
泛型类型：class com.test.User
---------------------------
返回值的泛型类型：class java.lang.Integer
返回值的泛型类型：class com.test.User

```

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



