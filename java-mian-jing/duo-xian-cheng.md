# 多线程

## **并行和并发有什么区别？**

* 并行是指两个或者多个事件在同一时刻发生；而并发是指两个或多个事件在同一时间间隔发生。
* 并行是在不同实体上的多个事件，并发是在同一实体上的多个事件。
* 在一台处理器上“同时”处理多个任务，在多台处理器上同时处理多个任务。如hadoop分布式集群。

所以并发编程的目标是充分的利用处理器的每一个核，以达到最高的处理性能。

## **线程和进程的区别？**

简而言之，进程是程序运行和资源分配的基本单位，一个程序至少有一个进程，一个进程至少有一个线程。进程在执行过程中拥有独立的内存单元，而多个线程共享内存资源，减少切换次数，从而效率更高。线程是进程的一个实体，是cpu调度和分派的基本单位，是比程序更小的能独立运行的基本单位。同一进程中的多个线程之间可以并发执行。

## **守护线程是什么？**

守护线程（即daemon thread），是个服务线程，准确地来说就是服务其他的线程。

## **创建线程有哪几种方式？**

继承Thread类创建线程类

* 定义Thread类的子类，并重写该类的run方法，该run方法的方法体就代表了线程要完成的任务。因此把run()方法称为执行体。
* 创建Thread子类的实例，即创建了线程对象。
* 调用线程对象的start()方法来启动该线程。

通过Runnable接口创建线程类

* 定义runnable接口的实现类，并重写该接口的run()方法，该run()方法的方法体同样是该线程的线程执行体。
* 创建 Runnable实现类的实例，并依此实例作为Thread的target来创建Thread对象，该Thread对象才是真正的线程对象。
* 调用线程对象的start()方法来启动该线程。

通过Callable和Future创建线程

* 创建Callable接口的实现类，并实现call()方法，该call()方法将作为线程执行体，并且有返回值。
* 创建Callable实现类的实例，使用FutureTask类来包装Callable对象，该FutureTask对象封装了该Callable对象的call()方法的返回值。
* 使用FutureTask对象作为Thread对象的target创建并启动新线程。
* 调用FutureTask对象的get()方法来获得子线程执行结束后的返回值。

## **说一下 runnable 和 callable 有什么区别？**

* Runnable接口中的run()方法的返回值是void，它做的事情只是纯粹地去执行run()方法中的代码而已；
* Callable接口中的call()方法是有返回值的，是一个泛型，和Future、FutureTask配合可以用来获取异步执行的结果。

## **线程有哪些状态？**

代码上可以看到有6种状态：new、runnable、blocked、waiting、timed\_waiting、terminated

```
public enum State {    
    /**     * Thread state for a thread which has not yet started.     */    
    NEW,    
    /**     * Thread state for a runnable thread.  A thread in the runnable     * state is executing in the Java virtual machine but it may     * be waiting for other resources from the operating system     * such as processor.     */    
    RUNNABLE,    
    /**     * Thread state for a thread blocked waiting for a monitor lock.     * A thread in the blocked state is waiting for a monitor lock     * to enter a synchronized block/method or     * reenter a synchronized block/method after calling     * {@link Object#wait() Object.wait}.     */    
    BLOCKED,    
    /**     * Thread state for a waiting thread.     * A thread is in the waiting state due to calling one of the     * following methods:     * <ul>     *   <li>{@link Object#wait() Object.wait} with no timeout</li>     *   <li>{@link #join() Thread.join} with no timeout</li>     *   <li>{@link LockSupport#park() LockSupport.park}</li>     * </ul>     *     * <p>A thread in the waiting state is waiting for another thread to     * perform a particular action.     *     * For example, a thread that has called <tt>Object.wait()</tt>     * on an object is waiting for another thread to call     * <tt>Object.notify()</tt> or <tt>Object.notifyAll()</tt> on     * that object. A thread that has called <tt>Thread.join()</tt>     * is waiting for a specified thread to terminate.     */    
    WAITING,    
    /**     * Thread state for a waiting thread with a specified waiting time.     * A thread is in the timed waiting state due to calling one of     * the following methods with a specified positive waiting time:     * <ul>     *   <li>{@link #sleep Thread.sleep}</li>     *   <li>{@link Object#wait(long) Object.wait} with timeout</li>     *   <li>{@link #join(long) Thread.join} with timeout</li>     *   <li>{@link LockSupport#parkNanos LockSupport.parkNanos}</li>     *   <li>{@link LockSupport#parkUntil LockSupport.parkUntil}</li>     * </ul>     */    
    TIMED_WAITING,    
    /**     * Thread state for a terminated thread.     * The thread has completed execution.     */    
    TERMINATED;
}
```

## **sleep() 和 wait() 有什么区别？**

