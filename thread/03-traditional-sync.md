# 线程之间的通信

先看一个通信的题目：

> 子线程循环3次，接着主线程循环5次，接着又回到子线程循环3次，又回到主线程循环5次，如此循环 10 次。请写出代码。

说实话一看这个题目我的脑子是蒙的，完全没有思路。但仔细一想其实也不是很难。

首先，『子线程循环 3 次，主线程循环 5 次』，从这句话可以知道，子线程和主线程是互斥的，即子线程执行的时候不能被主线程打断，主线程执行的时候不能被子线程打断。其次，『接着又回到子线程循环3次，接着又回到主线程循环5次』从这句话可以知道子线程和主线程之间是有通信的，即子线程执行完之后要告诉主线程去执行，主线程执行完之后高速子线程去执行，到这你可能会问，那怎么告诉呢？学过 Java 线程的应该知道线程之间的通信是通过 `wait()` 和 `notify()` 方法来实现的。

这个题目其实挺高明的，既考察了线程的互斥，又考察了线程是如何通信的。考点分析清楚了，但是代码该如何写呢？还是不知如何下手。

既然考点分析清楚了，其实就等于完成了一半了。先从简单的开始，先实现线程的互斥。代码如下：

```java
public static void main(String[] args){
	// 子线程执行的任务
   new Thread(new Runnable() {
		Override
		public void run() {
			for (int i=0;i<10;i++){
				// 使用 synchronized 关键字实现线程的互斥作用
				synchronized (Object.class){
					for (int j=0;j<3;j++){
						System.out.println("sub thread run of " + j + " in loop of " + i);
					}
				}
			}
		}
	}).start();

	// 主线程执行的任务
	for (int i=0;i<10;i++){
		// 使用 synchronized 关键字实现线程的互斥作用
		synchronized (Object.class){
			for (int j=0;j<5;j++){
				System.out.println("main thread run of " + j + " in loop of " + i);
			}
		}
	}
}
```

如上代码使用 `synchronized` 关键字实现了子线程和主线程的互斥，但是要是实现线程之间的通信好像就比较困难了。并且有很多冗余代码，也不符合面向对象的思想。

考虑到用到共同数据（包括同步锁）或者有共同算法的若干个方法应该归在同一个类上，这样正好体现了程序的高内聚性。

根据以上原则，可以将子线程和主线程运行的代码归到一个类上面。代码修改如下：

```java
public class Bussiness{
    /**
     * 子线程运行的代码，使用 synchronized 进行互斥
     * @param i
     */
    public synchronized void sub(int i){
        for (int j = 0; j < 3; j++) {
            System.out.println("sub thread run of " + j + " in loop of " + i);
        }
    }

    /**
     * 主线程运行的代码，使用 synchronized 进行互斥
     * @param i
     */
    public synchronized void main(int i){
        for (int j=0;j<5;j++){
            System.out.println("main thread run of " + j + " in loop of " + i);
        }
    }
}
```

此时如果在实现线程间的通信应该很简单的了。完整实现代码如下：

```java
public class TraditionalThreadCommunication {

    public static void main(String[] args) {
        final Bussiness bussiness = new Bussiness();
        new Thread(new Runnable() {
            @Override
            public void run() {
                for (int i=0;i<10;i++){
                    bussiness.sub(i);
                }
            }
        }).start();

        for (int i=0;i<10;i++){
            bussiness.main(i);
        }
    }

    static class Bussiness{

        // 默认子线程开始运行
        boolean bShouldsub = true;
        /**
         * 子线程运行的代码，使用 synchronized 进行互斥
         * @param i
         */
        public synchronized void sub(int i){
            // 如果不该子线程运行，则子线程等待
            // 此处使用 while 是为了防止假唤醒
            while (!bShouldsub){
                try {
                    this.wait();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            // 如果该子线程运行，则执行代码
            for (int j = 0; j < 3; j++) {
                System.out.println("sub thread run of " + j + " in loop of " + i);
            }
            // 将该子线程运行的标识改成 false
            bShouldsub = false;
            // 唤醒主线程，通知该主线程运行了
            this.notify();
        }

        /**
         * 主线程运行的代码，使用 synchronized 进行互斥
         * @param i
         */
        public synchronized void main(int i){
            // 如果不该主线程运行，则主线程等待
            // 此处使用 while 是为了防止假唤醒
            while (bShouldsub){
                try {
                    this.wait();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            // 如果该主线程运行，则执行代码
            for (int j=0;j<5;j++){
                System.out.println("main thread run of " + j + " in loop of " + i);
            }
            // 将该子线程运行的标识改成 true
            bShouldsub = true;
            // 唤醒子线程，通知该子线程运行了
            this.notify();
        }
    }
}
```
































