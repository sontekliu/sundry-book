# 聊聊线程 — 线程状态

如果在网上搜索线程的状态，那么肯定有很多资料，但是答案未必精确，下面就基于 JDK 源码说一下线程的状态。

在 Thread 类中有一个枚举表明了线程的状态，根据此枚举可知，线程有 6 种状态，分别为：`NEW`、`RUNNABLE`、`BLOCKED`、`WAITING`、`TIMED_WATIING`、`TERMINATED`。

#### NEW

线程尚未启动时的状态，即未执行 start() 方法。

#### RUNNABLE

线程处于可运行状态。此状态的线程正在 Java 虚拟机中执行，也可能正在等待操作系统的其他资源，如 CPU。

#### BLOCKED

线程等待监视器锁而处理阻塞状态。处于阻塞状态的线程正在等待一个监视器锁进入同步代码块或者同步方法。或者因为调用了 Object.wait() 方法而重新进入阻塞状态。

#### WAITING

线程调用了如下方法就处于等待状态，方法如下：
> Object.wait() 方法，没有 timeout 参数  
> Thread.join() 方法，没有 timeout 参数  
> LockSupport.park 方法

处于等待状态的线程等待另一个线程执行一个特定的动作。   
例如：  
一个线程调用了对象的 wait() 方法，则此线程就一直等待其他的线程去调用该对象的 notify() 或者 notifyAll() 方法。

一个线程调用了 Thread.join() 方法，则此线程一直等待直到调用 `join()` 方法的线程终止。
 
#### TIMED_WAITING

此状态与 WAITING 状态类似，此状态是指如果线程调用了如下方法则处于指定时间范围内的等待状态。方法有：
> Thread.sleep(long) 方法，有 timeout 参数   
> Object.wait(long) 方法，有 timeout 参数    
> Thread.join(long) 方法，有 timeout 参数  
> LockSupport.parkNanos 方法  
> LockSupport.parkUntil 方法  

#### TERMINATED

线程终止状态，当线程完成任务或者抛出异常，线程就处于终止状态。











































































