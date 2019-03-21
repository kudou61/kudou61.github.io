---
title: Java多线程
tags:
  - 笔记
categories:
  - Java学习
---

Java 多线程实现方式主要有三种：继承 Thread 类，实现 Runnable，使用 ExecutorService、Callable、Future 实现有返回结果的多线程。其中前两种没有返回值，只有最后一种是带返回值的。

# 线程状态类型

1. 新建状态（New）：新创建了一个线程对象。
2. 就绪状态（Runnable）：线程对象创建后，其他线程调用了该对象的 start()方法。该状态的线程位于可运行线程池中，变得可运行，等待获取 CPU 的使用权。
3. 运行状态（Running）：就绪状态的线程获取了 CPU，执行程序代码。
4. 阻塞状态（Blocked）：阻塞状态是线程因为某种原因放弃 CPU 使用权，暂时停止运行。直到线程进入就绪状态，才有机会转到运行状态。阻塞的情况分三种：
   - 等待阻塞：运行的线程执行 wait()方法，JVM 会把该线程放入等待池中。
   - 同步阻塞：运行的线程在获取对象的同步锁时，若该同步锁被别的线程占用，则 JVM 会把该线程放入锁池中。
   - 其他阻塞：运行的线程执行 sleep()或 join()方法，或者发出了 I/O 请求时，JVM 会把该线程置为阻塞状态。当 sleep()状态超时、join()等待线程终止或者超时、或者 I/O 处理完毕时，线程重新转入就绪状态。
5. 死亡状态（Dead）：线程执行完了或者因异常退出了 run()方法，该线程结束生命周期。

# 继承 Thread 类实现多线程

Thread 的本质上也是实现了 Runnable 接口的一个实例，它代表一个线程的实例，并且，启动线程的方法就是通过 Thread 类的 start()方法，start()方法是一个 native 方法，它将启动一个新线程，并执行 run 方法。这种方式实现多线程的方式就是 extend Thread，并覆写 run()方法。例如

```java
public class MyThread extend Thread{
		public void run(){
		//to do
	}
}
```

启动多线程的方式如下：

```java
MyThread mt = new MyThread();
mt.start();
```

# 实现 Runnable 接口

如果自己的类已经 extend 另一个类，就不能 extend Thread 类，此时，必须实现一个 Runnable 接口

```java
public class MyThread extend OtherClass implement Runnable{
	public void run(){
	// to do
	}
}
```

为了启动 MyThread，首先需要实例化一个 Thread，并传入实现了 Runnable 接口的类的实例

```java
MyThread mt = new MyThread();
Thread thread = new Thread(mt);
thread.start();
```

# 使用 ExecutorService、Callable、Future 实现

可返回值的线程必须实现 Callable 接口，执行 Callable 任务后，可以获取一个 Future 对象，在该对象上调用 get 就可以获取到 Callable 任务返回的 Object，再结合线程池接口 ExecutorService 就可以实现有返回结果的多线程了。

```java
import java.util.concurrent.*;
import java.util.Date;
import java.util.List;
import java.util.ArrayList;

/**
* 有返回值的线程
*/
@SuppressWarnings("unchecked")
public class Test {
public static void main(String[] args) throws ExecutionException,
    InterruptedException {
   System.out.println("----程序开始运行----");
   Date date1 = new Date();

   int taskSize = 5;
   // 创建一个线程池
   ExecutorService pool = Executors.newFixedThreadPool(taskSize);
   // 创建多个有返回值的任务
   List<Future> list = new ArrayList<Future>();
   for (int i = 0; i < taskSize; i++) {
    Callable c = new MyCallable(i + " ");
    // 执行任务并获取Future对象
    Future f = pool.submit(c);
    // System.out.println(">>>" + f.get().toString());
    list.add(f);
   }
   // 关闭线程池
   pool.shutdown();

   // 获取所有并发任务的运行结果
   for (Future f : list) {
    // 从Future对象上获取任务的返回值，并输出到控制台
    System.out.println(">>>" + f.get().toString());
   }

   Date date2 = new Date();
   System.out.println("----程序结束运行----，程序运行时间【"
     + (date2.getTime() - date1.getTime()) + "毫秒】");
}
}

class MyCallable implements Callable<Object> {
private String taskNum;

MyCallable(String taskNum) {
   this.taskNum = taskNum;
}

public Object call() throws Exception {
   System.out.println(">>>" + taskNum + "任务启动");
   Date dateTmp1 = new Date();
   Thread.sleep(1000);
   Date dateTmp2 = new Date();
   long time = dateTmp2.getTime() - dateTmp1.getTime();
   System.out.println(">>>" + taskNum + "任务终止");
   return taskNum + "任务返回运行结果,当前任务时间【" + time + "毫秒】";
}
}
```
