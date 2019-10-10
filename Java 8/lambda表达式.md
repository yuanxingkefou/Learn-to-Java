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

**Predicate<T>**

定义了一个test的抽象方法，接受一个泛型对象T，返回一个boolean值

**Consumer<T>**

定义了一个accept的抽象方法，接受一个泛型对象T，没有返回（void）

**Function<T,R>**

定义了一个apply的抽象方法，接受一个泛型对象T，并返回一个泛型为R的对象

T,R只能是引用类型，基本类型可以使用装箱后的类型，但需要消耗更多的内存，所以有以下接口：

IntPredicate，DoublePredicate，ToIntFunction，IntToDoubleFunction等

常用的函数式接口还有：Supplier<T>, UnaryOperator<T>, BinaryOperator<T>,

BiPredicate<L,R> ,BiConsumer<T,U>, BiFunction<T,U,R>

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

AndThen(),compose()

```
Function<Integer, Integer> f=x ->x+1;
Function<Integer, Integer> g=x ->x*2;
//g(f(x))
Function<Integer,Integer> h1=f.andThen(g);
//f(g(x))
Function<Integer,Integer> h2=f.compose(g);
Int result=h.apply(1);
```
