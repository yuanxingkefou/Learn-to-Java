#WeakHashMap

**引用类型**

* 强引用：StrongReference:引用指向对象，gc运行时不回收
* 软引用：SoftReference: gc运行时可能回收（在JVM内存不够时回收）
* 弱引用：WeakReference gc运行时立即回收
* 虚引用：PhantomReference 类似于无引用，立即跟踪对象被回收的状态，不能单独使用，必须与引用队列（ReferenceQueue）联合使用

目的：避免对象长期驻留在内存中，解决垃圾回收机制回收时机问题

WeakHashMap:

键为弱引用，回收键后自动删除key-value对象

```
import java.lang.ref.WeakReference;
import java.util.WeakHashMap;

public class WeakHashMapDemo 
{
	public static void main(String[] args) {
		WeakHashMap<String,String> map=new WeakHashMap<>();
		
		//测试数据,test1和test2不能被回收，test3和test4被回收
		map.put("a", "test1");
		map.put("b", "test2");
		map.put(new String("c"), "test3");
		map.put(new String("d"), "test4");
		
		System.out.println("gc回收前："+map.size());
		
		System.gc();
		System.runFinalization();
		//输出为：gc回收前：4
		//		 gc回收后：2
		System.out.println("gc回收后："+map.size());
		
//		testStrong();
//		testWeak();			
	}
	
	public static void testStrong()
	{
		/**
		 * 测试强引用
		 * 输出为：
		 * gc运行前：hello
		 * gc运行后：hello
		 */
		//字符串常量池， 共享（不能回收）
		String str="hello";
		
		WeakReference<String> wr=new WeakReference<String>(str);
		
		System.out.println("gc运行前："+wr.get());
		//断开引用
		str=null;
		System.gc();
		System.runFinalization();
		System.out.println("gc运行后："+wr.get());
	}
	
	public static void testWeak()
	{
		/**
		 * 测试弱引用
		 * 输出为：gc运行前：hello
				  gc运行后：null
		 */
		//字符串常量池
		String str=new String("hello");
		
		WeakReference<String> wr=new WeakReference<String>(str);
		
		System.out.println("gc运行前："+wr.get());
		//断开引用
		str=null;
		System.gc();
		System.runFinalization();
		System.out.println("gc运行后："+wr.get());
	}
}

```
