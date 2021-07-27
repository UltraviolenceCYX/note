## 线程池

使用线程池主要有以下三个原因：

1. 创建/销毁线程需要消耗系统资源，线程池可以**复用已创建的线程**。
2. **控制并发的数量**。并发数量过多，可能会导致资源消耗过多，从而造成服务器崩溃。（主要原因）
3. **可以对线程做统一管理**。

线程池的顶层接口是Executor接口，里面只定义了execute（Runnable）方法，Executors是一个常用的工厂类用来创建不同类型的线程池。

ThreadPoolExecutor构造方法的参数含义（线程池中有核心线程（铁饭碗，没活不会被取消）和非核心线程之分）:

```java
ThreadPoolExecutor(int 该线程池中核心线程数最大值,
                          int 核心+非核形线程的最大值,
                          long 非核心线程闲置超时时长,
                          TimeUnit 时间单位,
                         BlockingQueue<Runnable> 阻塞队列，维护着等待执行的Runnable任务象,
                   ----下面为可选参数----
                          ThreadFactory threadFactory,
                          RejectedExecutionHandler拒绝处理策略，线程数量大于最大线程数就会采用拒绝处理策略)
```