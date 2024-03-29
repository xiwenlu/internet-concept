# 线程池

## 什么是线程池？

是指在初始化一个多线程应用程序过程中创建一个线程集合，然后在需要执行新的任务时重用这些线程而不是新建一个线程。线程池中线程的数量通常完全取决于可用内存数量和应用程序的需求。然而，增加可用线程数量是可能的。线程池中的每个线程都有被分配一个任务，一旦任务已经完成了，线程回到池子中并等待下一次分配任务。

## Java的四种线程池使用 {id="java_1"}

### FixedThreadPool定长线程池

1. 通过Executor的newFixedThreadPool静态方法来创建
2. 线程数量固定的线程池
3. 只有核心线程切并且不会被回收
4. 当所有线程都处于活动状态时，新任务都会处于等待状态，直到有线程空闲出来

### CachedThreadPool可缓存线程池

1. 通过Executor的newCachedThreadPool静态静态方法来创建
2. 线程数量不定的线程池
3. 只有非核心线程，最大线程数量为Integer.MAX_VALUE，可视为任意大
4. 有超时机制，时长为60s，即超过60s的空闲线程就会被回收
5. 当线程池中的线程都处于活动状态时，线程池会创建新的线程来处理新任务，否则就会利用空闲的线程来处理新任务。因此任何任务都会被立即执行
6. 该线程池比较适合执行大量耗时较少的任务

### ScheduledThreadPool支持定时及周期性任务执行的定长线程池

1. 通过Executor的newScheduledThreadPool静态方法来创建
2. 核心线程数量是固定的，而非核心线程数不固定的，并且非核心线程有超时机制，只要处于闲置状态就会被立即回收
3. 该线程池主要用于执行定时任务和具有固定周期的重复任务

### SingleThreadPool单线程化的线程池

1. 通过Executor的newSingleThreadPool静态方法来创建
2. 只有一个核心线程，它确保所有的任务都在同一个线程中按顺序执行。因此在这些任务之间不需要处理线程同步的问题

## Java提供的四种线程池的好处 {id="java_2"}

1. 重用存在的线程，减少对象创建、消亡的开销，性能佳。
2. 可有效控制最大并发线程数，提高系统资源的使用率，同时避免过多资源竞争，避免堵塞。
3. 提供定时执行、定期执行、单线程、并发数控制等功能

## Java的四种线程池使用（2）

1. Executors.newScheduledThreadPool(int n)：定长线程池，可以控制线程最大并发数，超出的线程会在队列中等待，适用于长期的任务，性能比较好
2. Executors.newScheduledThreadPool()：定时线程池，支持定时以及周期性任务执行
3. Executors.newSingleThreadExecutor()：创建一个单线线程池。单线程池，它只会用唯一的工作线程来执行任务，保证所有任务按照指定顺序,适用于周期性的场景
4. Executors.newCacheThreadPool()：可缓存线程池，先查看池中有没有以前建立的线程，如果有，就直接使用。如果没有，就建一个新的线程加入池中，缓存型池子通常用于执行一些生存期很短的异步型任务

```java 
package test;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class ThreadPoolExecutorTest {

    public static void main(String[] args) {
        ExecutorService cachedThreadPool = Executors.newCachedThreadPool();
        for (int i = 0; i < 10; i++) {
            final int index = i;
            try {
                Thread.sleep(index * 1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            cachedThreadPool.execute(new Runnable() {
                public void run() {
                    System.out.println(index);
                }
            });
        }
    }
} 
```   

