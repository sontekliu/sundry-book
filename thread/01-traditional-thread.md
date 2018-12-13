# 传统线程技术

在 Java 的世界中，万事万物全是对象，当然线程也不例外，在 Java 中表示线程的类是 Thread，和创建其他对象一样，使用 `new` 关键字就可以创建线程，例如：`Thread thread = new Thread();`。创建了线程肯定是为了让线程去执行任务，线程执行任务是调用其 `start()` 方法，跟进源代码发现，`start()` 方法实际执行的是 `start0()`，而 `start0()` 方法是一个本地方法，根据我们以前学的知识可以知道，调用 `start()` 方法其实是系统底层启动新的线程去执行 `Thread` 里面的 `run()` 方法。

综上所知，创建线程并让其执行分两部，1. 创建线程 `new Thread()`；2. 让线程跑起来，调用 `start()`。

想让子线程去执行任务，如果使用传统的线程技术的话，有两种方式：

1. 继承 Thread 类，重写其 run 方法

```java
 // 创建线程对象
 Thread thread = new Thread(){
     @Override
     public void run() {
         while(true){
             try {
                 Thread.sleep(1000);
                 System.out.println("current thread name is :" + Thread.currentThread().getName());
                 // 获取当前线程可以使用 this，因为 this 对象就表示线程
                 System.out.println("this thread name is :" + this.getName());
             } catch (InterruptedException e) {
                 e.printStackTrace();
             }
         }
     }
 };

 // 启动线程，实际是执行 run 方法里面的代码
 thread.start();
```

2. 实现 Runable 接口，实现对应的 run 方法

```java
new Thread(new Runnable() {
    @Override
    public void run() {
        while(true){
            try {
                Thread.sleep(1000);
                System.out.println("current thread name is :" + Thread.currentThread().getName());
                // 此处不能使用 this，此时的 this 指的是 Runnable 对象，即所要执行的代码的宿主对象
                // System.out.println("当前线程名称是This：" + this.getName());
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}).start();
```

既然实现线程的方式有两种，那么这两种方式有什么区别呢，或者说哪一种更好？

第 2 种更符合面向对象的思想，线程是一个对象（Thread），线程所运行的代码是另外一个对象(Runnable)，这样就把线程和所运行的代码进行了解耦，更有利于程序的扩展。

看如下代码，请问输出内容是什么？

```java
new Thread(new Runnable() {
    @Override
    public void run() {
        while(true){
            try {
                Thread.sleep(1000);
                System.out.println("参数--当前线程名称是：" + Thread.currentThread().getName());
                // 此处不能使用 this，此时的 this 指的是 Runnable 对象，即所要执行的代码的宿主对象
                // System.out.println("当前线程名称是This：" + this.getName());
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}){
    @Override
    public void run() {
        while(true){
            try {
                Thread.sleep(1000);
                System.out.println("子类--当前线程名称是：" + Thread.currentThread().getName());
                // 此处不能使用 this，此时的 this 指的是 Runnable 对象，即所要执行的代码的宿主对象
                // System.out.println("当前线程名称是This：" + this.getName());
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}.start();
```

首先看清此代码的结构，即：`new Thread(runable){run()}.start()`，此代码是既重新了线程子类的 `run` 方法，同时还给 `Thread` 传递了 `Runnable` 参数，根据 `Java` 继承的思想可知，如果一个类的子类重写了父类的方法，那边就执行子类的方法，如果子类没有对应的方法，则执行父类的方法。由此可知先执行子类的 `run` 方法。
