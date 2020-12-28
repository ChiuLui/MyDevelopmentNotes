Android 开发基础 Java 线程池
------------
# 简介
> 本篇文章介绍的是 Java 的线程池的基础知识和简单运用. 我们从实际运用出发, 不讲深而难懂的原理.(我是 Android 开发者, 讲解的过程可能会从 Android 的角度出发与分析)

# 目录
[1.什么是线程池](#1)
[2.为什么要使用线程池](#2)
[3. ThreadPoolExecutor 类](#3)
[4.线程池的处理流程](#4)
[5.线程池的分类与简单解析](#5)

# <span id = "1">**1.什么是线程池**</span>
> 所谓线程池, 通俗化的讲就是把这么多个线程统一的放到一个地方管理, 这个地方就叫线程池.
> 每次线程执行完任务前,先把任务委派给线程池空闲的线程, 如果没有空闲的线程, 则根据线程池任务策略执行。处理完任务后, 线程不会直接被销毁掉,会放到线程池管理。
> 在 java 1.5 中提供了一个 Executor 框架, 用于把任务的提交和执行解耦, 任务的提交交给 Runnable 或 Callable, 而 Execute 框架用来处理任务. Execute 框架中最核心的成员就是 `java.util.concurrent.ThreadPoolExecutor` ,它是线程池的**核心类**.


# <span id = "2">**2.为什么要使用线程池**</span>
- 总的来说就是**减少系统资源的开销**.

> 我们平时在异步处理任务时, 进程会创建多条线程. 每个线程的创建和销毁都需要一定的开销.使用线程池避免新线程的创建、销毁等繁琐过程. 一个 JVM 里创建太多的线程可能会导致系统由于过度消耗内存而用完内存或“切换过度”。**线程池为线程生命周期开销问题和资源不足问题提供了解决方案。**通过对多个任务重用线程，线程的开销被分摊到了多个任务上。

# <span id = "3">**3. ThreadPoolExecutor 类**</span>
ThreadPoolExecutor 类是一个非常重要的类, 我们通过这个类去创建一个线程池.我们首先来介绍一下他的构造方法:

```java
public ThreadPoolExecutor(int corePoolSize,
						int maximumPoolSize,
						long keepAliveTime,
						TimeUnit unit,
						BlockingQueue<Runnable> workQueue,
						ThreadFactory threadFactory,
						RejectedExecutionHandler handler){
	...
	...
}

```
我们一个个来分析它的参数:

- corePoolSize (核心线程数)
> 线程池里的线程分为`核心线程`和`非核心线程`. 这个属性指的是一个线程池里面的核心线程数量. 默认情况下是空的, 只有任务提交时才会创建线程. 当运行的线程数少于 `corePoolSize`, 则会创建行的线程来处理任务, 如果等于或多与 `corePoolSize`, 则不再创建. 如果调用线程池的 `prestartAllcoreThread()` 方法, 线程池会提前创建并启动所有核心线程来等待任务.

- maximumPoolSize (线程池允许创建的最大线程数)
> 如果任务队列满了, 并且线程数小于 `maximumPoolSize` 时, 则线程池会创建新的线程来处理任务.

- keepAliveTime (非核心线程闲置的超时时间)
> `非核心线程`空闲超过这个时间, 则回收. 如果任务很多, 并且每个任务的执行时间事件很短, 则可以调大这个参数来提高线程利用率. 如果设置了 `allowCoreThreadTimeOut` 属性为 `true` 时,也会应用到 `核心线程` .

- TimeUnit (keepAliveTime 的单位)
> 可选择的单位有 `DAYS`天, `HOURS`小时, `MUNYTES`分钟, `SECONDS`秒, `MILLISECONDS`毫秒, 等.

- workQueue (任务的阻塞队列类型)
> 如果当前线程数大于 `corePoolSize`, 则将任务添加到此任务队列中. 该任务队列是 `BockingQueue` 类型, 也就是阻塞队列.
> 在Java中有7中阻塞队列, 分别是:
> - ArrayBlockingQueue: 由数组结构构成的有界阻塞队列.
> - LinkedBlockingQueue: 由链表结构组成的有界阻塞队列.
> - PriorityBlockingQueue: 支持优先级排序的无界阻塞队列.
> - DelayQueue: 使用优先级队列实现的无界阻塞队列.
> - SynchronousQueue: 不储存元素的阻塞队列.
> - LinkedTransferQueue: 由链表结构组成的无界阻塞队列.
> - LinkedBlockingDeque: 由链表结构组成的双向阻塞队列.

- ThreadFactory (线程工厂)
> 可以用线程工厂给每个创建出来的线程设置名字, 一般情况下不用设置这个参数.

- RejectedExecutionHandler (饱和策略)
> 这是当队列和线程池都满了时所采取的应对策略.有4种策略:
> - AbordPolicy :无法处理新任务, 并抛出 RejectedExecutionException 异常.
> - CallerRunsPolicy: 用调用者所在的线程来处理任务,. 此策略提供简单的反馈控制机制, 能够缓解新任务的提交速度.
> - DiscardPolicy :不能执行的任务, 并将该任务删除.
> - DiscardOldestPolicy :丢弃队列最近的任务, 并执行当前的任务.


# <span id = "4">**4.线程池的处理流程**</span>
我们来看一下当向线程池提交任务时的处理流程:
![提交任务时线程池的处理流程](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1543174475466&di=a76b2ba8d7f6887aff7c2a4fccacd37a&imgtype=0&src=http%3A%2F%2F5b0988e595225.cdn.sohucs.com%2Fimages%2F20171004%2Fcda11d305bd24b9fa9f83d4508c61c96.png)

> 我们梳理一下它的流程:
1. 如果线程池中的线程数未达到核心线程数, 则创建核心线程处理任务.
2. 如果线程数大于或者等于核心线程数, 则将任务加入任务队列, 线程池中的空闲线程会不断的从任务队列中取出任务来进行处理.
3. 如果任务队列满了, 并且线程数没有达到最大线程数, 则创建非核心线程去处理任务.
4. 如果线程超过了最大线程数, 则执行饱和策略.


> 可以看到他的顺序是:
> - 核心线程池 --> 任务队列 --> 线程池 -- 饱和策略.
> 也就是对应上面的属性:
> - corePoolSize --> workQueue --> maximumPoolSize -->RejectedExecutionHandler
> 这样就好理解每个传入构造方法的属性它的作用是什么.

# <span id = "5">**5.线程池的分类与简单解析**</span>
我们可以通过 ThreadPoolExecutor.创建多种不同类型的线程池, 在这里我介绍 4 种常用的线程池分别是:
- FixedThreadPool
- CacheThreadPool
- SingleThreadExecutor
- ScheduledThreadPool

### 5.1 FixedThreadPool
##### 特点: 可重用的固定线程数的线程池.
> FixedThreadPool 是一个典型且优秀的线程池，它具有**提高程序效率和节省创建线程时所耗的开销的优点**。但是，**在线程池空闲时**，即线程池中没有可运行任务时，它不会释放工作线程，还会**占用一定的系统资源**。

##### 简单解析:
我们来看一下它是如和创建的:
```
public static ExecutorService newFixedThreadPool(){
 return new ThreadPoolExecutor(nThreads, 
					nThreads, 
					OL, 
					TimeUnit.MILLISECONDS, 
					new LinkedBlockingQueue<Runnable>());
}
```
> 看它的参数, 和我们前面介绍的参数有所不同. 
> 1. FixedThreadPool 的 corePoolSize 和 maximumPoolSize 都设置为创建 FixedThreadPool 指定的参数 nThreads, 也就意味着 FixedThreadPool 只有核心线程, 并且数量是固定的, 没有非核心线程.
> 2. KeepAliveTime 设置为 0L 意味着多余的线程会立即被终止. 
> 3. 任务队列采用了无界的阻塞队列 LinkedBlockingQueue.

他的执行流程如下图:
![FixedThreadPool 的执行示意图](https://i.imgur.com/8aUqJy6.jpg)

### 5.2 CachedThreadPool
##### 特点: 是一个根据需要创建线程的线程池.
> 一个可缓存线程池，如果线程池长度超过处理需要，**可灵活回收空闲线程**，若无可回收，则新建线程。
在使用CachedThreadPool时，一定要**注意控制任务的数量**，否则，由于**大量线程同时运行，很有会造成系统瘫痪。**

##### 简单解析:
创建 CachedThreadPool 的代码如下:
```
public static ExecutorService newCacheThreadPool(){
 return new ThreadPoolExecutor(0, 
					Integer.MAX_VALUE, 
					60L, 
					TimeUnit.SECONDS, 
					new SynchronousBlockingQueue<Runnable>());
}
```
> 1. corePoolSize 为 0, maximumPoolSize 设置为 Integer.MAX_VALUE, 这意味着它没有核心线程, 非核心线程是无界的.
> 2. keepAliveTime 设置为 60L ,则空闲线程等待新任务的最长时间为 60s .
> 3. 阻塞队列用了 SynchronousQueue, 它是一个不储存元素的阻塞队列, 每个插入操作必须等待另一个线程的移除, 同样任何一个移除操作都等待另一个线程的插入操作.

execute 方法的执行如下图:
![CachedThreadPool 的执行示意图](https://i.imgur.com/momqMUI.jpg)

###  5.3 SingleThreadExecutor
##### 特点: 使用单个工作线程的线程池.
> 创建一个单线程化的Executor，即只创建唯一的工作者线程来执行任务，它只会用唯一的工作线程来执行任务，**保证所有任务按照指定顺序(FIFO, LIFO, 优先级)执行**。

##### 简单解析:
创建源码如下:
```
public static ExecutorService newSingleThreadExecutor(){
 return new ThreadPoolExecutor(1, 
					1, 
					0L, 
					TimeUnit.MILLISECONDS, 
					new LinkedBlockingQueue<Runnable>());
}
```
> 1. corePoolSize ,maximumPoolSize 都为 1, 这意味着它只有 1 个核心线程.
> 2. KeepAliveTime 设置为 0L 意味着多余的线程会立即被终止. 
> 3. 任务队列采用了无界的阻塞队列 LinkedBlockingQueue.

当 execute 方法执行时:
![SingleThreadPool 的执行示意图](https://i.imgur.com/zOMo9nR.jpg)

###  5.4 ScheduledThreadPool
##### 特点: 是一个能实现定时和周期性任务的线程池.
> 创建一个定长的线程池，而且**支持定时的以及周期性的任务执行**。

##### 简单解析:
创建源码如下:
```
public static ScheduledExecutorService newScheduledThreadPool(int corePoolSize){
 return new ScheduledThreadPoolEcecutor(corePoolSize)
}

...
...
...

public ScheduledThreadPoolExecutor (int corePoolSize){
 super(corePoolSize, 
	Integer.MAX_VALUE, 
	DEFAULT_KEEPALIVE_MILLIS, 
	TimeUnit.MILLISECONDS, 
	new DelayedWorkQueue());
}

```
> 1. corePoolSize 是传进来的默认值, maximumPoolSize 是 Intger.Max_VALUE.
> 2. 任务队列采用了无界的阻塞队列 DelayedWorkQueue, 所以 maximumPoolSize 这个参数是无效的.

当 execute 方法执行时:
![ScheduledThreadPoolExecutor 的执行示意图](https://i.imgur.com/zUOrg7j.jpg)