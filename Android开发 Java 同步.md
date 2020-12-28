Android 开发 Java 同步
--------------
 
# 简介

**本篇文章是带大家了解 Java 多线程中很重要的知识点之一, 同步.**

**主要内容:** 介绍同步的基本概念, 同步与异步的区别, 了解线程安全, 和简单的运用. 

我是Android开发者, 所以可能在讲解过程中插入 Android 中的实现.

目录:

[1.同步的概念](#1)****

[2.同步与异步的区别](#2)

[3.从 Java 内存模型来了解什么是线程安全](#3)

[4.在 Java 中常用的几种同步方法](#4)

# <span id = "1">**1.同步的概念**</span>

我们在 Android 开发中, 很少会用到同步这个知识点, 其实是后台同事帮我做了. 像我们客户端去请求接口去增删改查数据库, 肯定用到了同步这个知识点.

### 1.1什么是同步:
#####在生活中: 
举个最常用的例子, 买火车票. 火车票的数量和编号都是不同的, 每张火车票都会唯一的, 那么全国这么多个地方都卖火车票. 就可能回造成同一张票卖给了多个人. 使用同步就可以这样的问题. 当一个窗口在使用一个火车票资源的时, 后台就锁定这个火车票, 不给其他窗口访问. 当一个窗口访问完这个火车票后, 再解除锁定, 给下一个窗口访问. 这个就是同步. 把火车票比作一个资源, 把卖票窗口比作线程.

#####在 Java 中:
**一个共享资源, 在同一时刻只能被一个线程使用.** (请看上面的例子配合理解)

#####错误的理解: 
就是几个线程可以同时进行访问。


# <span id = "2">**2.同步与异步的区别**</span>

1. **同步:**发送一个请求,等待返回,然后再发送下一个请求.效率会比较低, 安全性高

2. **异步:**发送一个请求,不等待返回,随时可以再发送下一个请求.效率会比同步高, 但安全性高.

3. 同步和异步最大的区别就在于。一个需要等待，一个不需要等待。


# <span id = "3">**3.从 Java 内存模型来了解什么是线程安全**</span>

### 3.1 什么是线程安全:
**指在并发的情况之下，该代码经过多线程使用，线程的调度顺序不影响任何结果。**

### 3.2 了解 Java 内存模型:
在 Java 中的堆内存使用来存储对象实例, 堆内存是被所有线程共享的运行时内存区域, 因此存在内存可见性的问题. 而局部的变量和方法定义的参数则不会再线程之间共享, 因此没有可见性问题, 也不受内存模型的影响.先看下面这张图, 会有更客观的了解.

![Java 内存模型](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1542560012870&di=19580d854b84bfb21b837c6f14f0f83d&imgtype=0&src=http%3A%2F%2Fwww.th7.cn%2Fd%2Ffile%2Fp%2F2016%2F08%2F09%2F92c421097ec1a9fda2048ea440be8494.jpg)

#####Java内存模型定义了线程和主存之间的抽象关系:

线程之间的**共享变量**存储在**主存**中. 每一个**线程**都有一个私有的**本地内存**, 本地内存中存储了**该线程共享变量的副本**. 需要注意的是本地内存是 Java 内存模型的一个抽象概念, 并不真实存在, 它涵盖了缓存、写缓冲区、寄存器等。 Java 内存模型控制线程之间的通信，它决定一个线程对主存共享变量的写入，即何时对另一个线程可见。

#####上图线程 A 与线程 B 之间通信步骤：

1. 线程 A 把线程 A 的本地内存中更新过的共享变量刷新到主存中去：
2. 线程 B 到主存中去读取线程 A 之前已经更新过的共享变量.

这样大家能了解到一个共享变量从 A 线程改变到线程 B 的更新流程了.所以一个共享变量的改变不是马上的, 可能会发生一个线程改变了, 另一个线程还没有刷新, 导致数据异常.比如:

```
	int i = 3;
```

执行的线程必须先自己在工作线程中对变量 i 所在的缓存进行赋值操作, 然后再写入到主存中, 而不是直接将数值写入主存中.

### 3.3 三个重要概念: 原子性 , 可见性, 有序性

要做到线程安全, 就要做到这三点:

#####原子性:

- 概念:

**对基本的数据类型变量的读取和赋值操作就是原子性操作**, 即这些操作时不可被中断的, 要么执行完毕, 要么就不执行.比如:


```

	x = 3;//语句1

	y = x;//语句2

	x++;//语句3

```

**语句1**: 是原子性操作. 

**语句2**很短, 但是包含了两个操作:

1. 先读取 X 的值 
2. 再将 X 的值写入内存.
这两个操作单拿出来就是原子性操作, 但是合起来就不是了.

**语句3**是自增, 但是它包含了三个操作:

1. 读取 X 的值  
2. 对 X 的值进行 +1  
3. 再写入内存中

#####可见性:

- 概念:

可见性是指**线程之间的可见性**, 一个线程的修改状态对另一个线程是可见的.一个线程修改的结果另一个线程马上能看见.

- 实现方法:

 一般用 Volatile 关键字来修饰, 这个后面会介绍. 它会保证修改的值立即被更新到主存, 当其他线程要读取该值时会马上去主存读取最新的值.


#####有序性:

- 概念:

Java 内存模型中允许**编译器和处理器对指令进行重排序**, 不会影响到单线程执行的正确性, 但会影响到我们多线程并发执行的正确性.

- 实现方法:

可以通过 Volatile 来保证有序性. 还可以通过 synchronized 和 Lock 来保证每个时刻只有一个线程执行同步的代码, 从而来保证有序性. 


# <span id = "4">**4.在 Java 中常用的几种同步方法**</span>

### 4.1 ReentrantLock:

#####什么是 ReentrantLock(重入锁):

重入锁 ReentrantLook 就是支持重进入的锁, 它表示该锁能够支持一个线程的资源的重复加锁. 

#####重点类与方法:

- ReentrantLock 类:

重入锁的类

- lock()

**锁定**. 使程序进入临界区, 临界区就是在同一时刻只能有一个任务访问代码区. 一旦锁定, 任何其他线程都无法进入 Lock 语句.

- unlock()
**释放锁**. 解除锁定状态. 运行了 lock() 方法, 这个锁是一定要释放的, 一般把 unlock() 放在 finally 中. 假如在临界区发生了异常, 锁得不到释放, 其他线程则会被永远堵塞. 


- Condition 类

被称为**条件对象**. 进入临界区时, 却发现在某一个条件满足后, 它才能执行. 这时就可以使用条件对象来管理已经获得了锁, 却不能做有用的工作的线程. 条件对象又被称为条件变量.

- await()

调用该方法后, 当前线程被**阻塞, 并放弃锁**. 一个线程一旦调用了 await() , 它就会进入该条件的等待集并处于阻塞状态, 直到另一个线程调用了同一个条件对象的 signalAll() , 或 signal() 方法为止.

- signal()

随机**解除**这个条件对象的**某个线程的阻塞**. 注意, 如果解除了后该线程仍不能运行, 则再次被阻塞. 如果没有其他线程再次调用 signal() 就会造成 死锁.

- signalAll()

**解除**因为这个条件对象而等待的**所有线程**的阻塞.

注意, 调用 signalAll() 和 signal() **并不是立即激活一个等待的线程, 而是仅仅解除了线程的阻塞状态**, 以便这些线程在当前线程退出同步方法后, 通过竞争实现对对象的访问.

下面是一个模拟转账的例子:

```

public class Bank {
private double[] accounts;
	//锁对象
	private Lock bankLock;
	//条件对象
	private Condition condition;
	
	public Bank(int n,double initialBalance){ 
		accounts=new double[n];
		bankLock=new ReentrantLock(); 
		
		//得到条件对象 
		condition=bankLock.newCondition();
		
		for (int i=0;i<accounts.length;i++){
			accounts[i]=initialBalance; 
		} 
	} 
	
 public void transfer(int from,int to,int amount) throws InterruptedException {
	//锁定
	bankLock.lock();
	try{ 
		while (accounts[from]<amount){
		//阻塞当前线程，并放弃锁 
		condition.await(); 
		} 
	//转账的操作 
	... 
	//解除阻塞
	condition.signalAll();
	}finally {
		//释放锁
		bankLock.unlock();
	} 
 }


```


### 4.2 Synchronized 关键字(推荐):

使用关键字 Synchronized 使代码更简洁.在了解了重入锁之后理解这个关键字就不难了.

Lock和Condition接口为程序设计人员提供了高度的锁定控制，然而大多数情况下，并不需要那样的控制，并且可以使用一种嵌入到java语言内部的机制。

从Java1.0版开始，Java中的每一个对象都有一个内部锁。如果一个方法**用synchronized关键字声明，那么对象的锁将保护整个方法。synchorized是JVM实现的，可以通过监控工具来监控锁的状态，遇到异常JVM会自动释放掉锁。**

##### 使用


#####用 Synchronized 使代码更简洁.

```

public synchronized void method(){

}


```

等价于:

```

public void method(){
	this.lock.lock();
	try{

	}finally{
	
	this.lock.unlock();
	}
}


```


##### 用法:

1. 修饰方法

```

public synchronized void test(){

}


```



2. 修饰代码块


```

//获得了obj的锁, obj指的是一个对象.

synchronized(obj){

}

```


##### 常用方法:

1. wait()

等价于condition.await(); 

2. notifyAll() 

等价于condition.signalAll();


### 4.3 Volatile 关键字:

有时仅仅为了读写一个或者两个实例就是实使用同步的话, 未免有些开销过大. 而 volatile 关键字为实例域的同步访问提供了一个免锁机制. 如果一个域为 volatile ,那么编译器和虚拟机就知道该域可能被另一个线程并发更新的, 也就是具备了上面内存模型所说的 可见性和有序性.

#####概念:

也就是说当一个共享变量被 volatile 修饰之后, 就具备了两个含义:

1. 线程修改了变量的值时, 变量的新只对其他线程是立即可见的.(可见性) 

2. 禁止使用指令重排.(有序性)

#####特点:

1. volatile **不保证原子性**.

2. volatile 保证可见性.

3. volatile 保证有序性.

这种特点就说明, 不是每个地方都可以使用 volatile 来保证线程安全.

#####正确的使用 volatile 关键字:

synchronized 关键字是防止多个线程同时执行一段代码，那么就会很影响程序执行效率，而 volatile 关键字在某些情况下性能要优于 synchronized ，但是要注意 volatile 关键字是无法替代 synchronized 关键字的，因为 volatile 关键字无法保证操作的原子性。通常来说，使用 volatile 必须具备以下2个条件：

1. 对变量的写操作不依赖于当前值

2. 该变量没有包含在具有其他变量的不变式中

第一个条件就是不能是自增自减等操作，上文已经提到volatile不保证原子性。

第二个条件我们来举个例子它包含了一个不变式 ：下标总是小于或等于上标

```

public class NumberRange { 
	private volatile int lower, upper; 
	public int getLower() { 
		return lower; 
		
		} 
		
	public int getUpper() {
	return upper; 
	
	} 
	
	public void setLower(int value) { 
		if (value > upper)
			throw new IllegalArgumentException(...);
			lower = value; 
	} 
	
	public void setUpper(int value) { 
		if (value < lower) 
		throw new IllegalArgumentException(...); 
		upper = value; 
	} 
}


```

如果凑巧两个线程在同一时间使用不一致的值执行 setLower 和 setUpper 的话，则会使范围处于不一致的状态。例如，如果初始状态是 (0, 5)，同一时间内，线程 A 调用 setLower(4) 并且线程 B 调用 setUpper(3)，显然这两个操作交叉存入的值是不符合条件的，那么两个线程都会通过用于保护不变式的检查，使得最后的范围值是 (4, 3)
像上面这张情况就不可以使用 volatile 关键字, 可能会得出下标比上标大的情况, 这显然是和我们的条件不符合的.

#####正确的使用场景:

1. 状态标志:


```

volatile boolean shutdownRequested;
 ... 
 
 public void shutdown() {
 
	shutdownRequested = true; 
 
 } 
 
 public void doWork() { 
 
	while (!shutdownRequested) {
 
	// do stuff 
 
	} 
 
 }


```

很可能会从循环外部调用 shutdown() 方法 —— 即在另一个线程中 —— 因此，需要执行某种同步来确保正确实现 shutdownRequested 变量的可见性。然而，使用 synchronized 块编写循环要比使用volatile 状态标志编写麻烦很多。由于 volatile 简化了编码，并且状态标志并不依赖于程序内任何其他状态，因此此处非常适合使用 volatile。



2. 双重检查模式:


```

public class Singleton { 

	private volatile static Singleton instance = null; 
	
	public static Singleton getInstance() { 
	
		if (instance == null) { 
	
			synchronized(this) { 
	
			if (instance == null) { 
	
				instance = new Singleton(); 
	
			} 
		} 
	} 
	
	return instance; 
	
} 
} 


```


getInstance 方法中对 Singleton 进行了两次判空, 第一次是为了不必要的同步, 第二次是只有在 Singleton 等于 null 的情况下创建实例. 在这里用到了 volatile 关键字会或多或少的影响性能, 但是保证了程序的正确.