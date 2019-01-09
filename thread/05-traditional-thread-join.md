# 源码加示例分析线程的 join 方法

炸一看到 join() 方法可能有点懵，不知道此方法是干啥的。下面从源码的角度分析一下此方法，并结合相应的示例彻底搞懂 join() 方法。

**首先我们应该知道 join() 方法是定义在 Thread 类里面的，所以调用者必须也是一个线程类，必须先调用 start 方法再调用 join 方法**  
例如：

```java
Thread t = new CustomThread();
t.start();   # 注意要先调用 start 再调用 join
t.join();
```

代码示例如下：

```java
public static void main(String[] args) throws InterruptedException {
    System.out.println("Main started");

    Thread t1 = new Thread(){
        @Override
        public void run() {
            for (int i=1;i<=5;i++){
            	try {
                	Thread.sleep(500);
                } catch (InterruptedException e) {
                  	e.printStackTrace();
                }
                System.out.println("t1 print " + i);
            }
        }
    };

    t1.start();
    t1.join();
    System.out.println("Main over");
}

输出如下：
Main started
t1 print 1
t1 print 2
t1 print 3
t1 print 4
t1 print 5
Main over
```

有输出内容可知，主线程开始输出，然后再等待 t1 线程执行完成，最后主线程结束。但是 `t1.join()` 是如何保证先执行 t1 线程的呢？我们从源码角度看一下。

下面先看一下源码：

```java
/**
 * Waits for this thread to die.
 *
 * <p> An invocation of this method behaves in exactly the same
 * way as the invocation
 *
 * <blockquote>
 * {@linkplain #join(long) join}{@code (0)}
 * </blockquote>
 *
 * @throws  InterruptedException
 *          if any thread has interrupted the current thread. The
 *          <i>interrupted status</i> of the current thread is
 *          cleared when this exception is thrown.
 */
public final void join() throws InterruptedException {
    join(0);
}
```

由此源码可知 join 方法的作用是：**等待该线程终止。** 并且实际调用的方法是 join(0)，join(0) 的源码是：

```java
public final synchronized void join(long millis) throws InterruptedException {
    long base = System.currentTimeMillis();
    long now = 0;

    if (millis < 0) {
        throw new IllegalArgumentException("timeout value is negative");
    }

    if (millis == 0) {
        while (isAlive()) {
            wait(0);
        }
    } else {
        while (isAlive()) {
            long delay = millis - now;
            if (delay <= 0) {
                break;
            }
            wait(delay);
            now = System.currentTimeMillis() - base;
        }
    }
}
```

结合源码和示例分析可知，当程序执行到 `t1.start()` 方法时，t1 线程进入到 `RUNNABLE` 状态，但是 t1 未必获得到 CPU 的资源，所以 t1 未必立马执行。  
分两种情况分析：  
**第一，**假如 t1 获得 CPU 资源并立马执行，且在执行过程中 CPU 一直未切换，输出结果很合乎情理，为了避免这种情况，我故意添加了 Thread.sleep(500) 方法，防止 CPU 资源一直被 t1 占用的情况。

**第二，**t1 未获得 CPU 资源，而是由主线程即 main 方法所在的线程获得 CPU 资源，继续执行 `t1.join()` 方法，由 join(long millis) 方法的源码可知，此方法是一个同步方法，并且使用的锁是该线程对象本身。此时只有主线程访问，没有竞争的情况，所以主线程获得锁，继续执行到 `while(isAlive())` 时，由于 t1 目前还在 JVM 虚拟机中，结果肯定为 true，所以主线程继续执行到 wait(0) 方法，此时主线程释放锁，并且处于 `WATING` 状态。我们都知道处于 `WAITING` 状态的线程如果有其他线程调用了 notify() 或者 notifyAll() 方法时，此线程才有 `WAITING` 状态转变为 `RUNNABLE` 状态，而此示例代码中并没有任何线程调用 notfiy() 和 notifyAll() 方法，所以 CPU 资源再次由 t1 线程获得并执行代码，输出 5 条打印内容，t1 线程结束变为 `TERMINATED` 状态。此时 CPU 的资源又切换到主线程，此时 isAlive() 方法返回 false，join() 方法执行结束，最后主线程输出 Main over。

由以上分析可知：  

1. join() 确实是等待该线程终止，具体一点就是调用该线程(t1)的线程(主线程)，必须要等到该线程执行完成之后再执行，以上示例仅仅是以主线程和子线程为示例。


























