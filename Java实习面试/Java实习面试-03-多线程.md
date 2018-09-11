# 多线程

# 多线程概述

进程：一个正在运行的程序；在计算机中的运行路径；

一个程序只能有一个进程；

程序是静态的，进程是动态的；

线程：进程的组成单元，在一个进程中有一个或多个线程；

并发：多个任务轮询交替执行；

并行：多任务同时执行；

CPU调度方式：

①时间片轮询；

②抢占式调度方式； Java采用抢占式；

进程与线程共享“堆内存和方法区”；一个线程一个“栈内存”；

 

Java程序的运行原理：

当使用Java命令启动一个程序的时候，JVM启动，就启动了一个进程；main方法会执行，main方法就是该进程中的一个线程（主线程），同时也会启动GC是后台线程（守护线程）；

 

## 创建线程

1.创建线程

①继承Thread类；

②实现Runnable接口：本质是[Thread](mk:@MSITStore:C:\Users\admin\Desktop\JDK_API_1.6_zh_中文.CHM::/java/lang/Thread.html#Thread(java.lang.Runnable))([Runnable](mk:@MSITStore:C:\Users\admin\Desktop\JDK_API_1.6_zh_中文.CHM::/java/lang/Runnable.html) target)；

核心步骤：重写run方法，写入该线程要完成的事情；

2.使用步骤： 

      a）重写run方法；
    
      b）创建线程对象；
    
      c）调用start()；

3.两种线程实现方式的区别：

|        | 继承Thread类             | 实现Runnable接口   |
| ------ | ------------------------ | ------------------ |
| 优点： | 可直接使用线程所有方法； | 多实现；           |
| 缺点： | 单继承；                 | 不能直接创建线程； |

 

## 线程控制

1.线程名称

①设置名称：

Thread(String name)；

setName(String name)；

②String getName()；

③long getId()；//线程终生不变的唯一的标识符；

系统会为创建的线程设置默认Name：Thread-01；

主线程默认Name：main；

2.获取当前执行线程

Thread.currentThread()；返回对当前正在执行的线程对象的引用；

3.线程休眠

Thread.sleep(long millis)；在指定的毫秒数内让当前正在执行的线程休眠（暂停执行）；

4.守护线程

void setDaemon(true)；将线程标记为守护线程；

isAlive()；判断线程是否是活动状态（已启动未终止）；

isDaemon()；判断线程是否是守护线程；

5.加入线程

void join()；当前线程暂停执行，直到加入的线程执行完毕；

应用：可以让另一个线程优先执行；

 

6.礼让线程

Thread.yield()；暂停当前正在执行的线程对象，其他线程并不一定能抢占到。

7.线程优先级

Java抢占式线程调度算法；优先级1-10；默认优先级5；

getPriority()；返回线程的优先级。

setPriority(int newPriority)；更改线程的优先级。

 

线程异常只能处理，不能抛出，无接受异常者；

 

 

## 线程的生命周期

线程状态

1.新建（New）：创建线程对象（new）；JVM为其分配内存，并初始化其成员变量的值

2.就绪（Runnable）：线程对象调用start()；JVM为其创建方法调用栈和程序计数器，等待CPU调度运行；

3.运行（Running）：线程获得CPU使用权，开始执行run()；

4.阻塞（Blocked）：当前正在执行的线程由于某种原因（休眠，礼让，时间到达）暂时失去CPU使用权，等待再次获得CPU使用权；

线程进入阻塞状态：

①wait()；

②执行同步锁时，锁对象被其他线程占用；

③IO操作；

④sleep()，join()；

解除阻塞：

 

\5.    死亡（Dead）：线程执行结束；isAlive()==false；

①run()或call()方法执行完成，线程正常结束。

②线程抛出一个未捕获的Exception或Error。

③直接调用该线程stop()方法来结束该线程——该方法容易导致死锁，通常不推荐使用。

 

  

|      |                                                              |
| ---- | ------------------------------------------------------------ |
|      | ![img](file:///C:/Users/17794/AppData/Local/Temp/msohtmlclip1/01/clip_image001.gif) |

 



 

 

 

 

 

 

 

![index](file:///C:/Users/17794/AppData/Local/Temp/msohtmlclip1/01/clip_image003.jpg)

 

### 线程同步

线程同步：保护共享数据，防止数据不一致；

①同步代码块：

   synchronized(obj){   }   

锁对象：可任意对象；但要保证多线程使用同一锁对象，实现多线程同步；

不能在run()中的同步代码块中使用this作为锁对象；

②同步方法：

同步方法：synchronized关键字修饰的方法；

普通方法锁对象：this；

静态方法锁对象：本类Class对象；

#### 多线程环境中安全使用集合API

Collections.synchronizedXxxx()；

Collections中的静态synchronized方法：

①Collection synchronizedCollention(Collection c)；

②List synchronizedList(list l)；

③Set synchronizedSet(Set s)；

④Map synchronizedMap(Map m)；

   // Synchronized集合是线程安全的，但Iterator不是线程安全的；   List list = Collections.synchronizedList(new   ArrayList());      ...   //迭代时，阻塞其他线程调用add()或remove()修改元素；   synchronized(list) {       Iterator   i = list.iterator(); // Must be in synchronized block         while   (i.hasNext())               foo(i.next());     }   

 

## 定时任务

Timer类

TimerTask：实现 Runnable接口，重写run()方法；由 Timer 安排为一次执行或重复执行的任务。 

void schedule(TimerTask task, Date firstTime, long period)；

*task 执行的任务

*firsttime 开始执行的时间

*period 任务的间隔执行时间

 

 

 

## Java并发编程Executor框架与线程池

Executor框架：基于并发编程的线程池；

Executor：

ExecutorService：

#### 创建线程池

Executors的静态方法：

①创建一个固定线程数的线程池：

static ExecutorService Executors.newFixedThreadPool(int nThreads)；

线程池中，如果有空闲线程，则执行任务；如果没有空闲线程，则任务进入阻塞状态，等待空闲线程；

线程池执行任务：void execute(Runnable command)；

②创建一个带有缓存的线程池

static ExecutorService newCachedThreadPool() ；

线程池中，如果有空闲线程，则执行任务；如果没有空闲线程，则创建一个新线程执行任务；

 ③创建一个单线程线程池

static [ExecutorService](mk:@MSITStore:C:\Users\admin\Desktop\JDK_API_1.6_zh_中文.CHM::/java/util/concurrent/ExecutorService.html) newSingleThreadExecutor()；

线程池中，如果有空闲线程，则执行任务；如果没有空闲线程，则创建一个新线程执行任务；

④创建一个定时执行线程池

static ScheduledExecutorService newScheduledThreadPool(int corePoolSize) 

类似FixedThreadPool；

定时执行方法：

ScheduledFuture<?> schedule(Runnable command, long delay, TimeUnit unit)；

- command - 要执行的任务

- delay - 从现在开始延迟执行的时间

- unit - 延迟参数的时间单位 

 

一般情况下，生命周期短的CachedThreadPool是首选；但是在特殊情况下（线程数>系统负载），生命周期长的会选择FixedThreadPool；

 

#### 线程池可执行的任务：

Rannable接口：public void run()；//无返回值；

Callable<v>接口：public V call()； //有返回值；

 

submit()；

get()；

 

线程池的生命周期

运行状态：

关闭状态：shutdown()；任务提交，不在执行新任务；

终止状态：任务全部提交完毕

 

## 线程协作

多个线程同时完成一个任务时，多个线程共享任务数据；防止数据不一致，使用线程同步（synchronized）机制；

synchronized：

原子性：在每次执行任务时，能保证只有一个线程在执行任务；

可见性：针对每一个线程执行任务时，所见到的数据都真是有效；

1.用5个售票窗口销售100张火车票；

2.银行账户有1000元，一个人去柜台取钱，另一个人去ATM机取钱；

取款方式：柜台，ATM机；

银行类，一个账户，两个线程模拟取钱，

3.火车过山洞：

火车由A到B，中间有一个隧道，每次只允许一趟火车通过，每趟火车经过隧道需要10s，为防止事故发生，每次只允许一趟火车经过，现有5趟火车经过隧道；

 

## 线程死锁

多个线程争取锁对象的持有权时的竞争现象；

避免死锁：

①尽量避免锁的嵌套使用；

②让程序执行时，尽可能只获得一个锁；

③超时限制：Lock中的tryLock，在获取锁对象时，可设置时间限制；

 

## 线程通信

### Object类的监视器方法（monitor）

等待/通知机制

Object监视器方法：wait()、notify()、notifyAll()；

wait()：使线程进入等待状态，如果没有线程来唤醒，则一直处于等待状态；

wait(long timeout)：使线程进入等待状态； 

notify()：唤醒当前处于等待状态的随机一个线程；

notifyAll()：唤醒当前处于等待状态的所有线程；

锁对象执行wait()；和notify()；

 

### 互斥锁（Lock接口）

互斥锁：在一次执行中只能有一个线程持有锁对象；（JDK5新特性）

Lock接口+Condition接口；

2个以上线程通信，用到互斥锁；

 

### 生产者-消费者模型

基于等待/通知机制；

①仓库：共享数据

②生产者：线程同步；不满足条件，wait；满足条件，生产产品+notify；

③消费者：线程同步；不满足条件，wait；满足条件，消费产品+notify；