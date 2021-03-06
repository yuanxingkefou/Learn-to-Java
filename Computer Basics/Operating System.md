#操作系统

**一.基本概念**

进程process：正在执行的一个程序

地址空间（address space）：从某个最小值的存储位置到某个最大值存储位置的列表

**系统调用:**
fork系统调用创建一个原有进程的精确副本，在fork后，原有的进程及其副本就分开了，父子进程其中一个的后续变换并不会影响到另一个
exec系统调用

1.fork（）
    一个程序一调用fork函数，系统就为一个新的进程准备了前述三个段，首先，系统让新的进程与旧的进程使用同一个代码段，因为它们的程序还是相同的，对于数据段和堆栈段，系统则复制一份给新的进程，这样，父进程的所有数据都可以留给子进程，但是，子进程一旦开始运行，虽然它继承了父进程的一切数据，但实际上数据却已经分开，相互之间不再有影响了，也就是说，它们之间不再共享任何数据了。而如果两个进程要共享什么数据的话，就要使用另一套函数（shmget，shmat，shmdt等）来操作。现在，已经是两个进程了，对于父进程，fork函数返回了子程序的进程号，而对于子程序，fork函数则返回零，这样，对于程序，只要判断fork函数的返回值，就知道自己是处于父进程还是子进程中。
    
   事实上，目前大多数的unix系统在实现上并没有作真正的copy。一般的，CPU都是以“页”为单位分配空间的，象INTEL的CPU，其一页在通常情况下是4K字节大小，而无论是数据段还是堆栈段都是由许多“页”构成的，fork函数复制这两个段，只是“逻辑”上的，并非“物理”上的，也就是说，实际执行fork时，物理空间上两个进程的数据段和堆栈段都还是共享着的，当有一个进程写了某个数据时，这时两个进程之间的数据才有了区  
别，系统就将有区别的“页”从物理上也分开。系统在空间上的开销就可以达到最小。

2、对于exec系列函数
    一个进程一旦调用exec类函数，它本身就“死亡”了，系统把代码段替换成新的程序的代码，废弃原有的数据段和堆栈段，并为新程序分配新的数据段与堆栈段，唯一留下的，就是进程号，也就是说，对系统而言，还是同一个进程，不过已经是另一个程序了。不过exec类函数中有的还允许继承环境变量之类的信息，这个通过exec系列函数中的一部分函数的参数可以得到。
    
**对于fork（）：**

*1、子进程复制父进程的所有进程内存到其内存地址空间中。父、子进程的
“数据段”，“堆栈段”和“代码段”完全相同，即子进程中的每一个字节都  
  和父进程一样。
  
*2、子进程的当前工作目录、umask掩码值和父进程相同，fork（）之前父进程
  打开的文件描述符，在子进程中同样打开，并且都指向相同的文件表项。
  
*3、子进程拥有自己的进程ID。

**对于exec（）：**

*1、进程调用exec（）后，将在同一块进程内存里用一个新程序来代替调用
  exec（）的那个进程，新程序代替当前进程映像，当前进程的“数据段”，
“堆栈段”和“代码段”背新程序改写。

*2、新程序会保持调用exec（）进程的ID不变。

*3、调用exec（）之前打开打开的描述字继续打开（好像有什么参数可以令打开
  的描述字在新程序中关闭）

操作系统结构：单体系统，层次系统，微内核，客户机-服务器系统，虚拟机系统，exokernels等

**二.进程与线程**

进程：对正在运行程序的一个抽象

**有四种主要事件导致进程的创建：**

*1）系统初始化，

*2）执行了正在运行的进程所调用的进程创建系统调用

*3）用户请求创建一个新进程

*4）一个批处理作业的初始化

**一个进程的终止，通常有下列条件引起：**

*1）正常退出（自愿的）

*2）出错退出（自愿的）

*3）严重错误（非自愿）

*4）被其他进程杀死（非自愿）

进程的层次结构：进程只有一个父进程，但是可以有零个或多个子进程

**进程的状态：**

*1）运行态（该时刻实际占用CPU）

*2）就绪态（可运行，但因为其他进程正在运行而暂时终止）

*3）阻塞态（除非某种外部事件发生，否则进程不能运行）

转换关系：1——2                   1——3                 2——1                3——2

每个进程占用一个进程表项，即进程表，也称为进程控制块（PCB）

假设一个进程等待I/O操作的时间与其停留在内存中的时间比为p，当内存中同时有n个进程时，CPU的利用率为1-p^n

**线程：**

进程间通信（Inter Process Communication, IPC）

竞争条件：两个或多个进程读写某些共享数据，但最后的结果取决于进程运行的正确时序，称为竞争条件（race condition)

临界区：对共享内存进行访问的程序片段称作临界区域或临界区

**避免竞争条件的解决方案满足的条件：**

*1）任何两个进程不能同时处于临界区

*2）不应对CPU的速度和数量做任何假设

*3）临界区运行的进程不得阻塞其他进程

*4）不得使进程无限期等待进入临界区

**忙等待的互斥：**

*1）屏蔽中断

*2）锁变量

*3）严格轮换法

*4）Peterson解法

*5）TSL指令

信号量(semaphore)与互斥量(mutex)

管程

引入条件变量以及相关的两个操作：wait和signal

消息传递

使用两条原语send和recieve

**调度算法：**

*1）先来先服务（first-come first-severd)

*2）最短作业优先（shortest job first)

*3)最短剩余时间优先（shortest remaining time next)

*4)轮询

*5）优先级调度

**三.存储管理**

**空闲内存管理：**

*1）使用位图的存储管理

*2）使用链表的存储管理

虚拟内存的基本思想：每个程序拥有自己的地址空间，这个空间被分为多个块，每一块称为一页或页面，每一页有连续的地址范围。这些页被映射到物理内存，但并不是所有的页都必须在内存中才能运行程序

**分页技术：**
