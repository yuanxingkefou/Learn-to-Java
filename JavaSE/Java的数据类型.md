#Java的数据类型

Java的数据类型主要分为两大类：基本数据类型和引用数据类型

**##基本数据类型**

* 整型
  byte(8),short(16),int(32),long(64)
* 浮点类型
  float(32),double(64)
* 字符类型
  char(16)
* 枚举类型
  boolean(1)
  
  引用类型可分为：数组，类和接口
  
**##基本数据类型的自动转换**
  
  ![image]()
  
* boolean类型与其他基本类型不能进行类型的转换
  
* Java中，未声明的整型，其默认类型为int；未声明的浮点类型，其默认类型为double  
  
* 当将一个数值范围小的类型赋给一个数值范围大的类型时，jvm在编译过程中将此值的数值类型进行了自动提升，在数值类型的自动提升过程中，
    数值精度至少不应该降低
  
* jvm在编译过程中，对于默认为int类型的数值时，当赋给一个比int型数值范围小的数值类型变量（在此统一称为数值类型k，k可以是byte/char/short类型），
  会进行判断，如果此int型数值超过数值类型k，那么会直接编译出错。因为你将一个超过了范围的数值赋给类型为k的变量，
  你有没有进行强制类型转换，当然报错了。但是如果此int型数值尚在数值类型k范围内，jvm会自动进行一次隐式类型转换，
  将此int型数值转换成类型k。如图中的虚线箭头
  
```
  public class TypeConvert {
	public static void main(String[] args)
	{
		byte a=128;	//cannot convert from int to byte
    byte d=127; //编译成功，因为byte有八位，大小范围为-128-127,所以127可以有隐式的转换，128没有
		float b=1.5;	//cannot convert from double to float
		byte c=3;
	}
}
```

* char型其本身是unsigned型，同时具有两个字节，其数值范围是0 ~ 2^16-1，因为负数的问题，这直接导致byte型不能自动类型提升到char，
  char和short直接也不会发生自动类型提升
  
**##基本数据类型的强制转换**

当我们将数值范围大的数赋给数值范围小的数时，可能会有精度的丢失，所以要进行强制转换（上面所说的int隐式转换除外）

Java中，二进制数值是以补码表示的，所以，在赋值后，对数值范围大的数进行截断，得到的是补码。如：

byte  a=223;

223的二进制表示为0000 0000 0000 0000 0000 0000 1110 1001

转换为byte后，变为8位，即1110 1001

因为补码为1110 1001,原码为1001 0111,1为符号位

所以转换后的数字为-23

```
public class TypeConvert {
	public static void main(String[] args)
	{
		byte p=3;	         //编译正确：int到byte的隐式直接转换
		int a=3;
		byte b=a;	        //编译错误
    byte c=(byte)a;   //编译正确
    /*
      区别在于3是直接量，在编译期间可以直接进行判别，后者a为一变量，需要到运行期间才能进行判定，所以，编译期间为以访万一，需要进行
      强制类型转换
    */
	}
}
```

**##进行数值运算时，可能进行的类型转换和强制类型转换**

当进行数学运算时，数据类型会自动发生提升到运算符左右之较大者，以此类推。当将最后的运算结果赋值给指定的数值类型时，可能需要进行强制类型转换。

```
public class TypeConvert {
	public static void main(String[] args)
	{
		byte e=10,f=11;
		byte m=e+f;		//cannot convert from int to byte,+直接将10和11提升到了int类型
    byte m=(byte)(e+f);
	}
}
```
