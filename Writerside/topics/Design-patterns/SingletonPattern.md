# 单例模式

## java中单例模式
Java中单例模式是一种常见的设计模式，单例模式的写法有好几种，这里主要介绍三种：懒汉式单例、饿汉式单例、登记式单例。

单例模式有以下特点
1. 单例类只能有一个实例。
2. 单例类必须自己创建自己的唯一实例。
3. 单例类必须给所有其他对象提供这一实例。

单例模式确保某个类只有一个实例，而且自行实例化并向整个系统提供这个实例。在计算机系统中，线程池、缓存、日志对象、对话框、打印机、显卡的驱动程序对象常被设计成单例。这些应用都或多或少具有资源管理器的功能。每台计算机可以有若干个打印机，但只能有一个Printer Spooler，以避免两个打印作业同时输出到打印机中。每台计算机可以有若干通信端口，系统应当集中管理这些通信端口，以避免一个通信端口同时被两个请求同时调用。总之，选择单例模式就是为了避免不一致状态，避免政出多头。

```java
// 懒汉式单例类.在第一次调用的时候实例化自己   
public class Singleton {  
    private Singleton() {}  
    private static Singleton single=null;  
    // 静态工厂方法   
    public static Singleton getInstance() {  
        if (single == null) {    
            single = new Singleton();  
        }
    return single;
}

// 饿汉式单例类.在类初始化时，已经自行实例化   
public class Singleton1 {  
    private Singleton1() {}  
    private static final Singleton1 single = new Singleton1();  
    // 静态工厂方法   
    public static Singleton1 getInstance() {  
        return single;  
    }  
}  
```

## 单例模式的三种写法
```java

/**
 * 饿汉式－我不管你用不用我都提前最备好 
 * 浪费内存 
 * 
 */
public class Singleton {  

    //传参数没法做  
    private static Singleton instance = new Singleton();  

    private Singleton() {

    }  

    public static Singleton getInstace() {
        return instance;
    }  
}  
/**
 * 通常写法 
 *  
 * 懒汉式－－－当你使用的时候我就创建单例对象 
 *  
 * 一般的客户端开发经常使用的解决方案  
 * 
 */
public class Singleton {  

    private static Singleton instance;  

    private Singleton() {

    }  

    //在服务器，或者多线程访问  
    //服务器并发  
    public static synchronized Singleton getInstace() {
        //假设：3个线程－－－并发线程  
        //场景：同时调用getInstace方法  
        //线程一：  
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }  

}  
/**
 * 双重检查（折中考虑，既不浪费内存，性能相对来说也比较高）  
 */
public class Singleton {  

    //线程并发时候，当我们的变量被被修改之后，其他的并发线程及时收到通知，其他的线程就可以访问数据  
    //volatile--会屏蔽虚拟机优化过程  
    private volatile static Singleton instance;  

    //iOS开发经常是这么搞的（基本上写单例都加索）  
    private Singleton() {
    }  

    public static Singleton getInstace() {
        if(instance == null){
            synchronized (Singleton.class) {
                if(instance == null){
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }  

}  

```
## 饿汉式和懒汉式区别

**从名字上来说饿汉和懒汉**    
饿汉就是类一旦加载，就把单例初始化完成，保证getInstance的时候，单例是已经存在的了，而懒汉比较懒，只有当调用getInstance的时候，才回去初始化这个单例。

**另外从以下两点再区分一下这两种方式**   
1. 线程安全
    - 饿汉式天生就是线程安全的，可以直接用于多线程而不会出现问题，
    - 懒汉式本身是非线程安全的，为了实现线程安全有几种写法，分别是上面的1、2、3，这三种实现在资源加载和性能方面有些区别。

2. 资源加载和性能
    - 饿汉式在类创建的同时就实例化一个静态对象出来，不管之后会不会使用这个单例，都会占据一定的内存，但是相应的，在第一次调用时速度也会更快，因为其资源已经初始化完成，
    - 而懒汉式顾名思义，会延迟加载，在第一次使用该单例的时候才会实例化对象出来，第一次调用时要做初始化，如果要做的工作比较多，性能上会有些延迟，之后就和饿汉式一样了。