sleep()：方法是线程类（Thread）的静态方法，让调用线程进入睡眠状态，让出执行机会给其他线程，等到休眠时间结束后，线程进入就绪状态和其他线程一起竞争cpu的执行时间。因为sleep() 是static静态的方法，他不能改变对象的机锁，<mark style="color:orange;">当一个synchronized块中调用了sleep() 方法，线程虽然进入休眠，但是对象的机锁没有被释放，其他线程依然无法访问这个对象</mark>。

wait()：wait()是Object类的方法，当一个线程执行到wait方法时，它就进入到一个和该对象相关的等待池，同时释放对象的机锁，使得其他线程能够访问，可以通过notify，notifyAll方法来唤醒等待的线程。

## **notify()和 notifyAll()有什么区别？**

* 如果线程调用了对象的 wait()方法，那么线程便会处于该对象的等待池中，等待池中的线程不会去竞争该对象的锁。
* 当有线程调用了对象的 notifyAll()方法（唤醒所有 wait 线程）或 notify()方法（只随机唤醒一个 wait 线程），被唤醒的的线程便会进入该对象的锁池中，锁池中的线程会去竞争该对象锁。也就是说，调用了notify后只要一个线程会由等待池进入锁池，而notifyAll会将该对象等待池内的所有线程移动到锁池中，等待锁竞争。
* 优先级高的线程竞争到对象锁的概率大，假若某线程没有竞争到该对象锁，它还会留在锁池中，唯有线程再次调用 wait()方法，它才会重新回到等待池中。而竞争到对象锁的线程则继续往下执行，直到执行完了 synchronized 代码块，它会释放掉该对象锁，这时锁池中的线程会继续竞争该对象锁。

## **线程的 run()和 start()有什么区别？**

每个线程都是通过某个特定Thread对象所对应的方法run()来完成其操作的，方法run()称为线程体。通过调用Thread类的start()方法来启动一个线程。

start()方法来启动一个线程，真正实现了多线程运行。这时无需等待run方法体代码执行完毕，可以直接继续执行下面的代码； 这时此线程是处于就绪状态， 并没有运行。 然后通过此Thread类调用方法run()来完成其运行状态， 这里方法run()称为线程体，它包含了要执行的这个线程的内容， Run方法运行结束， 此线程终止。然后CPU再调度其它线程。

run()方法是在本线程里的，只是线程里的一个函数,而不是多线程的。 如果直接调用run(),其实就相当于是调用了一个普通函数而已，直接待用run()方法必须等待run()方法执行完毕才能执行下面的代码，所以执行路径还是只有一条，根本就没有线程的特征，所以在多线程执行时要使用start()方法而不是run()方法。\


## **创建线程池有哪几种方式？**

newFixedThreadPool(int nThreads)

创建一个固定长度的线程池，每当提交一个任务就创建一个线程，直到达到线程池的最大数量，这时线程规模将不再变化，当线程发生未预期的错误而结束时，线程池会补充一个新的线程。

newCachedThreadPool()

创建一个可缓存的线程池，如果线程池的规模超过了处理需求，将自动回收空闲线程，而当需求增加时，则可以自动添加新线程，线程池的规模不存在任何限制。

newSingleThreadExecutor()

这是一个单线程的Executor，它创建单个工作线程来执行任务，如果这个线程异常结束，会创建一个新的来替代它；它的特点是能确保依照任务在队列中的顺序来串行执行。

newScheduledThreadPool(int corePoolSize)

创建了一个固定长度的线程池，而且以延迟或定时的方式来执行任务，类似于Timer。

## **线程池都有哪些状态？**

线程池有5种状态：Running、ShutDown、Stop、Tidying、Terminated。

线程池各个状态切换框架图：

![线程池状态图](../.gitbook/assets/image.png)

## **线程池中 submit()和 execute()方法有什么区别？**

* 接收的参数不一样
* submit有返回值，而execute没有
* submit方便Exception处理

## **在 java 程序中怎么保证多线程的运行安全？**

线程安全在三个方面体现：

* 原子性：提供互斥访问，同一时刻只能有一个线程对数据进行操作，（atomic,synchronized）；
* 可见性：一个线程对主内存的修改可以及时地被其他线程看到，（synchronized,volatile）；
* 有序性：一个线程观察其他线程中的指令执行顺序，由于指令重排序，该观察结果一般杂乱无序，（happens-before原则）。

## **多线程锁的升级原理是什么？**

在Java中，锁共有4种状态，级别从低到高依次为：无状态锁，偏向锁，轻量级锁和重量级锁状态，这几个状态会随着竞争情况逐渐升级。锁可以升级但不能降级。

锁升级的图示过程：

![锁升级](<../.gitbook/assets/image (2).png>)

