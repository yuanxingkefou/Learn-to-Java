#ReentrantLock(重入锁)

reentrantlock可用于替代synchronized

构造器

`public ReentrantLock(boolean fair)`

```
/**
 * reentrantlock用于替代synchronized
 * 由于m1锁定this,只有m1执行完毕的时候,m2才能执行
 * 这里是复习synchronized最原始的语义
 * 
 * 使用reentrantlock可以完成同样的功能
 * 需要注意的是，必须要必须要必须要手动释放锁（重要的事情说三遍）
 * 使用syn锁定的话如果遇到异常，jvm会自动释放锁，但是lock必须手动释放锁，因此经常在finally中进行锁的释放
 * @author mashibing
 */
public class ReentrantLock2 {
	Lock lock = new ReentrantLock();

	void m1() {
		try {
			lock.lock(); //synchronized(this)
			for (int i = 0; i < 10; i++) {
				TimeUnit.SECONDS.sleep(1);

				System.out.println(i);
			}
		} catch (InterruptedException e) {
			e.printStackTrace();
		} finally {
			lock.unlock();
		}
	}

	void m2() {
		lock.lock();
		System.out.println("m2 ...");
		lock.unlock();
	}

	public static void main(String[] args) {
		ReentrantLock2 rl = new ReentrantLock2();
		new Thread(rl::m1).start();
		try {
			TimeUnit.SECONDS.sleep(1);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		new Thread(rl::m2).start();
	}
}

```
fair为true时，是公平锁，是哪个线程等待的时间越长，就先执行；fair为false时，是不公平锁，等待的线程执行的概率相同

**ReentrantLock获取锁定与三种方式：**

* lock(), 如果获取了锁立即返回，如果别的线程持有锁，当前线程则一直处于休眠状态，直到获取锁
* tryLock(), 如果获取了锁立即返回true，如果别的线程正持有锁，立即返回false；
* tryLock(long timeout,TimeUnit unit)，如果获取了锁定立即返回true，如果别的线程正持有锁，会等待参数给定的时间，在等待的过程中，如果获取了锁定，就    返回true，如果等待超时，返回false；
* lockInterruptibly:如果获取了锁定立即返回，如果没有获取锁定，当前线程处于休眠状态，直到获取锁定，或者当前线程被别的线程中断

**ReentrantLock和Synchronized的区别**

* ReentrantLock 拥有Synchronized相同的并发性和内存语义，此外还多了锁投票，定时锁等候和中断锁等候

  线程A和B都要获取对象O的锁定，假设A获取了对象O锁，B将等待A释放对O的锁定，
  
  如果使用 synchronized ，如果A不释放，B将一直等下去，不能被中断
  
  如果 使用ReentrantLock，如果A不释放，可以使B在等待了足够长的时间以后，中断等待，而干别的事情
  
* synchronized是在JVM层面上实现的，不但可以通过一些监控工具监控synchronized的锁定，而且在代码执行时出现异常，JVM会自动释放锁定，但是使用Lock则不     行，lock是通过代码实现的，要保证锁定一定会被释放，就必须将unLock()放到finally{}中

* 在资源竞争不是很激烈的情况下，Synchronized的性能要优于ReetrantLock，但是在资源竞争很激烈的情况下，Synchronized的性能会下降几十倍，但是             ReetrantLock的性能能维持常态


