


> Written with [StackEdit](https://stackedit.io/).

 [原文链接](https://www.journaldev.com/1162/java-multithreading-concurrency-interview-questions-answers#process-vs-thread)

### 1. 进程（Process）和线程（Thread）的 区别

进程是一个独立的执行环境，可以看作是程序或者应用程序，而 Thread 是进程中的单个执行任务。线程可以称为轻量级的进程。线程需要较少的资源来创建并存在于进程中，线程共享进程资源。

### 2. 多线程编程的好处
在多线程编程中，多个线程同时执行，从而提高性能，因为 CPU 没有空闲以防某些线程正在等待获取资源。多个线程共享堆内存（heap memory）。

### 3. 用户线程(User Thread)和守护线程(Daemon Thread)有什么区别
当我们在 Java 中创建一个线程时，它被称为用户线程。守护线程在后台运行，不会阻止 JVM 终止。当所有的用户线程都终止运行时，JVM 会关闭程序并退出。从守护线程创建的子线程也是一个守护线程。

### 4. 在 Java 中我们如何创建一个 Thread
在 Java 中，有两种方式创建线程。第一种是实现 Runnable 接口然后创建一个 Thread 将 Runnable 的实现类作为参数传递进去。第二种是直接继承 Thread 。[Createing threads in java](https://www.journaldev.com/1016/java-thread-example)

### 5. Thread 的声明周期中的不同状态
当我们在 Java 程序中创建一个 Thread 时候，Thread 的状态是 New，然后我们调用 start 方法开启一个线程之后，状态会变成 Runnable，线程调度程序负责将 CPU 分配给 Runnable 线程池中的线程，并将其状态更改为 Running， 线程的其他状态为 Waiting，Blocked 和 Dead。[life cycle of thread](https://www.journaldev.com/1044/thread-life-cycle-in-java-thread-states-in-java)

### 6. 我们可以调用 Thread 类的 run 方法吗？
是的，我们可以调用Thread 类的 run 方法，但他会想普通方法一样运行。要在 Thread 中执行它，我们需要视同 `Thread.start()` 方法启动它。

### 7. 如何在特定时间暂停线程的执行
我们可以使用 Thread 类的 sleep() 方法暂停执行 Thread 一段时间。请注意，这不会停止线程，一旦线程从睡眠(sleep)中唤醒(awake)，线程的状态会基于线程调度变为 Runnable，然后将会被执行。

### 8. 你对线程的

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTkzMjQ4MDk2MywtMTEzOTEwMDc0OCwzMD
I5NTM4MzZdfQ==
-->