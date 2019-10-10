**#Stream**

**##定义**

从支持数据处理的源生成的元素序列

流主要的目的在于表达计算

**##常用的数据处理操作**

**中间操作**

中间操作会返回一个流，并可以链接在一起，可以用它们来设置一条流水线，但是不会产生任何结果。

* filter ：接受lambda，从流中排除某些元素
* map：接受一个Lambda，将元素转换成其他信息或者提取信息
* limit：截断流，使其不超过给定数量
* sorted
* distinct

查找和匹配，可以利用短路求值
* allMatch
* anyMatch
* findFirst
* findAny

**终端操作**

* forEach：消费流中的每个元素，并对其应用Lambda，返回void
* count：返回流中元素的个数，返回long
* collect：把流归约为一个集合，比如list，Map，甚至是Integer
* reduce：Lambda反复结合每一个元素，直到流被归约到一个值

会返回一个非流的值，并处理流水线以返回结果

**##构建流**

**由值创建流**

```
Stream<String> stream=Stream.of("Java 8","in","Action");
Stream<String> emptyStream=Stream.empty();
```

**由数组创建流**

```
Arrays.stream(new int[]{1,3,4,5});
```

**由文件生成流**

可以使用File.lines得到一个流，其中的每一个元素都是给定文件中的一行

**由函数生成流（无限流）**

Stream.iterate()	接受一个初始值，还有一个依次应用在每个产生的新值上的Lambda

Stream.generate()  接受一个Supplier<T>类型的Lambda提供新的值

```
Stream.iterate(0,n->n+2)
		.limit(5)
		.forEach(System.out::println);

Stream.generate(Math::random)
		.limit(5)
		.forEach(System.out::println);
```

**##收集器**

**预定义收集器**

* 将流元素归约和汇总为一个值
```
Collectors.counting()
Collectors.maxBy()
Collectors.minBy()
//汇总
Collectors.summingInt()	summingLong()	summingDouble()
Collectors.averagingInt()	averagingLong()	averagingDouble()
//收集所有信息
DoubleSummaryStatistics,LongSummaryStatistics
IntSummaryStatistics menuStatistics=
	menu.stream().collect(summarizingInt(Dish::getCalories));	

//连接字符串
Collectors.joining()	 将流中每一个对象应用toString()方法得到的所有字符串连接成一个字符串
重载版本：Collectors.joining(",");	可以添加元素之间的分界符

广义上的归约：Collectors.reducing(起始值，转换函数，Lambda表达式)
//计算总和
Int totalCalories = menu.stream().collect(reducing(0 ,Dish::getCalories, (i,j)->i+j));
```
* 元素分组
```
Map< , > = Collectors.groupingBy()
参数为一个Function，叫做分类函数，用来把流中的元素分成不同的组
```
* 元素分区

由一个谓词（返回一个布尔值的函数）作为分类函数，所以最多可以分为两组

partitioningBy()

**##并行流**

**定义**

把内容分成多个数据块，并用不同的线程分别处理每个数据块的流

并行流内部使用流默认的ForkJoinPool框架

* 将顺序流转换为并行流
```
Stream.iterate(1L , i -> i +1)
		  .limit (n)
		  .parallel()
		  .reduce(0L, Long::sum);

转换为顺序流，则使用.sequential()
```

