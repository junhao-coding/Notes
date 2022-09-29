# 并发

## 1.什么是线程

> 一个程序执行多个任务，通常每一个任务称为一个线程，可以同时运行一个以上的线程的程序称为多线程程序。
>
> 进程和线程的区别：每个进程都拥有自己的一整套变量，而线程则共享资源。

### 1.1线程的创建

1）将任务代码移动到实现了**Runnable**接口的类的run方法中，由于**Runnable**是一个函数式接口，可以使用***lambda***表达式

~~~java
Runnable r = () -> {task code};
~~~

2）由**Runnable**创建一个**Thread**对象

~~~java
Thread t = new Thread(r);
~~~

3）启动线程

~~~java
t.start()
~~~

**注意：**==不要调用Thread类或Runnable对象的run方法，直接调用run方法，只会执行一个线程的同一个任务，而不会启动新线程==

### 1.2 中断线程

当线程的run方法由return语句返回时，或者出现了在方法中没有捕获的异常时，线程将终止，没有强制终止线程的办法，但是可以用**interrupt**方法来终止线程

~~~java
//将线程中断状态设置为true
Thread.currentThread().interrupt();
//检查当前线程是否被中断
Thread.currentThread().isInterrupted();
~~~

但是当线程被阻塞（调用sleep或wait）时，无法检测中断状态，这将产生**InterruptedException** 异常。应该这样进行处理

~~~java
//方式一：在catch子句中设置中断状态
void myTask()
{
    ···
    try{
        ···;
        sleep(delay);
    }catch(InterruptedException e){
        Thread.currentThread.interrupt();
    }
    ···
}
//方式二：抛出异常而不是用try语句捕获，最终由调用者来捕获
void myTask() throws InterruptedException{
    ···
    sleep(delay);
    ···
}
~~~

### 1.3 线程状态

#### 1.3.1 New(新创建)

当使用New操作符创建一个新线程时，线程还没有开始运行，通常在线程运行之前还有一些基础工作要做。

#### 1.3.2 Runnable(可运行)

当调用start方法时，线程处于可运行状态，线程什么时候被运行由操作系统的调度策略或者线程优先级来决定。

#### 1.3.3 Blocked(被阻塞)，Waiting(等待)和Timed-Waiting(计时等待)

当线程被阻塞或等待时，暂时不活动，他不运行任何代码并且消耗最少的资源，以下情况线程被阻塞或等待

- 当一个线程试图获取一个内部的对象锁，而该锁被其它线程所持有，线程进入阻塞态，当其它线程释放该锁时，

  并且线程调度器允许本线程持有它时，线程变为非阻塞态

- 当线程等待另一个线程通知调度器一个条件时，它自己进入等待状态。

- 计时等待，比如sleep，wait等方法，给定一个超时参数，线程进入计时等待状态，并一直保持知道超时。

#### 1.3.4 Terminate(被终止)

* run方法正常退出
* 一个没有捕获的异常终止了run方法

### 1.4 线程属性

#### 1.4.1 线程优先级

在Java中，每一个线程都有一个优先级，默认情况下，一个线程继承它父线程的优先级。可以使用setPriority方法提高或者降低线程的优先级。

~~~Java
thread.setPriority(MAX_PRIORITY);//优先级设为最高（10）
thread.setPriority(MIN_PRIORITY);//优先级设为最低（1）
//默认优先级：NORM_PRIORITY(5)
~~~

线程的优先级依赖于系统，Windows有七个优先级，Java的优先级将映射到相同的操作系统优先级，而在为Linux提供的虚拟机中，优先级被忽略。

==另外，优先级高的线程不一定比优先级低的线程先执行，优先级更高只是更大概率能够抢占CPU的时间片。==

#### 1.4.2 守护线程

