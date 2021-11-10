---
title: "java-multithreading"
date: 2021-11-03T10:23:05+08:00
draft: false
---

# 多线程与并发

## 线程与进程

进程：一个进程包括由操作系统分配的内存空间，包含一个或多个线程。一个进程一直运行，知道所有的非守护线程都结束运行后才结束。

线程：一条线程指的是进程中一个单一顺序的控制流，一个进程可以并发多个线程，每条线程并行执行不同的任务。一个线程不能独立存在，它必须是进程的一部分。

## 为什么需要多线程

1. cpu的计算速度很快

可以看一下这篇文章[《我是一个CPU：这个世界慢！死！了！》](https://blog.51cto.com/u_13188467/2065321)

现在的cpu都是多核的，如果程序只有一个线程，那么cpu永远都在等......

2. Java的执行模式是同步/阻塞（block）的

- 同步、异步：针对客户端。

- 同步：客户端请求后等待返回。应用程序执行一个系统调用，在系统调用没有完成，应用程序会一直阻塞。、

- 异步：客户端请求发出后，不用等待返回结果，执行下一步动作，当系统调用返回时，通过状态、通知来通知调用者，或通过回调函数处理这个调用。

- 阻塞、非阻塞：针对服务器。

- 阻塞调用：是指调用结果返回之前，当前线程会被挂起。函数只有在得到结果之后才会返回。

- 非阻塞调用：指在不能立刻得到结果之前，该函数不会阻塞当前线程，而会立刻返回。[原文链接](https://blog.csdn.net/ri_mu_xi_shan/article/details/79195068)

3. Java默认情况下只有一个线程
- 处理问题非常自然
- 但具有严重的性能问题

## 一个线程的生命周期
1. 新建状态

使用new以及Thread类或其子类创建一个线程对象

2. 就绪状态

使用start（）方法之后，线程进入就绪状态，等待JVM里的线程调度器的调度。

3. 运行状态

如果就绪状态的线程获取CPU资源，就可以执run（），此时线程便处于运行状态。

运行状态下的线程非常复杂，可以变为阻塞状态、就绪状态和死亡状态

4. 阻塞状态

如果一个线程执行了sleep（睡眠）、suspend（挂起）等方法，失去所占用资源之后，该线程就从运行状态进入阻塞状态。在睡眠时间已到或获得设备资源后可以重新进入就绪状态。可以分为三种：

- 等待阻塞：运行状态中的线程执行 wait() 方法，使线程进入到等待阻塞状态。
- 同步阻塞：线程在获取 synchronized 同步锁失败(因为同步锁被其他线程占用)。
- 其他阻塞：通过调用线程的 sleep() 或 join() 发出了 I/O 请求时，线程就会进入到阻塞状态。当sleep() 状态超时，join() 等待线程终止或超时，或者 I/O 处理完毕，线程重新转入就绪状态。

5. 死亡状态

一个运行状态的线程完成任务或者其他终止条件发生时，该线程切换到终止状态。

## 开启一个新的线程 Thread

线程难的本质原因是你要看着同一份代码，想象不同的人在疯狂地以乱序执行它

1. start方法并发执行
start方法来启动线程，真正实现了bai多线程运行，这时无需等待run方法体代码执行完毕而直接继续执行下面的代码。通过调用Thread类的
start()方法来启动一个线程，这时此线程处于就绪（可运行）状态，并没有运行，一旦得到cpu时间片，就开始执行run()方法，这里方法
run()称为线程体，它包含了要执行的这个线程的内容，Run方法运行结束，此线程随即终止。

2. 每多一个线程就多一个执行流

3. ⽅法栈(局部变量量)是线程私有的
4. 静态变量量/类变量量是被所有线程共享的

- 线程难的本质原因是：你要看着同⼀份代码，想象不同的人在疯狂地以乱序执行它。

## 创建线程的三种方法

1. Runnable

```java
class RunnableDemo implements Runnable {
   private Thread t;
   private String threadName;
   
   RunnableDemo( String name) {
      threadName = name;
      System.out.println("Creating " +  threadName );
   }
   
   public void run() {
      System.out.println("Running " +  threadName );
      try {
         for(int i = 4; i > 0; i--) {
            System.out.println("Thread: " + threadName + ", " + i);
            // 让线程睡眠一会
            Thread.sleep(50);
         }
      }catch (InterruptedException e) {
         System.out.println("Thread " +  threadName + " interrupted.");
      }
      System.out.println("Thread " +  threadName + " exiting.");
   }
   
   public void start () {
      System.out.println("Starting " +  threadName );
      if (t == null) {
         t = new Thread (this, threadName);
         t.start ();
      }
   }
}
 
public class TestThread {
 
   public static void main(String args[]) {
      RunnableDemo R1 = new RunnableDemo( "Thread-1");
      R1.start();
      
      RunnableDemo R2 = new RunnableDemo( "Thread-2");
      R2.start();
   }   
}
```

2. 继承Thread类本身

```java
class ThreadDemo extends Thread {
   private Thread t;
   private String threadName;
   
   ThreadDemo( String name) {
      threadName = name;
      System.out.println("Creating " +  threadName );
   }
   
   public void run() {
      System.out.println("Running " +  threadName );
      try {
         for(int i = 4; i > 0; i--) {
            System.out.println("Thread: " + threadName + ", " + i);
            // 让线程睡眠一会
            Thread.sleep(50);
         }
      }catch (InterruptedException e) {
         System.out.println("Thread " +  threadName + " interrupted.");
      }
      System.out.println("Thread " +  threadName + " exiting.");
   }
   
   public void start () {
      System.out.println("Starting " +  threadName );
      if (t == null) {
         t = new Thread (this, threadName);
         t.start ();
      }
   }
}
 
public class TestThread {
 
   public static void main(String args[]) {
      ThreadDemo T1 = new ThreadDemo( "Thread-1");
      T1.start();
      
      ThreadDemo T2 = new ThreadDemo( "Thread-2");
      T2.start();
   }   
}
```

3. 通过Callable和Future创建线程

```java
public class CallableThreadTest implements Callable<Integer> {
    public static void main(String[] args)  
    {  
        CallableThreadTest ctt = new CallableThreadTest();  
        FutureTask<Integer> ft = new FutureTask<>(ctt);  
        for(int i = 0;i < 100;i++)  
        {  
            System.out.println(Thread.currentThread().getName()+" 的循环变量i的值"+i);  
            if(i==20)  
            {  
                new Thread(ft,"有返回值的线程").start();  
            }  
        }  
        try  
        {  
            System.out.println("子线程的返回值："+ft.get());  
        } catch (InterruptedException e)  
        {  
            e.printStackTrace();  
        } catch (ExecutionException e)  
        {  
            e.printStackTrace();  
        }  
  
    }
    @Override  
    public Integer call() throws Exception  
    {  
        int i = 0;  
        for(;i<100;i++)  
        {  
            System.out.println(Thread.currentThread().getName()+" "+i);  
        }  
        return i;  
    }  
}
```

4. 三种方法的不同

- 采用实现 Runnable、Callable 接口的方式创建多线程时，线程类只是实现了 Runnable 接口或 Callable 接口，还可以继承其他类。
- 使用继承 Thread 类的方式创建多线程时，编写简单，如果需要访问当前线程，则无需使用 Thread.currentThread() 方法，直接使用 this 即可获得当前线程。

[阅读原文](https://www.runoob.com/java/java-multithreading.html)

## 多线程的应用场景
1. IO密集型
IO密集型指的是系统的CPU性能相对硬盘、内存要好很多，此时，系统运作，大部分的状况是CPU在等I/O (硬盘/内存) 的读/写操作，此时CPU Loading并不高。

2. CPU密集型
CPU密集型也叫计算密集型，指的是系统的硬盘、内存性能相对CPU要好很多，此时，系统运作大部分的状况是CPU Loading 100%，CPU要读/写I/O(硬盘/内存)，I/O在很短的时间就可以完成，而CPU还有许多运算要处理，CPU Loading很高。

3. 性能提升的上限

- 单核CPU 100%
- 多核CPU N*100%

## 昂贵的线程

1. 线程无法将性能无穷无尽的提升
2. 线程的昂贵性在于

- CPU切换上下文很慢
- 线程需要占用内存等系统资源

3. 如果用户很少

- new Thread().start()

4. 如果负载很高

- 使用线程池：JUC包

## 线程安全

#### 使用多线程付出的代价
1. 原子性

是指一个操作是不可中断的。即使是多个线程一起执行的时候，一个操作一旦开始，就不会被其他线程干扰。比如，对于一个静态全局变量int i，两个线程同时对它赋值，线程A给他赋值为1，线程B给他赋值为-1。那么不管这两个线程以何种方式。何种步调工作，i的值要么是1，要么是-1.线程A和线程B之间是没有干扰的。这就是原子性的一个特点，不可被中断。

2. 共享变量

3. 默认的实现几乎都不是线程安全的

#### 线程不安全的表现

1. 数据错误
2. 死锁
   - HashMap死锁问题
   - 预防死锁产生的原则：所有的线程都按照相同的顺序获得资源的锁

3. 死锁问题的排查

使用jps+jstack

4. [哲学家用餐](https://baike.baidu.com/item/哲学家就餐问题/10929794?fr=aladdin)

#### 线程安全的基本手段

1. 不可变类

- Integer/String/....


2. synchronized同步块

- synchronized（一个对象）把这个对象当成锁
- Static synchronized方法把Class对象当成锁
- 实例的synchronized方法把该实例当成锁
- Collections.synchronized

3. JUC包

- Atomiclnteger/...
- ConcurrentHashMap:任何使用HashMap有线程安全问题的地方都可以无脑使用
- ReentrantLock

## 其他
#### Object类里的线程方法

Object.wait()/notify()/notifyAll()方法
#### 线程池
线程池是预先定义好的若干个线程

#### Callable/Future
- 类比Runnable,Callable可以返回值，抛出异常
- Future代表一个“未来才会返回的结果”

#### 多线程经典问题：生产者/消费者模型

