# java集合类

<img src="collection.png" alt=""/>

1. Collection框架中实现比较需要实现java.util.Comparator接口或者实现java.lang.Comparable接口。
2. Map和Set集合都是调用equals方法逐一比较是否有相同元素。
3. ArrayList调用clear方法后，开辟的新的数组长度不会变。只会清除里面的非空数据，不会把空间清除掉。
4. ArrayList的底层是由一个Object[]数组构成的，而这个Object[]数组，默认的长度是10。超过了默认长度以1.5倍扩容。
5. ArrayList的size()方法，指的是“逻辑”长度。所谓“逻辑”长度，是指内存已存在的“实际元素的长度” 而“空元素不被计算”，因此取出来的是0。

## 为什么出现集合类？
面向对象语言对事物的体现都是以对象的形式，所以为了方便对多个对象的操作，就对对象进行存储，集合就是存储对象最常用的一种方式。

## 数组和集合类区别
数组和集合类同是容器。数组虽然也可以存储对象，但长度是固定的，而且只能存储相同类型一组数据；集合长度是可变的。数组中可以存储基本数据类型，集合只能存储对象。

## java有集合类有哪些？
数组是Java中最基础的集合类，主要用于存储固定大小的相同类型的数据。而集合则是一组对象的组合，这些对象可以是不同的类型和大小。
1. List：用于存储一组有序的元素，并且允许有重复元素。常用实现类有ArrayList（数组）、LinkedList（链表）、Vector（线程安全）等。
2. Set：用于存储一组不重复的元素。常用实现类有HashSet、TreeSet（二叉树）、LinkedHashSet等。
3. Map：用于存储键值对，其中键是唯一的。常用实现类有HashMap、TreeMap、LinkedHashMap等。
4. Queue：用于存储一组元素，并按照先进先出的原则进行访问。常用实现类有LinkedList、PriorityQueue等。
5. Deque：用于存储一组元素，并支持在两端进行添加和删除操作。常用实现类有LinkedList、ArrayDeque等。
6. SortedMap和SortedSet：用于存储键值对或元素，并按照一定的顺序进行排序。常用实现类有TreeMap、TreeSet等。
7. Collection：是所有集合类的根接口，用于定义集合的基本操作。

## 什么是Collection？
Collection是Java中的集合框架。它是一种统一处理集合的泛型接口，可以用于处理对象的存储和访问。  
Collection提供了对一组对象进行操作的接口，如添加、删除、查找等。它是Java中非常重要的一个部分，为数据的存储和访问提供了统一的标准和方式。  
Collection是单值存放的最大父接口，是集合类的上级接口，继承自它的子接口主要有List、Set、Queue和SortedSet。

## 什么是Collections？
java.util.Collections 是Java集合框架中的一个实用工具类，它提供了一系列静态方法来操作和处理集合。这个类中的方法都是静态的，可以直接通过类名调用，而不需要创建实例。

## Iterator和ListIterator的区别
Iterator是ListIterator的父接口，Iterator是单列集合公共取出容器中元素的方式。对于List，Set都通用。而ListIterator是List集合的特有取出元素方式。Iterator中具备的功能只有hashNext(),next()，remove(); ListIterator中具备着对被遍历的元素进行增删改查的方法，可以对元素进行逆向遍历。

## ArrayList在1.6、1.7、1.8版本升级的变化
1. 1.6没有最大容量限制的，但是jdk1.7做了一个改进，进行了容量限制。
2. 1.6在容量进行扩展的时候，按照整除运算将容量扩展为原来的1.5倍加1，而jdk1.7是利用位运算，从效率上，jdk1.7就要快于jdk1.6。
3. 1.6和1.7在初始化得时候他的长度都是设置好的是10
4. 1.8在初始化的时候长度是null，在进行add时候他的长度才会变成10

## ArrayList、LinkedList和Vector区别
List接口实现类有ArrayList、LinkedList、Vector。

|    特性    |    ArrayList     | LinkedList |     Vector     |  
|:--------:|:----------------:|:----------:|:--------------:|  
|   推出时间   |      jdk1.2      |     -      |     jdk1.0     |  
|  底层数据结构  |        数组        |    双向链表    |       数组       |  
|   查询速度   |        快         |     慢      |       中        |  
|   增删速度   |        慢         |     快      |       慢        |  
|  线程安全性   |       不安全        |    不安全     |       安全       |  
|  动态扩容效率  |  高（每次增加多个存储单元）   | 低（需要遍历链表）  | 高（每次增加多个存储单元）  |  
| 默认初始化容积值 | 初始大小为10，每次增加1.5倍 |     无      | 初始大小为10，每次增加2倍 |

当操作是在一列数据的后面添加数据而不是在前面或中间，并且需要随机地访问其中的元素时，使用ArrayList性能比较好；当操作是在一列数据的前面或中间添加或删除数据，并且按照顺序访问其中的元素时，就应该使用LinkedList了。
> 想获取更多，请阅览[数组和链表的区别](data-structure.md#data_1)。

## List、Map和Set接口的区别

|  特性   |        List（列表）        |      Map（映射）       |      Set（集合）      |  
|:-----:|:----------------------:|:------------------:|:-----------------:|  
|  元素   |          单列元素          |        双列元素        |       单列元素        |
| 继承接口  |       Collection       |      Iterable      |    Collection     |  
| 元素重复性 |        可以有重复元素         |   不允许重复键，可以有重复值    |      不允许重复元素      |  
| 元素有序性 |       有序，保持插入顺序        | 无序，但某些实现类可能对元素进行排序 |        无序         |  
| 主要实现类 | ArrayList, LinkedList等 | HashMap, TreeMap等  | HashSet, TreeSet等 |

## 什么是treeSet?
Java中的TreeSet是一个基于树结构的Set集合，它继承自AbstractSet类，并实现了NavigableSet接口。TreeSet集合按照元素的自然顺序或者自定义顺序进行排序，并且不允许存储重复元素。
1. 排序：TreeSet集合按照元素的自然顺序或者自定义顺序进行排序，可以根据元素类型或自定义Comparator对象来指定排序方式。
2. 唯一性：TreeSet集合不允许存储重复元素，如果试图添加重复元素，则会抛出IllegalArgumentException异常。
3. 高效性：TreeSet集合内部使用红黑树数据结构进行存储，因此在插入、删除和查找等操作上具有较高的效率。

## HashSet和TreeSet的区别

|       --       |          HashSet           |     TreeSet     |  
|:--------------:|:--------------------------:|:---------------:|
|     底层数据结构     |         HashMap哈希表         |   TreeMap二叉树    |
|     元素的唯一性     | 唯一，hashCode()方法和equals()方法 | 重复，Comparator方法 |
|       顺序       |             无序             |       有序        |
|      null      |             允许             |       不允许       |
| 插入、删除、查找的时间复杂度 |            O(1)            |    O(log n)     |


## 遍历集合有几种方法？
1. For-Each 循环这是最简单的方法，适用于大多数集合类型。
2. 使用 Iterator 这是更通用的方法，因为不是所有的集合都支持for-each循环。
3. 使用 for 循环和索引，这种方法通常在需要随机访问集合元素时使用，但是需要注意越界问题。