深入理解什么是守护进程可以看这一篇博客：[谈谈什么是守护进程及作用](https://www.cnblogs.com/quanxiaoha/p/10731361.html)

通过调用setDaemon方法可以将线程设置为一个守护线程。==如果 JVM 中没有一个正在运行的非守护线程，这个时候，JVM 会退出。==

~~~JAVA
t.setDaemon(true);//这一方法必须在线程启动之前调用
~~~

==守护线程拥有自动结束自己生命周期的特性，而非守护线程不具备这个特点。==

#### 1.4.3 未捕获异常处理器

详细的未捕获异常处理使用可以看这一片博客：[Java线程未捕获异常](https://blog.csdn.net/hongxingxiaonan/article/details/50527169)

线程的run方法不能抛出任何异常，但是不被检测的异常将导致线程终止，可以使用未捕获异常处理器来处理这些异常。

该处理器必须实现**Thread.UncaughtExceptionHandler**接口，这个接口只一个一个方法。

~~~java
void uncaughtException(Thread t, Throwable e);
//t: 由未捕获异常而终止的线程
//e: 未捕获的异常对象
~~~

可以用**setUncaughtExceptionHandler**方法为任何线程安装一个处理器，也可以用Thread中的静态方法**setDefaultUncaughtExceptionHandler**

==为所有线程按安装一个默认的处理器==，如果不显示的安装处理器，处理器为空，此时的处理器是该线程的**ThreadGroup**对象。

> ThreadGroup对象是一个用来统一管理线程集合，默认情况下，所有的线程属于相同的线程组，在没有创建一个新的线程组时，会继承main线程组。

ThreadGroup类实现了**Thread.UncaughtExceptionHandler**接口，它的方法会做如下操作

1）如果由父线程组就低矮用父线程组的**uncaughtException**方法

2）否则，如果调用**Thread.getDefaultUncaughtExceptionHandler**返回一个非空的默认处理器，就调用它。

3）否则，如果**Throwable** 是**ThreadDeath**的一个实例，就什么都不会做

4）否则，线程的名字和***Throwable**的栈轨迹被输出到**System.err**上。

## 2. 同步

### 2.1 竞争条件

在多线程应用程序中，两个或两个以上的线程需要共享对同一数据的存取，当每一个线程都调用了修改对象状态的方法，

将会发生预期之外的错误，这中情况称为**竞争条件**。一个例子如下：

~~~java
public class BankTest {
    public static final int NACCOUNT = 100;//银行账户数量
    public static final double INITIAL_BALANCE = 5000D;//出现金额
    public static final double MAX_AMOUNT = 1000D;
    public static int DElAY = 10;//休眠延迟
    public static int STEP = 50;//在一个线程中执行STEP次交易

    public static void main(String[] args) {
        Bank bank = new Bank(NACCOUNT, INITIAL_BALANCE);
        Runnable r = () -> {
            for (int j = 0; j < STEP; j++) {
                try {
                    int fromAccount = (int) (bank.size() * Math.random());//交易双方
                    int toAccount = (int) (bank.size() * Math.random());
                    double amount = MAX_AMOUNT * Math.random();
                    bank.transfer(fromAccount, toAccount, amount);//进行双方的交易
                    Thread.sleep((int) (DElAY * Math.random()));
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        };
        for (int i = 0; i < 5; i++) {//启动五个线程执行run方法
            Thread t = new Thread(r);
            t.start();
        }
    }
}
class Bank{
    private final double[] accounts;

    public Bank(int n, double balance) {
        accounts = new double[n];
        Arrays.fill(accounts, balance);
    }
    public int size(){
        return accounts.length;
    }
    public double calTotalMoney(){
        double result = 0;
        for (double money : accounts) {
            result += money;
        }
        return result;
    }
    public void transfer(int from, int to, double money){
        if(accounts[from] < money || from == to)
            return;
        System.out.print(Thread.currentThread());
        accounts[from] -= money;
        System.out.printf("%10.2f from %d to %d", money, from, to);
        accounts[to] += money;
        System.out.printf("Total Balance: %10.2f\n", calTotalMoney());
    }
}
~~~

==按理来说银行的总金额应该不会发生改变，但是在程序执行过程中出现了总金额小于500000的情况。==

分析：假定两个线程同时执行指令 ***accounts[to] += money***，该指令将被处理如下

1）将accounts[to]加载寄存器中。

2）增加amount。

3）将结果写回到accounts[to]。

当一个线程执行步骤1和2，然后被剥夺了执行权，此时第二个线程被唤醒并修改了accounts数组的同一项，这时第一个

线程被唤醒并执行了步骤三，==这个动作擦去了第二个线程所做的更新，于是总金额就不再正确。==

### 2.2 同步代码块



### 2.3 同步方法

## 3.线程池

在多线程应用程序的开发中，如果用户每发起一个请求就创建一个线程，而线程创建的开销是很大的，这样就会影响系统的性能。

于是出现了线程池技术，==可以对线程进行复用==。简单来说就是一个线程完成任务后并不是销毁而是去接待下一个用户的请求。

![线程池](C:\课外学习\博客笔记\Java\JAVA 高级\img\线程池.jpg)

### 3.1 线程池API

线程池的接口是**ExecutorService**，有两种方式可以创建线程池对象。

* 方式一：使用**ExcutorService**的实现类**ThreadPoolExecutor**来创建线程池对象。
* 方式二：使用**Executors**，即线程的工具类来创建不同特点的线程池对象。

建议使用方式一来创建线程池来规避风险。

#### 3.1.1ThreadPoolExecutor

完整的构造器参数如下：

~~~java
public ThreadPoolExecutor(int corePoolSize,//核心线程数量
                          int maximumPoolSize,//线程池可接受的最大线程数量
                          long keepAliveTime,//临时线程在终止前等待新任务的最长时间
                          TimeUnit unit,//等待时间的时间单位
                          BlockingQueue<Runnable> workQueue,//任务阻塞队列
                          ThreadFactory threadFactory,//指定线程创建的工厂
                          RejectedExecutionHandler handler)//指定线程忙时的处理器
~~~


注意两个问题：

* 临时线程的创建时间：只有当核心线程在忙且任务队列已满时，才会创建临时线程
* 线程池能支持的最大任务量：任务队列的大小加上最大线程数量

### 3.2 线程池处理任务

~~~java
void execute(Runnable command);//执行Runnable任务
Future<T> submit(Callable<T> task);//执行Callable任务，返回Future接口对象。
void shutDown();//等任务执行完毕后关闭线程池
List<Runnable> shutDownNow();//立即关闭线程池并返回线程列表
~~~

