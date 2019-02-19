#ThreadLocal

**概念**

ThreadLocal是线程本地变量的意思，它不是一个线程而是一个线程的本地化对象。当工作于多线程环境中的对象采用ThreadLocal维护变量时，ThreadLocal为每个使用该变量的线程分配一个独立的副本。每个线程都可以独立的改变自己的副本，而不影响其他线程的副本。

ThreadLocal是使用空间换时间，synchronized是使用时间换空间

```
/**
 * ThreadLocal线程局部变量
 *
 * ThreadLocal是使用空间换时间，synchronized是使用时间换空间
 * 比如在hibernate中session就存在与ThreadLocal中，避免synchronized的使用
 * 
 * @author ASUS
 */
package yxxy.c_022;

import java.util.concurrent.TimeUnit;

public class ThreadLocal2 {
	//volatile static Person p = new Person();
	static ThreadLocal<Person> tl = new ThreadLocal<>();
	
	public static void main(String[] args) {
				
		new Thread(()->{
			try {
				TimeUnit.SECONDS.sleep(2);
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
			
			System.out.println("t1:"+tl.get());
		}).start();
		
		new Thread(()->{
			try {
				TimeUnit.SECONDS.sleep(1);
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
			tl.set(new Person());
			System.out.println("t2:"+tl.get());
		}).start(); 
	}
	
	static class Person {
		String name = "zhangsan";
	}
}
//输出：
//t2:yxxy.c_022.ThreadLocal2$Person@23dbd0b7
//t1:null
//在线程t2中修改tl对象，只会影响t2中的对象，不会影响t1中的对象
```

**常用方法**

* void set(Object value) 设置当前线程的线程局部变量的值
* public Object get() 返回当前线程的线程局部变量的值
* public void remove() 删除当前线程的局部变量的值
* protected Object initialValue() 返回当前线程局部变量的初始值

**应用场景**

数据库连接，Session管理等
