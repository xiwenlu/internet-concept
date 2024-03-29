# java锁

1. 可见性就是在一个线程的工作内存中修改了该变量的值，该变量的值立即能回显到主内存中，从而保证所有的线程看到这个变量的值是一致的

## 什么是volatile？

volatile可以看做是一个轻量级的synchronized，它可以在多线程并发的情况下保证变量的“可见性”，它的开销比synchronized小、使用成本更低。

虽说这个volatile关键字可以解决多线程环境下的同步问题，不过这也是相对的，因为它不具有操作的原子性，也就是它不适合在对该变量的写操作依赖于变量本身自己。

举个最简单的例子：在进行count++计数操作时，实际是count=count+1，count最终的值依赖于它本身的值。所以使用volatile修饰的变量在进行这么一系列的操作的时候，就有并发的问题

## wait和sleep的区别

都是释放cpu执行权。

1. wait时会释放锁资源但sleep不会释放锁资源
2. wait通常和notify以及notifyAll结合使用，需要notify或者notifyAll对其进行唤醒，sleep通常在指定的时间内自动唤醒。

## synchronized和lock的区别

synchronized锁的实现原理，简单概括就是在JVM层运用了object的monitor来实现同步。而Lock是Java大神 Doug Lea 开发的util.concurrent中的一个锁工具，它实现了Synchronized的所有功能。下面从使用和性能这两个部分来具体分析两者的不同。

### synchronized和lock的用法区别 {id="synchronized-lock_1"}

synchronized：在需要同步的对象中加入此关键字，可以加在方法上、代码块中，可以添加加锁的对象，不添加则默认是该类或该class对象。

lock：需要显示的加在需要同步的开始和结束处。一般使用ReentrantLock类为锁。使用lock方法加锁，unlock方法解锁，unlock方法一般要放在finally块中保证一定执行。

区别于Synchronized，在使用wait和notify方法中，Synchronized是无条件的阻塞和唤醒。而Lock可以使用内置的Condition类来区别唤醒不同的对象。

### synchronized和lock的性能区别 {id="synchronized-lock_2"}

Synchronized是JVM层面执行的指令，而Lock是Java写的控制线程的代码。

在Java1.6之前，Synchronized一直被称为重量级锁，因为它需要调用操作系统的线程指令，导致有可能加锁消耗的资源比加锁以外的操作还多。相比之下，Lock的性能很可能更高一些，它不需要调用系统的阻塞操作。不过，在Java1.6之后，对Synchronized进行了各种优化，如自适应自旋（轻量级锁中等使用）、锁粗化（合并锁）、偏向锁（单线程使用）、锁消除等技术，致使Java1.6上，Synchronized的性能并不比Lock差。

此外，Synchronized采用了悲观锁的机制。顾名思义，它假设每次执行同步的时候，别的线程总会也去执行，所以每次都会上锁。线程获得的是独占锁，意味着其他线程只能依靠阻塞来等待占有线程来释放锁。而CPU阻塞和唤醒线程会引起上下文切换，大量线程竞争锁的时候，导致低效。

而Lock使用了乐观锁的方式。顾名思义，就是每次不加锁而是假设没有冲突而去完成操作。它是基于CAS操作的（Compare And Swap），通过竞争Lock内置队列同步器（AQS）中的状态位state，填充入自己的线程来获得锁。

现代CPU提供了指令，可以自动更新共享数据，而且能够检测其他线程的干扰，而compareAndSet()就用这些代替了锁定，这是一个自旋而非阻塞的算法，不会被挂起或者影响其他线程。

## synchronized和lock的区别（2） {id="synchronized-lock_3"}

1. lock并发包下的一个工具类是一个接口，能完成synchronize锁的全部功能，而synchronized是Java中的关键字，synchronized是内置的语言实现。synchronize主要用在线程较少、线程资源竞争不激烈的情况下，而lock锁则用在线程较多、线程资源竞争激烈的情况。synchronize的互斥锁在使用完CPU资源时会自动释放锁，lock锁必须手动调用unlock()去释放锁，unlock()必须在finally块中执行，否则有可能会造成死锁。
2. synchronized在发生异常时，会自动释放线程占有的锁，因此不会导致死锁现象发生；而Lock在发生异常时，如果没有主动通过unLock去释放锁，则很可能造成死锁现象，因此使用Lock时需要在finally块中释放锁。
3. Lock可以让等待锁的线程响应中断，而synchronized却不行，使用synchronized时，等待的线程会一直等待下去，不能够响应中断。
4. 通过Lock可以知道有没有成功获取锁，而synchronized却无法办到。
5. Lock可以提高多个线程进行读操作的效率。
