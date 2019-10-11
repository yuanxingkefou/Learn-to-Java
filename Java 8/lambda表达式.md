**#Lambda表达式**

**##格式**

(Parameters) -> Expression
或
(Parameters) -> { Statements; }
先前：
```
Comparator<Apple> byWeight =new Comparator<Apple>()
{
	public int compare(Apple a1, Apple a2)
	{
		return a1.getWeight().compareTo(a2.getWeight());
	}
}
```

之后（用了lambda表达式）：
```
(Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight());
```

**##应用场景**

**###函数式接口**

定义：只定义了一个抽象方法的借口（如Comparator和Runnable）

可以使用@FunctionalInterface来标示，方便编译检查

**Supplier<T>**

无参有返， T get()
```
Supplier<T>		//提供一个T类型的对象
IntSupplier 		//提供一个Int类型的对象
LongSupplier 		//提供一个Long类型的对象
DoubleSupplier 		//提供一个Double类型的对象
BooleanSupplier 	//提供一个Boolean类型的对象
```

**Predicate<T>**

定义了一个test的抽象方法，接受一个泛型对象T，返回一个boolean值

```
Perdicate<T>		//接受单个参数，进行判断
BiPerdicate<T,U>	//接受两个参数，进行判断
IntPerdicate		//接受一个Integer类型的参数，进行判断
LongPerdicate		//接受一个Long类型的参数，进行判断
DoublePerdicate		//接受一个Double类型的参数，进行判断
```

**Consumer<T>**

定义了一个accept的抽象方法，接受一个泛型对象T，没有返回（void）

```
Consumer<T>		//接受单个参数，进行消费
BiConsumer<T,U>		//接受两个参数，进行消费
IntConsumer		//接受一个Integer类型的参数，进行消费
LongConsumer		//接受一个Long类型的参数，进行消费
DoubleConsumer		//接受一个Double类型的参数，进行消费
ObjIntConsumer<T>	//接受T和Integer类型的参数，进行消费
ObjLongConsumer<T>	//接受T和Long类型的参数，进行消费
ObjDoubleConsumer<T>    //接受T和Double类型的参数，进行消费
```

**Function<T,R>**

定义了一个apply的抽象方法，接受一个泛型对象T，并返回一个泛型为R的对象

```
Function<T,R>			//接受T类型的参数，转换成R类型。

IntFunction<R>			//接受Integer类型的参数，转换成R类型。
LongFunction<R>			//接受Long类型的参数，转换成R类型。
DoubleFunction<R>		//接受Double类型的参数，转换成R类型。

ToIntFunction<T>		//接受T类型的参数，转换成Integer类型。
ToLongFunction<T>		//接受T类型的参数，转换成Long类型。
ToDoubleFunction<T>		//接受T类型的参数，转换成Double类型。

IntToLongFunction		//接受Int类型的参数，转换成Long类型。
IntToDoubleFunction		//接受Int类型的参数，转换成Double类型。
LongToIntFunction<T>		//接受Long类型的参数，转换成Integer类型。
LongToDoubleFunction<T>		//接受Long类型的参数，转换成Double类型。
DoubleToIntFunction<T>		//接受Double类型的参数，转换成Integer类型。
DoubleToLongFunction<T>		//接受Double类型的参数，转换成Long类型。

ToIntBiFunction<T,U>		//接受T类型和U类型的参数，转换成Integer类型。
ToLongBiFunction<T,U>		//接受T类型和U类型的参数，转换成Long类型。
ToDoubleBiFunction<T,U>		//接受T类型和U类型的参数，转换成Double类型。

IntUnaryOperator		//接受Integer类型的参数，转换成Integer类型。可以理解为`一元操作符`。
LongUnaryOperator		//接受Long类型的参数，转换成Long类型。
DoubleUnaryOperator		//接受Double类型的参数，转换成Double类型。

IntBinaryOperator		//接受2个Integer类型的参数，转换成Integer类型。可以理解为`二元操作符`。
LongBinaryOperator		//接受2个Long类型的参数，转换成Long类型。
DoubleBinaryOperator		//接受2个Double类型的参数，转换成Double类型。

```

常用的函数式接口还有：

UnaryOperator<T>: 对类型T进行的一元操作
	
BinaryOperator<T>：对类型T进行的二元操作

**##原理**

**###类型检查**

目标类型：上下文（接受它传递的方法的参数，或接受它的值的局部变量）中Lambda需要的类型

同样的Lambda可以应用到不同的函数式接口

**###类型推断**

Java编译器会推断Lambda的参数类型，可以进一步简化代码

```
//a没有显式类型
List<Apple> greenApples=filter(inventory, a-> “green”,equals(a.getColor()));
```

**###限制**

使用局部变量：
```
Int portNumber=8088;
Runnable r=()-> System.out.println(portNumber);
```

局部变量必须自己显式声明为final或者事实上是final

**###方法引用**

```
(Apple a) -> a.getWeight();
Apple::getWeight
```

类别：

* 指向静态方法的方法引用
* 指向任意类型实例方法的方法引用
* 指向现有对象的实例方法的方法引用

构造函数引用：

对于一个现有构造函数，可以利用名称和关键字new来创建它的一个引用：

ClassName::new

**##复合Lambda表达式**

**比较器复合**

逆序：reverse()
比较器链：comparing().thenComparing()

**谓词复合**

Negate(),and,or

**函数复合**

andThen(),compose()

```
Function<Integer, Integer> f=x ->x+1;
Function<Integer, Integer> g=x ->x*2;
//g(f(x))
Function<Integer,Integer> h1=f.andThen(g);
//f(g(x))
Function<Integer,Integer> h2=f.compose(g);
Int result=h.apply(1);
```
