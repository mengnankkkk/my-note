---
title: JUC-线程池
categories:
  - 技术栈
  
abbrlink: 2701
date: 2025.5.9
tags: 
   - java 
   - juc
---

# 前置知识

JUC是java.util.concurrent包的简称，在Java5.0添加，目的就是为了更好的支持**高并发**任务。让开发者进行多线程编程时减少竞争条件和死锁的问题！

进程：**一个运行中的程序的集合**; 一个进程往往可以包含多个线程,至少包含一个线程

Java默认有几个线程? 两个 main线程 gc线程

线程：线程（thread）是操作系统能够进行运算调度的最小单位。

并发(多线程操作同一个资源,交替执行)
CPU一核, 模拟出来多条线程,天下武功,唯快不破,快速交替
并行(多个人一起行走, 同时进行)
CPU多核,多个线程同时进行 ; 使用线程池操作

线程实现主要分为三类:**用户级线程(ULT)、内核级线程(KLT)和混合型线程**实现。

线程实现主要分为三类:用户级线程(ULT)、内核级线程(KLT)和混合型线程实现。轻量级进程(LWP)不是线程的实现方式,而是操作系统内核用来支持线程运行的一种机制。

分析三种线程实现方式:

\1. 用户级线程(ULT):
\- 线程的创建、调度和管理都由用户程序完成
\- 操作系统对线程一无所知
\- 优点是切换开销小,缺点是无法利用多处理器

\2. 内核级线程(KLT):
\- 线程的创建、调度和管理都由内核完成
\- 操作系统直接对线程进行调度
\- 优点是可以利用多处理器,缺点是系统调用开销大

\3. 混合线程:
\- 结合了ULT和KLT的优点
\- 用户级线程与内核级线程进行多对多映射
\- 既保证了系统调用的效率,又可以充分利用多处理器

线程的六种状态：

**new runnable blocked waiting time_waiting terminated**

**wait和sleep的区别：**

1. 来自不同的类： wait来自object类, sleep来自线程类
2. 关于锁的释放：wait会释放锁, sleep不会释放锁
3. 使用的范围不同： wait必须在同步代码块中， sleep可以在任何地方睡眠

## 线程的方法

新建线程调用**start()方法后**，线程并不会立即进入运行状态。线程的状态变化是：**新建→就绪→运行**。调用start()方法后，线程会进入就绪状态，等待CPU调度才能进入运行状态。这取决于线程调度器的调度策略。start()方法会**创建新的线程并执行run()方法**，而直接调用run()方法只会在**当前线程中执行**，不会启动新线程。