## **怎么防止死锁？**

死锁的四个必要条件：

* 互斥条件：进程对所分配到的资源不允许其他进程进行访问，若其他进程访问该资源，只能等待，直至占有该资源的进程使用完成后释放该资源
* 请求和保持条件：进程获得一定的资源之后，又对其他资源发出请求，但是该资源可能被其他进程占有，此事请求阻塞，但又对自己获得的资源保持不放
* 不可剥夺条件：是指进程已获得的资源，在未完成使用之前，不可被剥夺，只能在使用完后自己释放
* 环路等待条件：是指进程发生死锁后，若干进程之间形成一种头尾相接的循环等待资源关系

这四个条件是死锁的必要条件，只要系统发生死锁，这些条件必然成立，而只要上述条件之 一不满足，就不会发生死锁。

理解了死锁的原因，尤其是产生死锁的四个必要条件，就可以最大可能地避免、预防和 解除死锁。

所以，在系统设计、进程调度等方面注意如何不让这四个必要条件成立，如何确 定资源的合理分配算法，避免进程永久占据系统资源。

此外，也要防止进程在处于等待状态的情况下占用资源。因此，对资源的分配要给予合理的规划。

## **ThreadLocal 是什么？有哪些使用场景？**

线程局部变量是局限于线程内部的变量，属于线程自身所有，不在多个线程间共享。Java提供ThreadLocal类来支持线程局部变量，是一种实现线程安全的方式。但是在管理环境下（如 web 服务器）使用线程局部变量的时候要特别小心，在这种情况下，工作线程的生命周期比任何应用变量的生命周期都要长。任何线程局部变量一旦在工作完成后没有释放，Java 应用就存在内存泄露的风险。

## **说一下 synchronized 底层实现原理？**

synchronized可以保证方法或者代码块在运行时，同一时刻只有一个方法可以进入到临界区，同时它还可以保证共享变量的内存可见性。

Java中每一个对象都可以作为锁，这是synchronized实现同步的基础：

* 普通同步方法，锁是当前实例对象
* 静态同步方法，锁是当前类的class对象
* 同步方法块，锁是括号里面的对象

## **synchronized 和 Lock 有什么区别？**

* 首先synchronized是java内置关键字，在jvm层面，Lock是个java类；
* synchronized无法判断是否获取锁的状态，Lock可以判断是否获取到锁；
* synchronized会自动释放锁(a 线程执行完同步代码会释放锁 ；b 线程执行过程中发生异常会释放锁)，Lock需在finally中手工释放锁（unlock()方法释放锁），否则容易造成线程死锁；
* 用synchronized关键字的两个线程1和线程2，如果当前线程1获得锁，线程2线程等待。如果线程1阻塞，线程2则会一直等待下去，而Lock锁就不一定会等待下去，如果尝试获取不到锁，线程可以不用一直等待就结束了；
* synchronized的锁可重入、不可中断、非公平，而Lock锁可重入、可判断、可公平（两者皆可）；
* Lock锁适合大量同步的代码的同步问题，synchronized锁适合代码少量的同步问题。

## **synchronized 和 ReentrantLock 区别是什么？**

synchronized是和if、else、for、while一样的关键字，ReentrantLock是类，这是二者的本质区别。既然ReentrantLock是类，那么它就提供了比synchronized更多更灵活的特性，可以被继承、可以有方法、可以有各种各样的类变量，ReentrantLock比synchronized的扩展性体现在几点上：&#x20;

* ReentrantLock可以对获取锁的等待时间进行设置，这样就避免了死锁&#x20;
* ReentrantLock可以获取各种锁的信息
* ReentrantLock可以灵活地实现多路通知&#x20;

另外，二者的锁机制其实也是不一样的:ReentrantLock底层调用的是Unsafe的park方法加锁，synchronized操作的应该是对象头中mark word。

## **说一下 atomic 的原理？**

Atomic包中的类基本的特性就是在多线程环境下，当有多个线程同时对单个（包括基本类型及引用类型）变量进行操作时，具有排他性，即当多个线程同时对该变量的值进行更新时，仅有一个线程能成功，而未成功的线程可以向自旋锁一样，继续尝试，一直等到执行成功。

Atomic系列的类中的核心方法都会调用unsafe类中的几个本地方法。我们需要先知道一个东西就是Unsafe类，全名为：sun.misc.Unsafe，这个类包含了大量的对C代码的操作，包括很多直接内存分配以及原子操作的调用，而它之所以标记为非安全的，是告诉你这个里面大量的方法调用都会存在安全隐患，需要小心使用，否则会导致严重的后果，例如在通过unsafe分配内存的时候，如果自己指定某些区域可能会导致一些类似C++一样的指针越界到其他进程的问题。



## &#x20;
