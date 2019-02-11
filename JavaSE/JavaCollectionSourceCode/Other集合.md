#Other集合

**IdentityHashMap**

特点：键只以地址去重，而不是比较hashcode和equals

注意：键是常量池中的字符串

```
import java.util.IdentityHashMap;
import java.util.Iterator;

/**
 * 键只以地址去重
 * @author ASUS
 *
 */
public class IdentityHashMapDemo {
	
	public static void main(String[] args) {
		IdentityHashMap<String,String> map=new IdentityHashMap<>();
		//"a"的地址相同，去重
		map.put("a", "test1");
		map.put("a", "test2");
		//"b"的地址不同，是不同的key值
		map.put(new String("b"), "test3");
		map.put(new String("b"), "test4");
		
		System.out.println(map.size());
		
		Iterator itr=map.keySet().iterator();
		
		while(itr.hasNext())
		{
			System.out.println(itr.next());
		}
	}
}

```
**EnumMap**

特点：

* 键必须为枚举的值
* 构造器： public EnumMap(指定枚举class对象）

```
import java.util.EnumMap;

public class EnumMapDemo 
{
	public static void main(String[] args) {
		
		EnumMap<Season,String> map=new EnumMap<>(Season.class);
		
		map.put(Season.WINTER, "12");
		map.put(Season.SUMMER, "6");
		
		System.out.println(map.size());
	}
	
	enum Season
	{
		SPRING,SUMMER,AUTUMN,WINTER
	}
}

```
