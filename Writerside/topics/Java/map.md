# map

1. HashTable源码中是使用synchronized来保证线程安全的

## hashMap底层 {id="hashmap_1"}
1. HashMap底层实现是一个线性数组Entry。HashMap其实也是一个线性的数组实现的，所以可以理解为存储数据的容器就是一个线性数组。
2. 首先HashMap里面实现一个静态内部类Entry，重要的属性有 key , value, next，从属性key,value，我们就能很明显的看出来Entry就是HashMap键值对实现的一个基础bean，所以说到HashMap的基础就是一个线性数组。
3. Map里面的内容都保存在Entry[]里面。

## hashmap的key出现hash冲突是如何解决的
利用Entry类的next变量来实现链表，把最新的元素放到链表头，旧的数据则被最新的元素的next变量引用。      


## HashMap为什么线程不安全 {id="hashmap_2"}
1. HashMap内部存储使用了一个Node数组(默认大小是16)，而Node类包含一个类型为Node的next的变量，也就是相当于一个链表，所有hash值相同(即产生了冲突)的key会存储到同一个链表里。
2. HashMap在并发执行put操作时会引起死循环，导致CPU利用率接近100%。因为多线程会导致HashMap的Node链表形成环形数据结构，一旦形成环形数据结构，Node的next节点永远不为空，就会在获取Node时产生死循环。

## 如何让HashMap线程安全 {id="hashmap_3"}
1. 通过Collections.synchronizedMap()返回一个新的Map，这个新的map就是线程安全的      
2. 重新改写了HashMap     
3. 使用hashTable
   

## 如果HashMap的大小超过了负载因子定义的容量，怎么办？
hashMap默认的负载因子大小为0.75，也就是说，当一个map填满了75%的bucket时候，和其它集合类(如ArrayList等)一样，将会创建原来HashMap大小的两倍的bucket数组，来重新调整map的大小，并将原来的对象放入新的bucket数组中。这个过程叫作rehashing，因为它调用hash方法找到新的bucket位置。

## 为什么LinkedHashMap是有序的？
LinkedHashMap.Entry 继承自HashMap.Node 除了Node 本身有的几个属性外，额外增加了before after 用于指向前一个Entry 后一个Entry。也就是说，元素之间维持着一条总的链表数据结构。正式因为这个链表才保证了LinkedHashMap的有序性。

## Hashtable与HashMap的区别
Map是一个以键值对存储的接口，其中键和值都是对象，并且不能包含重复键，但可以包含重复值。Map下有两个具体的实现，分别是HashMap和HashTable。
1. HashMap不是线程安全的，HashTable是线程安全。
2. HashMap允许空键和空值，HashTable则不允许键或值为空。
3. HashMap性能优于Hashtable。


| 比较点  | HashMap             | Hashtable                                  |
|------|---------------------|--------------------------------------------|
| 推出时间 | JDK 1.2之后推出，属于新的操作类 | JDK 1.0时推出，属于旧的操作类                         |
| 性能   | 采用异步处理方式，性能更高       | 采用同步处理方法，性能较低                              |
| 线程安全 | 属于非线程安全的操作类         | 属于线程安全的操作类                                 |
| 空键   | 允许将key设置为null       | 不允许将key设置为null，否则会出现NullPointerException异常 |

## LinkedHashMap和TreeMap区别
1. 只说区别的话，LinkedHashMap 是 HashMap 的子类，TreeMap ，实现了SortMap接口。
2. LinkedHashMap保存了记录的插入顺序，在用Iterator遍历LinkedHashMap时，先得到的记录肯定是先插入的。也可以在构造时带参数，按照应用次数排序。
3. TreeMap不仅可以保持顺序，还能够把它保存的记录根据键排序。默认是按键值的升序排序，也可以指定排序的比较器，当用Iterator 遍历TreeMap时，得到的记录是排过序的。
4. LinkedHashMap 采用是的和HashMap相同的hash算法，TreeMap采用的是红黑树算法的实现

### 应用场景      
Map的key和Set都有一个共同的特性就是集合的唯一性，TreeMap更是多了一个排序的功能。
1. 一般情况下，我们用的最多的是HashMap。HashMap里面存入的键值对在取出的时候是随机的，它根据键的HashCode值存储数据，根据键可以直接获取它的值，具有很快的访问速度。在Map 中插入、删除和定位元素，HashMap 是最好的选择。
2. TreeMap取出来的是排序后的键值对。但如果您要按自然顺序或自定义顺序遍历键，那么TreeMap会更好。
3. LinkedHashMap 是HashMap的一个子类，如果需要输出的顺序和输入的相同,那么用LinkedHashMap可以实现,它还可以按读取顺序来排列，像连接池中可以应用。


## ConcurrentHashmap和HashTable的区别
它们都可以用于多线程的环境
1. Hashtable的大小增加到一定的时候，性能会急剧下降，因为迭代时需要被完全锁定。每次只有一个线程可以操作。
2. Hashtable的任何操作都会把整个表锁住，是阻塞的。好处是总能获取最实时的更新，比如说线程A调用putAll写入大量数据，期间线程B调用get，线程B就会被阻塞，直到线程A完成putAll，因此线程B肯定能获取到线程A写入的完整数据。坏处是所有调用都要排队，效率较低。
3. ConcurrentHashMap锁的方式是稍微细粒度的。 ConcurrentHashMap将hash表分为16个桶（默认值），诸如get,put,remove等常用操作只锁当前需要用到的桶。原来只能一个线程进入，现在却能同时16个写线程进入（写线程才需要锁定，而读线程几乎不受限制），并发性的提升是显而易见的。
4. ConcurrentHashMap 是设计为非阻塞的。在更新时会局部锁住某部分数据，但不会把整个表都锁住。同步读取操作则是完全非阻塞的。好处是在保证合理的同步前提下，效率很高。坏处是严格来说读取操作不能保证反映最近的更新。例如线程A调用putAll写入大量数据，期间线程B调用get，则只能get到目前为止已经顺利插入的部分数据。此外，使用默认构造器创建的ConcurrentHashMap比较占内存

## map常用方法
```Java

map.put(key,value);     // 添加元素
map.remove(key);        // 根据key的值删除键值对
map.size();             // 获得存储元素的长度
map.get(key);           // 通过key获得到value
map.values();           // 获取到所有的value,以Collection对象返回
map.keySet();           // 返回所有key的Set集合
map.entrySet();         // 返回Entry的Set集合对象

// 遍历方式1
Set set = map.keySet();         // 获取到所有Key
Iterator iter = set.iterator(); // 拿到迭代器
while(iter.next()){             // 判断是否有下一个元素
    Object key = iter.next();   // 拿到下一个元素,也就是key
    // 有了key就可以调用get方法,根据key拿到对应的值了
    System.out.println("key:" + key = ",value:" + map.get(key));
}

//遍历方式2
Set entrySet = map.entrySet();  // 拿到所有的Entry对象集合
Iterator it = entrySet.iterator();
while(it.hasNext()){
    Entry entry = (Entry)it.next();
    // 通过entry对象的getKey和getValue方法分别拿到key-value值
    System.out.println("key:" + entry.getKey() + " value:" + entry.getValue());
}

```