![](https://uploadfiles.nowcoder.com/images/20250422/59_1745314586904/AB85F6C1A80F74538F959A181704EA92)

run（）方法是定义线程执行体的核心方法。当我们创建一个线程类时,必须重写Thread类的run()方法或实现Runnable接口的run()方法,在其中编写具体的线程执行逻辑。

可以实现线程之间通知和唤醒的方法是：

Object.wait/notify/notifyAll

Condition.await/signal/signalAll

**shutdown方法：**shutdown()方法会让线程池进入"关闭"状态,**此时不再接受新的任务提交,但会继续执行队列中的任务直到完成。这是一种平缓的关闭方式。**

如果需要等待任务执行完成,需要配合使用a**waitTermination()**方法

shutdownNOw()立刻终止正在执行的所有任务，并返回等待执行的任务列表(List)。这些任务是尚未开始执行的任务。

**awaitTermination**(long timeout, TimeUnit unit)是阻塞方法,它会等待直到以下三种情况之一发生:
\- 所有任务执行完成
\- 到达指定的超时时间
\- 当前线程被中断
这个方法常用于确保线程池完全关闭。





# JUC结构

Java多线程实现有两种主要方式:**继承Thread类和实现Runnable接口**

继承Thread类
\- 直接继承Thread类
\- 重写run()方法
\- 创建线程对象后调用start()方法启动线程
\- 优点是编码简单直观
\- 缺点是Java不支持多继承,如果类已经继承了其他类就不能再继承Thread

实现Runnable接口
\- 实现Runnable接口
\- 实现run()方法
\- 将实现类实例传入Thread构造函数创建线程对象
\- 调用start()方法启动线程
\- 优点是可以避免单继承限制,更适合多个线程共享同一个资源的情况
\- **这是更常用的方式**

## tool

又称“信号量三兄弟”，分为三个

**CountDownLatch（闭锁）**

- 是一个同步辅助类，在**完成一组线程操作之前**，允许**一个或多个线程等待**。
- **典型应用：并发执行多个任务后统一汇总结果。**

**CyclicBarrier（栅栏）**

- 允许一组线程互相等待，直到**都达到一个公共屏障点**。
- **支持循环使用**（释放后可重用）。
- **典型应用：分布式计算中任务拆分后合并。**

**Semaphore（信号量）**

- 控制对某个资源的**访问线程数量**，相当于“共享锁”。
- 提供方法：
  - `acquire()`：尝试获取许可，**无则阻塞等待**。
  - `release()`：释放持有的许可。
- **典型应用：限制并发访问数，比如控制数据库连接池数量。**

## executor

是Java里面线程池的顶级接口，但它只是一个执行线程的工具，真正的线程池接口是**ExecutorService**，里面包含的类有：

1）ScheduledExecutorService 解决那些需要任务重复执行的问题

**周期性执行或者延迟任务**

2）ScheduledThreadPoolExecutor 周期性任务调度的类实现

**定时任务调度**

## atomic

是JDK提供的一组原子操作类，

包含有AtomicBoolean、AtomicInteger、AtomicIntegerArray等原子变量类，他们的实现原理大多是**持有它们各自的对应的类型变量value，而且被volatile关键字修饰了**。借助 `Unsafe` 类 + CAS 实现原子性。因为volatile并不能保证原子性，这样来保证每次一个线程要使用它都会拿到**最新的值。**

volatile变量在每次被线程访问时，**都强制从主内存中重新读取最新值，而不是使用线程工作内存中的值。这确保了变量的可见性。**

## locks

根据线程获取**锁的抢占机制**,锁可以分为**公平锁和非公平锁**。根据锁只能被单个线程**持有还是能被多个线程共同持有**，锁可以分为**独占锁和共享锁。**

当线程因为抢占式调度而停止运行时,**会被放入可运行队列的前面**。这是为了保证被抢占的线程能够优先得到下一次执行机会,**体现了调度的公平性。**

是JDK提供的锁机制，相比synchronized关键字来进行同步锁，功能更加强大，它为锁提供了一个框架，该框架允许更灵活地使用锁包含的实现类有：

1）**ReentrantLock** 它是**独占锁**，是指只能被独自占领，即同一个时间点只能被一个线程锁获取到的锁。可以被主动打断，可以调用**newCondition**方法实现多个条件变量。可以重入，有公平和非公平模式

2）**ReentrantReadWriteLock** 它包括子类ReadLock和WriteLock。ReadLock是共享锁，而WriteLock是独占锁。

🔍 **读锁（共享锁）**：多个线程可以**同时读**，不会互相影响。

✍️ **写锁（独占锁）**：写线程是**排他的**，写的时候不能读也不能写。

```java
public class ReadWriteLockDemo {

    public static void main(String[] args) {
        MyCache myCache = new MyCache();

        // 写线程
        for (int i = 1; i <= 5; i++) {
            final int temp = i;
            new Thread(() -> {
                myCache.put(temp + "", temp + "");
            }, "写线程" + i).start();
        }

        // 读线程
        for (int i = 1; i <= 5; i++) {
            final int temp = i;
            new Thread(() -> {
                myCache.get(temp + "");
            }, "读线程" + i).start();
        }
    }
}

// 缓存类，使用读写锁保证线程安全
class MyCache {
    private final Map<String, String> map = new HashMap<>();
    private final ReentrantReadWriteLock rwLock = new ReentrantReadWriteLock();

    // 写操作：加写锁
    public void put(String key, String value) {
        rwLock.writeLock().lock();
        try {
            System.out.println(Thread.currentThread().getName() + " 写入 " + key);
            TimeUnit.MILLISECONDS.sleep(300);
            map.put(key, value);
            System.out.println(Thread.currentThread().getName() + " 写入完成");
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            rwLock.writeLock().unlock();
        }
    }

    // 读操作：加读锁
    public void get(String key) {
        rwLock.readLock().lock();
        try {
            System.out.println(Thread.currentThread().getName() + " 读取 " + key);
            TimeUnit.MILLISECONDS.sleep(300);
            System.out.println(Thread.currentThread().getName() + " 读取完成: " + map.get(key));
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            rwLock.readLock().unlock();
        }
    }
}

```

3）**LockSupport** 它具**备阻塞线程和解除阻塞线程**的功能，并且不会引发死锁。



## collection

主要是提供线程安全的集合， 比如：

1）ArrayList对应的高并发类是**CopyOnWrite**ArrayList，

2）HashSet对应的高并发类是 CopyOnWriteArraySet，

3）HashMap对应的高并发类是ConcurrentHashMap

4）Queue对应的高并发类是ConcurrentLinkedQueue

这里还有一类特殊的集合，内部基于**`ReentrantLock`** 和 **`Condition`** 控制线程安全和阻塞操作。

它们都属于 **阻塞队列（Blocking Queue）的一种实现**，适用于多线程生产者-消费者模型。

两者构造方法中可以设置**容量上限**（有界）。

- `new LinkedBlockingQueue<>(1000)` 限定最大容量为 1000。

如果使用无参构造，默认容量是：`Integer.MAX_VALUE`，**理论上无界**。

所以 **从默认行为看是无界的**，但实际上 **可以设置为有界队列**。

------

LinkedBlockingQueue

- **阻塞队列，线程安全**
- **基于链表结构**
- **支持 FIFO（先进先出）操作**
- 插入满了会阻塞，移除空了也会阻塞
- **常用于生产者-消费者模型**
- 支持一个方向的插入和移除（头出尾进）。

使用 **`ReentrantLock`** 实现线程安全

使用两个锁：**takeLock、putLock**，避免入队和出队相互阻塞

LinkedBlockingDeque

**线程安全**

**基于链表结构**

**双端阻塞队列**（支持两端操作）

既可以作为 **队列（FIFO）**，也可以作为 **栈（LIFO）** 使用

能实现 **队列模型**（tail add，head remove）

也能实现 **栈模型**（head add，head remove）

# lock（top）

## synchronized

synchronized是Java中的关键字,是一种**内置的同步锁**，用于实现线程间的互斥访问。

可以修饰：：：：：

1. 修饰一个代码块，**被修饰的代码块称为同步语句块**，其作用的范围是大括号{}括起来的代码，**作用的对象是调用这个代码块的对象**；
　　2. 修饰一个方法，被修饰的方法称为**同步方法**，其作用的范围是整个方法，作用的对象是**调用这个方法的对象**；
　　3. 修改一个静态的方法，**其作用的范围是整个静态方法**，作用的对象是**这个类的所有对象；**
  4. 修改一个类，其作用的范围是synchronized后面括号括起来的部分，**作用主的对象是这个类的所有对象。**

JDK 提供的显示锁机制，使用时需显式地加锁和释放锁。

实现类：**`ReentrantLock`**

```java
// 构造器
public ReentrantLock() {
    sync = new NonfairSync();  // 默认是非公平锁
}
public ReentrantLock(boolean fair) {
    sync = fair ? new FairSync() : new NonfairSync();  // 可设置是否公平
}

```

| 类型     | 特点                               |
| -------- | ---------------------------------- |
| 公平锁   | **先进先出排队机制，谁先来谁先得** |
| 非公平锁 | **可能插队，性能更好（默认）**     |

实例：

```java
public class SaleTicketDemo {
    public static void main(String[] args) {
        Ticket ticket = new Ticket();
        new Thread(() -> { for (int i = 0; i < 40; i++) ticket.sale(); }, "a").start();
        new Thread(() -> { for (int i = 0; i < 40; i++) ticket.sale(); }, "b").start();
        new Thread(() -> { for (int i = 0; i < 40; i++) ticket.sale(); }, "c").start();
    }
}

class Ticket {
    private int ticketNum = 30;
    private Lock lock = new ReentrantLock();

    public void sale() {
        lock.lock();
        try {
            if (ticketNum > 0) {
                System.out.println(Thread.currentThread().getName() + " 购得第 " + ticketNum-- + " 张票, 剩余 " + ticketNum + " 张票");
            }
            Thread.sleep(10); // 模拟异常情况，增加线程交替
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
}

```

### **`synchronized` vs `Lock` 区别总结**(top)

| 比较点 | `synchronized` | `Lock`（如 `ReentrantLock`） |
| ------ | -------------- | ---------------------------- |
|        |                |                              |

| 本质 | Java 关键字 | Java 类（接口） |
| ---- | ----------- | --------------- |
|      |             |                 |

| 获取锁状态 | ❌ 无法判断 | ✅ 可以用 `tryLock()` 判断是否获取成功 |
| ---------- | ---------- | ------------------------------------- |
|            |            |                                       |

| 自动释放锁 | ✅ 出现异常时自动释放 | ❌ 必须手动 `unlock()`，否则易死锁 |
| ---------- | -------------------- | --------------------------------- |
|            |                      |                                   |

| 可中断性 | ❌ 不支持中断等待锁 | ✅ 支持（`lockInterruptibly()`） |
| -------- | ------------------ | ------------------------------- |
|          |                    |                                 |

| 公平性支持 | ❌ 默认非公平 | ✅ 可选公平/非公平 |
| ---------- | ------------ | ----------------- |
|            |              |                   |

| 适用范围 | 少量代码同步场景 | 更复杂、灵活的并发控制场景 |
| -------- | ---------------- | -------------------------- |
|          |                  |                            |

| 条件变量支持 | ❌ 不支持条件队列 | ✅ 支持多个 `Condition`，可精确唤醒 |
| ------------ | ---------------- | ---------------------------------- |
|              |                  |                                    |

| 特性 / 类型            | `synchronized`       | `Lock`（ReentrantLock）      | `ReadWriteLock`（ReentrantReadWriteLock） |
| ---------------------- | -------------------- | ---------------------------- | ----------------------------------------- |
| 是否可重入             | ✅ 是                 | ✅ 是                         | ✅ 是（读锁和写锁都支持）                  |
| 公平性选择             | ❌ 否（默认非公平）   | ✅ 可选（构造函数可设）       | ✅ 可选（构造函数可设）                    |
| 是否支持中断           | ❌ 不支持             | ✅ 支持 `lockInterruptibly()` | ✅ 支持（读/写锁都支持中断）               |
| 是否支持超时尝试获取锁 | ❌ 不支持             | ✅ 支持 `tryLock(timeout)`    | ✅ 支持                                    |
| 是否为读写分离         | ❌ 不支持             | ❌ 不支持                     | ✅ 支持，读锁共享，写锁独占                |
| 是否需要手动释放       | ❌ 不需要（自动释放） | ✅ 需要 `unlock()`            | ✅ 需要分别释放读锁和写锁                  |
| 性能                   | 中（JVM 优化较好）   | 高（更灵活，适合复杂控制）   | 非常高（读多写少的场景性能最好）          |
| 使用简单性             | ✅ 非常简单           | ⛔ 相对复杂（需要手动释放）   | ⛔ 更复杂（需区分读写锁）                  |

### `Condition` 精准线程通信（生产者-消费者模型）

| Object方法    | Condition对应方法 |
| ------------- | ----------------- |
| `wait()`      | `await()`         |
| `notify()`    | `signal()`        |
| `notifyAll()` | `signalAll()`     |

Condition是个接口，基本的方法就是await()和signal()方法；

调用Condition的await()和signal()方法，都必须在lock保护之内，就是说必须在**lock.lock()和lock.unlock**之间才可以使用

常用try tatch finally中

try中是条件，catch捕捉异常，finally执行解锁

实例：

```java
class SharedResource {
    private int number = 0;
    private Lock lock = new ReentrantLock();
    private Condition condition = lock.newCondition();

    public void increment() {
        lock.lock();
        try {
            while (number != 0) {
                condition.await();//wait
            }
            number++;
            System.out.println(Thread.currentThread().getName() + " >> " + number);
            condition.signalAll();//唤醒其他等待线程
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();// 必须释放锁
        }
    }

    public void decrement() {
        lock.lock();
        try {
            while (number != 1) {
                condition.await();
            }
            number--;
            System.out.println(Thread.currentThread().getName() + " >> " + number);
            condition.signalAll();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
}

```

线程之间使用 `Condition` 精准控制等待和唤醒

# 线程共享和线程安全

## 线程安全

### list

使用 `Vector`（线程安全，效率低）

```
List<String> list = new Vector<>();
```

使用同步包装器（JDK 提供的工具类）

```
List<String> list = Collections.synchronizedList(new ArrayList<>());
```

使用 **CopyOnWriteArrayList** ✅ 推荐！

```
List<String> list = new CopyOnWriteArrayList<>();

```

**核心机制：写时复制（Copy-On-Write，简称 COW）**

- 写数据时：先复制出一个新数组，把数据添加到新数组，**然后再用新数组替换原数组**
- 读数据时：直接读旧数组，不会受写操作影响，做到了**读写分离**

### set

`HashSet` 底层就是 `HashMap`，而 `HashMap` 线程不安全，因此 `HashSet` 也不安全

使用Collections.synchronizedSet

```
Set<String> set = Collections.synchronizedSet(new HashSet<>());
```

使用 CopyOnWriteArraySet（线程安全）

```
Set<String> set = new CopyOnWriteArraySet<>();
```

### hashmap

不是线程安全的，并发环境中，`HashMap` 容易出现：

- 死循环（JDK 1.7）
- 数据丢失
- 键值覆盖

推荐使用ConcurrentHashMap

```
Map<String, String> map = new ConcurrentHashMap<>();
```

分段锁机制（JDK 1.7）→ JDK 1.8 后使用 **CAS + synchronized** 优化

支持高并发场景，性能远优于 `Hashtable`

或者是使用`Collections.synchronizedMap`

还有Hashtable

老版本的 Map 实现，**线程安全**，所有方法都被 `synchronized` 修饰。

不允许 **null 键或 null 值**。

**不保证顺序**。

已被 `ConcurrentHashMap` 替代，在现代项目中已很少使用。

### 单线程

**线程环境是线程安全的**（因为没有竞争）

**单例模式中的双重检查（DCL）**：

- 需要 `volatile` 修饰变量，防止 **指令重排序**。

**SimpleDateFormat 是非线程安全的**：

当多个线程同时使用同一个SimpleDateFormat对象时,可能会导致解析和格式化错误。这是因为SimpleDateFormat的设计中包含了可变的成员变量,在多线程环境下会相互影响。

- 推荐：每线程一个实例 or 使用 `ThreadLocal<SimpleDateFormat>`。

## 线程共享

**线程共享区域：**

- **方法区**：类型信息、静态变量、常量等。
- **Java 堆**：对象实例的主要内存区域。

**线程私有区域：**

- **程序计数器**：记录当前线程执行位置。
- **虚拟机栈**：每个线程有独立方法栈，存局部变量等信息。







# callable

多线程的另一种实验方式

**还可以使用使用Callable接口**，Callable接口的**call()方法确实可以返回值，并且能够抛出异常**。这是它区别于Runnable接口run()方法的重要特征。run()方法既不能返回值，也不能抛出受检异常。

实例：

```java
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.FutureTask;

public class CallableTest {

    public static void main(String[] args) throws ExecutionException, InterruptedException {
        // 创建一个 FutureTask，包装一个 Callable 对象
        FutureTask<Integer> futureTask = new FutureTask<>(new MyThread());

        // 启动线程，实际执行的是 futureTask.run()，内部会调用 call() 方法
        new Thread(futureTask, "a").start();

        // 阻塞等待 call() 执行结果返回
        System.out.println(futureTask.get());
    }
}

// 自定义一个线程任务，实现 Callable 接口
class MyThread implements Callable<Integer> {

    @Override
    public Integer call() throws Exception {
        System.out.println(Thread.currentThread().getName() + " 正在执行");
        return 123;
    }
}

```

| 特性 | Runnable | Callable |
| ---- | -------- | -------- |
|      |          |          |

| 是否有返回值 | ❌ 没有 | ✅ 有返回值 |
| ------------ | ------ | ---------- |
|              |        |            |

| 是否抛异常 | ❌ 不能抛 checked 异常 | ✅ 可以抛异常 |
| ---------- | --------------------- | ------------ |
|            |                       |              |

| 方法名 | `run()` | `call()` |
| ------ | ------- | -------- |
|        |         |          |

| 执行方式 | `Thread` / `ExecutorService` | 一般配合 `FutureTask` 使用 |
| -------- | ---------------------------- | -------------------------- |
|          |                              |                            |

## `FutureTask`

- 是 `Runnable` + `Future` 的实现类
- 可以传入一个 `Callable`，通过 `Thread` 执行它
- 可以用 `.get()` 获取返回值，**该方法会阻塞，直到任务执行完毕**

```java
public class BigStar {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        Callable<String> callable = new Test(100);
        FutureTask<String> futureTask = new FutureTask<>(callable);
        new Thread(futureTask).start();
        String rs = futureTask.get();
        System.out.println(rs);

    }
}
```



# 辅助类--tool

## `CountDownLatch`（倒计时锁）

```java
CountDownLatch latch = new CountDownLatch(5); // 初始化计数器为5

for (int i = 0; i < 5; i++) {
    new Thread(() -> {
        // 执行完后调用 countDown
        latch.countDown();
    }).start();
}

latch.await(); // 等待计数器归零
System.out.println("main线程继续执行");

```

`countDown()`：每调用一次，计数器减1。

`await()`：阻塞等待，直到计数器为0再往下执行。

多个子任务完成后主线程才执行（比如：并行加载资源 → 全部完成 → 主线程继续）。

## `CyclicBarrier`（循环屏障）

```java
CyclicBarrier barrier = new CyclicBarrier(7, () -> {
    System.out.println("召唤神龙成功！");
});

for (int i = 0; i < 7; i++) {
    new Thread(() -> {
        barrier.await(); // 等待所有线程到齐
    }).start();
}

```

所有线程都调用 `await()`，等数量满后触发回调（可选），**同时释放所有线程**。

**可以重复使用（循环屏障）**。

多个线程**并发阶段性同步**，如：多线程分批写入数据库，每批完成后统一提交。

## `Semaphore`（信号量）

```java
Semaphore semaphore = new Semaphore(3); // 同时只能有3个线程访问

for (int i = 0; i < 6; i++) {
    new Thread(() -> {
        semaphore.acquire(); // 请求许可（占坑）
        // 执行业务逻辑
        semaphore.release(); // 释放许可（离开）
    }).start();
}

```

`acquire()`：申请资源，没资源就阻塞。

`release()`：释放资源，唤醒等待的线程。

控制同时访问资源的线程数，比如：限流、数据库连接池、停车场3个车位6辆车示例等。

**CyclicBarrier和CountDownLatch**确实都可以让**一组线程等待其他线程**。CyclicBarrier用于让一组线程互相等待，直到所有线程都到达某个公共屏障点。CountDownLatch则允许一个或多个线程等待其他线程完成一组操作。



# 阻塞

## 概念

- **接口**：`java.util.concurrent.BlockingQueue<E>`
- **特点**：当队列空时，取元素的操作会**阻塞等待**；当队列满时，存元素的操作会**阻塞等待**。
- **典型场景**：生产者–消费者模型、线程池内部任务队列、并发限流（令牌桶）等。

队满队空的时候会阻塞

## 四类API

| 分组            | 插入（Add）         | 移除（Remove）           | 检查（Examine） |
| --------------- | ------------------- | ------------------------ | --------------- |
| **1. 抛异常**   | `add(e)`            | `remove()` / `remove(e)` | `element()`     |
| **2. 特殊值**   | `offer(e)`          | `poll()`                 | `peek()`        |
| **3. 阻塞**     | `put(e)`            | `take()`                 | —               |
| **4. 超时阻塞** | `offer(e, timeout)` | `poll(timeout)`          | —               |

抛异常组：

**`add(e)`**：队满时抛 `IllegalStateException`

**`remove()`**：队空时抛 `NoSuchElementException`

**`element()`**：队空时抛 `NoSuchElementException`

特殊值组：

**`offer(e)`**：队满时返回 `false`，否则返回 `true`

**`poll()`**：队空时返回 `null`

**`peek()`**：队空时返回 `null`

阻塞组：

**`put(e)`**：队满时**阻塞**直到有空间

**`take()`**：队空时**阻塞**直到有元素

超时阻塞组：

- **`offer(e, timeout, unit)`**：等待指定时间仍满则返回 `false`
- **`poll(timeout, unit)`**：等待指定时间仍空则返回 `null`

## 常见实现类

| 实现类                  | 特点                                       |
| ----------------------- | ------------------------------------------ |
| `ArrayBlockingQueue`    | **有界**阻塞队列，基于数组，**FIFO**       |
| `LinkedBlockingQueue`   | 可选有界/无界，基于双向链表，**吞吐量高**  |
| `PriorityBlockingQueue` | **无界**，支持**优先级排序**，不允许null   |
| `DelayQueue`            | **延迟队列**，元素只有到期后才能取出       |
| `SynchronousQueue`      | **零容量**队列，每个 `put` 必须等待 `take` |
| `LinkedTransferQueue`   | 支持 **transfer** 操作的无界队列           |

**SynchronousQueue：**

**容量为 0**，不存储元素。

每一次 `put(e)` 必须等待另一个线程来 `take()`，否则**一直阻塞**。

典型用于**直接交替**的场景，例如：线程间**直接交换数据**，或线程池的手动切换策略。

```java
SynchronousQueue<String> queue = new SynchronousQueue<>();
// 生产线程
new Thread(() -> {
    System.out.println("T1 put 1");
    queue.put("1");   // 阻塞直到被取
    System.out.println("T1 put 2");
    queue.put("2");
}, "T1").start();

// 消费线程
new Thread(() -> {
    TimeUnit.SECONDS.sleep(3);
    System.out.println("T2 take=> " + queue.take());
    TimeUnit.SECONDS.sleep(3);
    System.out.println("T2 take=> " + queue.take());
}, "T2").start();

```

意思就是生成完不消费的话，就会一直block

## 小结

**线程池**：`ThreadPoolExecutor` 内部使用 `BlockingQueue<Runnable>` 来缓存任务。

**生产者–消费者**：高效的线程间传递数据。

**限流/令牌桶**：控制并发量或速率。

**直接交互**：`SynchronousQueue` 做快速交替传递。

# TLS

TLS(线程局部存储)是一种特殊的存储机制，它为每个线程提供**独立的变量副本**

避免多线程访问冲突

每个线程只能访问自己的变量副本，**其他线程无法访问**。

注意：如果变量操作涉及多个步骤或访问共享资源，**仍然需要同步机制**。

Java 中的 **`ThreadLocal`** 是 TLS 的具体实现。

## ThreadLocal

`ThreadLocal` 是 Java 实现 **线程本地存储** 的重要机制。设计的目的是为了保证线程安全

每个线程内部维护一个 **`ThreadLocalMap` 哈希表**，存储自己的变量副本。

本质：**避免多线程共享数据**，每个线程有自己的数据副本，实现数据隔离。

每个线程访问ThreadLocal变量时，实际是在**操作自己的ThreadLocalMap中的副本。**

`ThreadLocal`：

- **不是继承 Thread 类**
- **不实现 Runnable 接口**

使用了 **开放定址法** 解决哈希冲突。





# 线程池

就是一个可以**复用线程**的技术

原理：

分为工作线程和任务队列两部分

任务是一个实现Runnable或者Callable的对象

会控制任务的数量和线程的数量，不会占用过多的资源

## 3大创建方式

| 方法                                  | 含义           | 特点                                         |
| ------------------------------------- | -------------- | -------------------------------------------- |
| `Executors.newFixedThreadPool(int n)` | 固定大小线程池 | 核心线程数固定，任务队列无界，**容易 OOM**   |
| `Executors.newSingleThreadExecutor()` | 单线程线程池   | 一个核心线程，顺序执行，**串行任务执行**     |
| `Executors.newCachedThreadPool()`     | 可伸缩线程池   | 无限线程数，有空线程就复用，**容易线程爆炸** |

这三种方法不推荐在生产使用！因为：

- 队列是无界的，容易导致 **内存溢出**
- 缺少参数控制，维护困难

## 手动创建

```java
public ThreadPoolExecutor(
    int corePoolSize,              // 1. 核心线程数
    int maximumPoolSize,           // 2. 最大线程数
    long keepAliveTime,            // 3. 线程最大空闲时间
    TimeUnit unit,                 // 4. 时间单位
    BlockingQueue<Runnable> workQueue, // 5. 任务队列
    ThreadFactory threadFactory,   // 6. 线程工厂（可选：自定义命名）
    RejectedExecutionHandler handler // 7. 拒绝策略
)
```

```java
public class BigStar {
    public static void main(String[] args) {
        // 创建线程池，核心线程数为2，最大线程数为4，保持空闲线程时间为60秒，任务队列容量为10
        ThreadPoolExecutor executor = new ThreadPoolExecutor(
                2, // 核心线程数
                4, // 最大线程数
                60, // 线程空闲保持时间
                TimeUnit.SECONDS, // 时间单位
                new ArrayBlockingQueue<>(10) // 阻塞队列容量为10
        );

        // 提交任务到线程池
        executor.execute(() -> {
            for (int i = 0; i <= 5; i++) {
                System.out.println("aaa" + i);
            }
        });

        executor.execute(() -> {
            for (int i = 0; i <= 5; i++) {
                System.out.println("bbb" + i);
            }
        });

        executor.execute(() -> {
            for (int i = 0; i <= 5; i++) {
                System.out.println("ccc" + i);
            }
        });

        // 关闭线程池，防止新任务提交，但会执行完已提交的任务
        executor.shutdown();

        // 等待线程池中任务完成
        try {
            if (!executor.awaitTermination(60, TimeUnit.SECONDS)) {
                executor.shutdownNow();
            }
        } catch (InterruptedException e) {
            executor.shutdownNow();
        }
    }
}
```

实战模板：

```java
ExecutorService threadPool = new ThreadPoolExecutor(
    2, // corePoolSize
    5, // maximumPoolSize
    3, TimeUnit.SECONDS, // keepAliveTime
    new LinkedBlockingQueue<>(3), // workQueue 容量为3
    Executors.defaultThreadFactory(), // threadFactory
    new ThreadPoolExecutor.CallerRunsPolicy() // 拒绝策略
);

```

注意事项：

- **临时线程什么时候创建**？新任务提交的时候核心线程都在忙，任务队列满了，并还可以创建新的临时线程，才会创建临时线程
- **什么时候会开始拒绝新任务**？核心线程和临时线程都在忙，任务队列也满了，新的任务才会开始拒绝新任务

核心线程数量应该是多少？

- 计算密集的 cpu+1
- IO密集的 cpu*2

## 关闭策略

shutdown()方法会让线程池进入"关闭"状态,此时不再接受新的任务提交,但会继续执行队列中的任务直到完成。这是一种**平缓的关闭方式。**

shutdownNow()方法会**尝试终止所有正在执行的任务,并返回等待执行的任务列表(List)**。这些任务是尚未开始执行的任务。

awaitTermination(long timeout, TimeUnit unit)是阻塞方法,它会等待直到以下三种情况之一发生:
\- 所有任务执行完成
\- 到达指定的超时时间
\- 当前线程被中断
这个方法常用于**确保线程池完全关闭**。

## 四大拒绝策略

| 策略类                | 策略说明                               |
| --------------------- | -------------------------------------- |
| `AbortPolicy`（默认） | 抛出 `RejectedExecutionException` 异常 |
| `CallerRunsPolicy`    | **谁提交谁执行**（回退给调用线程）     |
| `DiscardPolicy`       | 直接丢弃任务，不抛异常                 |
| `DiscardOldestPolicy` | 丢弃最早的任务，然后尝试加入新任务     |

## 常见面试问题

**线程池执行流程**：

```java
if (当前线程数 < corePoolSize)
    创建新线程执行任务
else if (队列未满)
    放入阻塞队列
else if (当前线程数 < maximumPoolSize)
    创建非核心线程处理
else
    启动拒绝策略

```

**为什么不用 Executors 创建线程池？**

`Executors.newFixedThreadPool` 和 `newSingleThreadExecutor` 使用的都是**无界阻塞队列**（如 `LinkedBlockingQueue`），在任务积压时会无限添加任务导致内存溢出（OOM）

`newCachedThreadPool` 使用的是**无界线程数**，短时间大量任务可能创建海量线程，导致 **CPU 被打爆或内存溢出**。因此生产推荐使用 `ThreadPoolExecutor` 并指定参数

用手动创建更好

**核心线程与最大线程的区别？**

`corePoolSize` 是核心线程数，线程池创建后**优先使用核心线程**；核心线程默认是常驻线程，即使空闲也不回收（除非设置 `allowCoreThreadTimeOut(true)`）。
`maximumPoolSize` 是线程池能容纳的**最大线程数**，当核心线程+队列都满后才会创建非核心线程，超过该数量后再有任务就触发拒绝策略。

**线程池如何避免资源耗尽？**

1）合理设置 `corePoolSize` 和 `maximumPoolSize`；
2）合理设置队列大小（如 `ArrayBlockingQueue`）；
3）选择合适的 `RejectedExecutionHandler` 拒绝策略（如 `CallerRunsPolicy` 可回退执行）；
4）限制任务提交速率（限流）或增加监控报警机制。

**线程池为什么需要 keepAliveTime？**

`keepAliveTime` 用于控制**非核心线程的生存时间**：线程在空闲超过这个时间后会被回收，避免线程长时间闲置浪费资源。可通过 `allowCoreThreadTimeOut(true)` 让核心线程也参与超时机制，提升资源利用率。

**如何定位线程池满的情况？**

可从以下几个方面进行排查：
1）日志中会抛出 `RejectedExecutionException`（默认拒绝策略）；
2）设置自定义拒绝策略中记录日志或告警；
3）使用线程池监控工具如 **Arthas**、**JFR (Java Flight Recorder)**、**VisualVM**，查看线程池状态：活跃线程数、任务队列长度等；
4）通过 `ThreadPoolExecutor` 的 `getPoolSize()`、`getActiveCount()`、`getQueue().size()` 等方法动态采集指标并写入监控系统（如 Prometheus + Grafana）。

