当不涉及同步，只是涉及线程通信的时候，用synchronized + wait/notify就显得太重了
此时可以使用CountDownLatch,CyclicBarrier,Semaphore

**#CountDownLatch**

CountDownLatch类位于java.util.concurrent包下，利用它可以实现类似计数器的功能。比如有一个任务A，它要等待其他4个任务执行完毕之后才能执行，此时就可以利

用CountDownLatch来实现这种功能了。

只有一个构造器`public CountDownLatch(int count)`,count为计数值

方法：
` public void await() throws InterruptedException`//调用await方法的线程会被挂起，直到计数值为0

`public boolean await(long timeout, TimeUnit unit)`//和await()类似，只不过等待一段时间后，如果计数器没有变为0，线程也会继续运行

`public void countDown()`//对计数器的值减1

**#cyclicBarrier**

字面意思回环栅栏，通过它可以实现让一组线程等待至某个状态之后再全部同时执行。叫做回环是因为当所有等待线程都被释放以后，CyclicBarrier可以被重用。我们暂且把这个状态就叫做barrier，当调用await()方法之后，线程就处于barrier了

构造器：
```
public CyclicBarrier(int parties, Runnable barrierAction)
public CyclicBarrier(int parties)
```
parties代表让多少个线程等待到barrier状态；barrierAction指等到barrier状态后执行的行为

方法：
```
//用来挂起当前线程，直至所有线程都到达barrier状态再同时执行后续任务
 public int await() throws InterruptedException, BrokenBarrierException
 //让这些线程等待至一定的时间，如果还有线程没有到达barrier状态就直接让到达barrier的线程执行后续任务。
 public int await(long timeout, TimeUnit unit)
```
```
package concurrent;
import java.util.concurrent.BrokenBarrierException;
/**
 * 回环栅栏，通过它可以实现让一组线程等待至某个状态之后再全部同时执行
 */
import java.util.concurrent.CyclicBarrier;
import java.util.concurrent.TimeUnit;

public class CyclicBarrierDemo 
{
	private static final int N=3;
	public static void main(String[] args) 
	{
		CyclicBarrier barrier=new CyclicBarrier(N);
		
		
		for(int i=0;i<N;i++)
		{
			new Thread(new Writer(barrier)).start();
		}
		
		
		try {
			TimeUnit.SECONDS.sleep(3);
		} catch (InterruptedException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		System.out.println("--------------重用barrier-------------------");
		
		for(int i=0;i<N;i++)
		{
			new Thread(new Writer(barrier)).start();
		}
	}
}

class Writer implements Runnable
{
	private CyclicBarrier barrier;
	
	public Writer(CyclicBarrier barrier) {
		this.barrier=barrier;
	}
	@Override
	public void run() {
		System.out.println("线程"+Thread.currentThread().getName()+"开始");
		try {
			TimeUnit.SECONDS.sleep(1);
			System.out.println("线程"+Thread.currentThread().getName()+"写入完毕");
			barrier.await();
		} catch (InterruptedException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (BrokenBarrierException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		System.out.println("所有线程写入完毕，可以执行其他任务");
	}
}

```

**#semaphore**

Semaphore翻译成字面意思为 信号量，Semaphore可以控制同时访问的线程个数，通过 acquire() 获取一个许可，如果没有就等待，而 release() 释放一个许可。

构造器：
```
public Semaphore(int permits) {          //参数permits表示许可数目，即同时可以允许多少线程进行访问
    sync = new NonfairSync(permits);
}
public Semaphore(int permits, boolean fair) {    //这个多了一个参数fair表示是否是公平的，即等待时间越久的越先获取许可
    sync = (fair)? new FairSync(permits) : new NonfairSync(permits);
}
```

方法：
```
public void acquire() throws InterruptedException {  }     //获取一个许可
public void acquire(int permits) throws InterruptedException { }    //获取permits个许可
public void release() { }          //释放一个许可
public void release(int permits) { }    //释放permits个许可
```

注：这四个方法会一直堵塞，直到获得结果；release必须先获得一个许可，才能释放

要想不被堵塞，可以采用tryAcquire(),tryRelease()方法

```
package concurrent;

import java.util.concurrent.Semaphore;
import java.util.concurrent.TimeUnit;

/**
 * 假若一个工厂有5台机器，但是有8个工人，一台机器同时只能被一个工人使用，
 * 只有使用完了，其他工人才能继续使用。：
 * @author ASUS
 *
 */
public class SemaphoreDemo 
{
	public static void main(String[] args) 
	{
		int N=8;								//工人数
		
		Semaphore sem=new Semaphore(5);			//机器数
		
		for(int i=0;i<N;i++)
		{
			new Worker(i,sem).start();
		}
	}

}
class Worker extends Thread
{
	private int num;
	private Semaphore sem;
	public Worker(int num,Semaphore sem) {
		this.num=num;
		this.sem=sem;
	}
	@Override
	public void run() {
		try {
			sem.acquire();
			System.out.println("工人"+this.num+"在进行生产");
			
			TimeUnit.SECONDS.sleep(2);
			
			sem.release();
			System.out.println("工人"+this.num+"生产完毕");
		} catch (InterruptedException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
}
输出：
工人0在进行生产
工人2在进行生产
工人1在进行生产
工人3在进行生产
工人4在进行生产
工人0生产完毕
工人4生产完毕
工人3生产完毕
工人1生产完毕
工人2生产完毕
工人7在进行生产
工人6在进行生产
工人5在进行生产
工人6生产完毕
工人7生产完毕
工人5生产完毕
//同一时间，只有五个人在用五台机器，只有其中一人释放，才会有新的人进入使用
```

**总结：**

* CountDownLatch和CyclicBarrier都能够实现线程之间的等待，只不过它们侧重点不同：
    
    CountDownLatch一般用于某个线程A等待若干个其他线程执行完任务之后，它才执行；
    
    CyclicBarrier一般用于一组线程互相等待至某个状态，然后这一组线程再同时执行；
    
    CountDownLatch是不能够重用的，而CyclicBarrier是可以重用的。

* Semaphore其实和锁有点类似，它一般用于控制对某组资源的访问权限。
