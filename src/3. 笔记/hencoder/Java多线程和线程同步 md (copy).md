**HenCoder Plus** 第 **17** 课 讲义 线程间通信的本质和原理理，以及 **Android** 中 

的多线程 线程间交互 

⼀一个线程启动别的线程:new Thread().start()、Executor.execute() 等 ⼀一个线程终结另⼀一个线程 

Thread.stop() Thread.interrupt():温和式终结:不不⽴立即、不不强制 

interrupted() 和 isInterrupted():检查(和重置)中断状态 InterruptedException:如果在线程「等待」时中断，或者在中断状态「等待」，直 接结束等待过程(因为等待过程什什么也不不会做，⽽而 interrupt() 的⽬目的是让线程做完收 尾⼯工作后尽快终结，所以要跳过等待过程) 

Object.wait() 和 Object.notify() / notifyAll() 

在未达到⽬目标时 wait()
 ⽤用 while 循环检查
 设置完成后 notifyAll()
 wait() 和 notify() / notifyAll() 都需要放在同步代码块⾥里里 

Thread.join():让另⼀一个线程插在⾃自⼰己前⾯面 Thread.yield():暂时让出⾃自⼰己的时间⽚片给同优先级的线程 

**Android** 的 **Handler** 机制 

```
  本质:在某个指定的运⾏行行中的线程上执⾏行行代码
  思路路:在接受任务的线程上执⾏行行循环判断
  基本实现:
```

Thread ⾥里里 while 循环检查
 加上 Looper(优势在于⾃自定义 Thread 的代码可以少写很多): 再加上 Handler(优势在于功能分拆，⽽而且可以有多个 Handler) 

Java 的 Handler 机制: 

HandlerThread:具体的线程 Looper:负责循环、条件判断和任务执⾏行行 Handler:负责任务的定制和线程间传递 

AsyncTask:
 AsyncTask 的内存泄露露 

众所周知的原因:AsyncTask 持有外部 Activity 的引⽤用
 扔物线学堂 / rengwuxian.com 1 

没提到的原因:执⾏行行中的线程不不会被系统回收
 Java 回收策略略:没有被 GC Root 直接或间接持有引⽤用的对象，会被回收 GC Root: 

\1. 运⾏行行中的线程
 \2. 静态对象
 \3. 来⾃自 native code 中的引⽤用 

所以: 

AsyncTask 的内存泄露露，其他类型的线程⽅方案(Thread、Executor、 HandlerThread)⼀一样都有，所以不不要忽略略它们，或者认为 AsyncTask ⽐比别的⽅方 案更更危险。并没有。
 就算是使⽤用 AsyncTask，只要任务的时间不不⻓长(例例如 10 秒之内)，那就完全没 必要做防⽌止内存泄露露的处理理。 

**Service** 和 **IntentService** Service:后台任务的活动空间。适⽤用场景:⾳音乐播放器器等。 

IntentService:执⾏行行单个任务后⾃自动关闭的 Service **Executor**、**AsyncTask**、**HandlerThead**、**IntentService** 的选 

择 

原则:哪个简单⽤用哪个 

能⽤用 Executor 就⽤用 Executor
 需要⽤用到「后台线程推送任务到 UI 线程」时，再考虑 AsyncTask 或者 Handler
 HandlerThread 的使⽤用场景:原本它设计的使⽤用场景是「在已经运⾏行行的指定线程上执⾏行行代 码」，但现实开发中，除了了主线程之外，⼏几乎没有这种需求，因为 HandlerThread 和 Executor 相⽐比在实际应⽤用中并没什什么优势，反⽽而⽤用起来会麻烦⼀一点。不不过，这⼆二者喜欢⽤用谁就⽤用谁吧。 IntentService:⾸首先，它是⼀一个 Service;另外，它在处理理线程本身，没有⽐比 Executor 有任何优 势 

关于 **Executor** 和 **HandlerThread** 的关闭 如果在界⾯面组件⾥里里创建 Executor 或者 HandlerThread，记得要在关闭的时候(例例如 

Activity.onDestroy() )关闭 Executor 和 HandlerThread。 

@Override 

```
protected void onDestroy() {
    super.onDestroy();
    executor.shutdown();
}

@Override 
protected void onDestroy() {
 super.onDestroy();
 handlerThread.quit(); // 这个其实就是停⽌止 Looper 的循环 

} 
```



