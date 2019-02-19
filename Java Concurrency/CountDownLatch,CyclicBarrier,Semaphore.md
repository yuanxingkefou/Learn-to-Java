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

**##cyclicBarrier**

字面意思回环栅栏，通过它可以实现让一组线程等待至某个状态之后再全部同时执行。叫做回环是因为当所有等待线程都被释放以后，CyclicBarrier可以被重用。我们暂且把这个状态就叫做barrier，当调用await()方法之后，线程就处于barrier了

**##semaphore**

Semaphore翻译成字面意思为 信号量，Semaphore可以控同时访问的线程个数，通过 acquire() 获取一个许可，如果没有就等待，而 release() 释放一个许可。
