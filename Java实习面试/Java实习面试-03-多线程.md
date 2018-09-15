# 多线程

并行与并发：
- 并行：指多个任务同时运行。
- 并发：指多个任务轮询交替执行。由于轮询时间间隔短，使人感觉多个任务同时运行。

进程与线程：

- 进程：一段程序的执行过程。进程作为分配资源的基本单位。

- 线程：一个进程可以包含多个线程。线程作为独立运行和独立调度的基本单位。

  由于线程比进程更小，基本上不拥有系统资源，故对它的调度所付出的开销就会小得多，能更高效的提高系统多个程序间并发执行的程度。

多线程：

- 在一个程序中，这些独立运行的程序片段叫作“线程”（Thread），利用它编程的概念就叫作“多线程处理”。多线程是为了同步完成多项任务，不是为了提高运行效率，而是为了提高资源使用效率来提高系统的效率。线程是在同一时间需要完成多项任务的时候实现的。

CPU调度方式：

- 时间片轮询：
- 抢占式：

## 线程

- 用户线程：前台线程，

- 守护线程：后台线程，守护线程作用是为其他前台线程的运行提供便利服务，而且仅在普通、非守护线程仍然运行时才需要。如果没有用户线程，守护线程也就没有存在下去的意义了。

### 创建线程

1. 继承Thread类，重写 run() 方法。

2. 实现Runnable接口，重写 run() 方法。本质是 `Thread(Runnable target)` ；

### 线程状态

- **新建状态（New）**：创建线程后，进入新建状态。

- **就绪状态（Runnable）**：线程调用 start() 方法进入就绪状态，随时准备获取CPU使用权。

- **运行状态（Running）**：CPU调度该线程，线程获取到CPU使用权，进入运行状态。

- **阻塞状态（Blocked）**：① wait()，进入等待阻塞，② 获取synchronized同步锁时，锁对象被其他线程占用，进入同步阻塞状态，③ sleep()，④ join()，⑤阻塞式 IO 操作；

- **死亡状态（Dead）**：① 线程执行完毕，② 线程异常退出，③ stop()；

**阻塞&阻塞解除**：

- wait() 进入阻塞，notify() 与 notifyAll() 解除阻塞。
- 等待同步锁进入阻塞，获得同步锁解除阻塞。
- 阻塞式IO操作进入阻塞，阻塞式IO结束解除阻塞。
- sleep() 进入阻塞，睡眠时间到解除阻塞。
- join() 进入阻塞。

### 线程同步

线程同步：保护共享数据，防止数据不一致；

1. 同步方法：
2. 同步代码块：

## 线程池

1、线程是稀缺资源，使用线程池可以减少创建和销毁线程的次数，每个工作线程都可以重复使用。

2、可以根据系统的承受能力，调整线程池中工作线程的数量，防止因为消耗过多内存导致服务器崩溃。

### 线程池创建

```
public ThreadPoolExecutor(int corePoolSize, int maximumPoolSize, long keepAliveTime, TimeUnit unit, BlockingQueue<Runnable> workQueue, RejectedExecutionHandler handler) 
```

> - corePoolSize：线程池核心线程数量
> - maximumPoolSize：线程池最大线程数量
> - keepAliverTime：当活跃线程数大于核心线程数时，空闲的多余线程最大存活时间
> - unit：存活时间的单位
> - workQueue：存放任务的队列
> - handler：超出线程范围和队列容量的任务的处理程序

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

*command - 要执行的任务

*delay - 从现在开始延迟执行的时间

*unit - 延迟参数的时间单位 

一般情况下，生命周期短的CachedThreadPool是首选；但是在特殊情况下（线程数>系统负载），生命周期长的会选择FixedThreadPool；

### 线程池实现原理

1、判断**线程池里的核心线程**是否都在执行任务，如果不是（核心线程空闲或者还有核心线程没有被创建）则创建一个新的工作线程来执行任务。如果核心线程都在执行任务，则进入下个流程。

2、线程池判断工作队列是否已满，如果工作队列没有满，则将新提交的任务存储在这个工作队列里。如果工作队列满了，则进入下个流程。

3、判断**线程池里的线程**是否都处于工作状态，如果没有，则创建一个新的工作线程来执行任务。如果已经满了，则交给饱和策略来处理这个任务。

