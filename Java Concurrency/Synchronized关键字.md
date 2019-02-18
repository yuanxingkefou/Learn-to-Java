#Synchronized关键字

是一种同步锁，修饰的对象有以下几种：

* **修饰一个代码块**，作用的范围是用{}括起来的代码块，作用于调用这个代码块的对象

每个对象只与一个锁相关联，所以如果有两个线程访问两个对象的同步代码块，并不会发生互斥

```
package concurrent;

public class SynchronizedDemo01 extends Thread
{
	@Override
	public void run() {
		synchronized(this)
		{
			for(int i=0;i<5;i++)
			{
				System.out.println(Thread.currentThread().getName()+" count :"+i);
			}
		}
	}

	public static void main(String[] args) {
		SynchronizedDemo01 syn01=new SynchronizedDemo01();
		SynchronizedDemo01 syn02=new SynchronizedDemo01();
		
		Thread thread1=new Thread(syn01,"thread1");
		Thread thread2=new Thread(syn02,"thread2");
		
		thread1.start();
		thread2.start();
	}
}
输出：
thread2 count :0
thread2 count :1
thread2 count :2
thread2 count :3
thread2 count :4
thread1 count :0
thread1 count :1
thread1 count :2
thread1 count :3
thread1 count :4

```

  当一个线程访问对象的一个synchronized(this)同步代码块时，另一个线程仍然可以访问该对象中的非synchronized(this)同步代码块

* **修饰一个方法**，作用的范围是整个方法，作用于调用这个方法的对象

  注：
  
  1.一个同步方法可以调用另一个同步方法，一个线程已经拥有某个对象的锁，再次申请的时候仍然会得到该对象的锁，也就是说synchronized获得的锁是可重用的

  2.synchronized关键字不能继承。
  
  虽然可以使用synchronized来定义方法，但synchronized并不属于方法定义的一部分，因此，synchronized关键字不能被继承。如果在父类中的某个方法使用
  了synchronized关键字，而在子类中覆盖了这个方法，在子类中的这个方法默认情况下并不是同步的，而必须显式地在子类的这个方法中加上synchronized关键字
  才可以。
  
  3.在定义接口方法时不能使用synchronized关键字。

  4.构造方法不能使用synchronized关键字，但可以使用synchronized代码块来进行同步

* **修饰一个静态方法**,作用的范围是整个方法，作用于该类的所有对象

* **修饰一个类**，作用于该类的所有对象,与静态方法类似

```
package concurrent;

public class SynchronizedClass implements Runnable
{
	private static int count=0;
	
	public void method()
	{
		//synchronized修饰一个类
		synchronized(SynchronizedClass.class)
		{
			for(int i=0;i<5;i++)
			{
				System.out.println(Thread.currentThread().getName()+"count: "+count++);
			}
		}
	}
	
	@Override
	public void run() {
		method();		
	}
	
	public static void main(String[] args) {
		SynchronizedClass sc=new SynchronizedClass();
		SynchronizedClass sc2=new SynchronizedClass();
		Thread thread=new Thread(sc,"thread1");
		Thread thread2=new Thread(sc2,"thread2");
		thread.start();
		thread2.start();
	}
}
/*
输出为：
thread1count: 0
thread1count: 1
thread1count: 2
thread1count: 3
thread1count: 4
thread2count: 5
thread2count: 6
thread2count: 7
thread2count: 8
thread2count: 9
*/

```


程序在执行过程中，如果出现异常，默认情况锁会释放
```
/**
 * 程序在执行过程中，如果出现异常，默认情况锁会释放
 * @author mashibing
 */
package yxxy.c_011;

import java.util.concurrent.TimeUnit;

public class T {
	int count = 0;
	synchronized void m() {
		System.out.println(Thread.currentThread().getName() + " start");
		while(true) {
			count ++;
			System.out.println(Thread.currentThread().getName() + " count = " + count);
			try {
				TimeUnit.SECONDS.sleep(1);
				
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
			
			if(count == 5) {
				int i = 1/0; //此处抛出异常，锁会被释放，要像不被释放，可以在这里进行catch，然后让循环继续
				System.out.println(i);
			}
		}
	}
	
	public static void main(String[] args) {
		T t = new T();
		Runnable r = new Runnable() {

			@Override
			public void run() {
				t.m();
			}
			
		};
		new Thread(r, "t1").start();
		
		try {
			TimeUnit.SECONDS.sleep(3);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		
		new Thread(r, "t2").start();
	}
}
```

 锁定某对象o，如果o的属性发生改变，不影响锁的使用但是如果o变成另外一个对象，则锁定的对象发生改变.应该避免将锁定对象的引用变成另外的对象
 
```
/**
 * 锁定某对象o，如果o的属性发生改变，不影响锁的使用
 * 但是如果o变成另外一个对象，则锁定的对象发生改变
 * 应该避免将锁定对象的引用变成另外的对象
 * @author mashibing
 */
package yxxy.c_017;

import java.util.concurrent.TimeUnit;


public class T {
	
	Object o = new Object();

	void m() {
		synchronized(o) {
			while(true) {
				try {
					TimeUnit.SECONDS.sleep(1);
				} catch (InterruptedException e) {
					e.printStackTrace();
				}
				System.out.println(Thread.currentThread().getName());
				
				
			}
		}
	}
	
	public static void main(String[] args) {
		T t = new T();
		//启动第一个线程
		new Thread(t::m, "t1").start();
		
		try {
			TimeUnit.SECONDS.sleep(3);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		//创建第二个线程
		Thread t2 = new Thread(t::m, "t2");
		
		t.o = new Object(); //锁对象发生改变，所以t2线程得以执行，如果注释掉这句话，线程2将永远得不到执行机会
		
		t2.start();
		
	}
}

```
