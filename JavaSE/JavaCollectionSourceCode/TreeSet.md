#TreeSet

特点：数据元素可以排序且不重复

和HashSet对比，HashSet中的元素类必须重写hashcode()和equals()方法

去重：比较等于0就是重复

有两种构造器：

* 元素可以排序， java.lang.Comparable+compareTo(),使用 new TreeSet()

* 元素本身不能排序，使用业务排序类， java.util.Comparator+compare()
  使用 new TreeSet(Comparator<? super E> comparator);

**注：**

* TreeSet在添加数据时排序，数据更改不会影响原来的排序

* 不要修改数据，否则可能重复

```
import java.util.Comparator;
import java.util.TreeSet;
import java.lang.Comparable;
/**
 * 无法排序的类，即没有实现Comparable接口，所以无法排序，只能使用Comparator接口的比较器
 * @author ASUS
 *
 */
public class Person {
	private String name;	//名字
	private int handsome;	//帅气指数
	
	public Person()
	{
		
	}
	
	public Person(String name, int handsome) {
		super();
		this.name = name;
		this.handsome = handsome;
	}


	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getHandsome() {
		return handsome;
	}
	public void setHandsome(int handsome) {
		this.handsome = handsome;
	}
	
	public String toString()
	{
		return "名字："+this.getName()+"帅气指数："+this.getHandsome()+"\n";
	}
	public static void main(String[] args) {
		Person per1=new Person("刘德华",100);
		Person per2=new Person("梁朝伟",80);
		Person per3=new Person("张学友",90);
/*		
 * 会报错：Person cannot be cast to java.lang.Comparable
		TreeSet<Person> set=new TreeSet<>();
		set.add(per1);
		set.add(per2);
		set.add(per3);
		System.out.println(set);
*/		
		//采用Comparator的构造器(使用内部类实现)
		TreeSet<Person> set2=new TreeSet<>(
				new Comparator<Person>()
				{

					@Override
					public int compare(Person o1, Person o2) {
						return o1.getHandsome()-o2.getHandsome();
					}
					
				});
		set2.add(per1);
		set2.add(per2);
		set2.add(per3);
		
		System.out.println(set2);
		System.out.println("-----------------------");
		/*		输出为：
		 * 		[名字：梁朝伟 帅气指数：80
				 , 名字：张学友 帅气指数：90
				 , 名字：刘德华 帅气指数：100
				 ]
		 */
		per1.setHandsome(-100);
		System.out.println(set2);
		/*		输出为：
		 * 		[名字：梁朝伟 帅气指数：80
				 , 名字：张学友 帅气指数：90
				 , 名字：刘德华 帅气指数：-100
				 ]
				 在添加数据时排序，修改不会改变原来的数据，所以也有可能出现重复的值
		 *		所以要想安全的使用TreeSet，将要将类的属性用final修饰，使其不能被修改	 
		 */	
	}
}
```
