# java集合

<img src="collection.png" alt=""/>

1. Collection框架中实现比较需要实现java.util.Comparator接口或者实现java.lang.Comparable接口。
2. Map和Set集合都是调用equals方法逐一比较是否有相同元素。
3. ArrayList调用clear方法后，开辟的新的数组长度不会变。只会清除里面的非空数据，不会把空间清除掉。
4. ArrayList的底层是由一个Object[]数组构成的，而这个Object[]数组，默认的长度是10。超过了默认长度以1.5倍扩容。
5. ArrayList的size()方法，指的是“逻辑”长度。所谓“逻辑”长度，是指内存已存在的“实际元素的长度” 而“空元素不被计算”，因此取出来的是0。

## 集合概述
Collection 集合父接口
### List
有序集合接口,可以重复
1. ArrayList 线程不安全,底层由数组实现,查询快,增删慢 
2. LinkedList 线程不安全,底层由链表实现,查询慢,增删快
3. Vector 线程安全
### Set {id="set_1"}
无序集合接口,不可以重复
1. HashSet 底层由Hash码实现
2. TreeSet 底层由二叉树实现,可以自动排序

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

## list常用方法 {id="list_method"}
```Java

List list = new LinkedList();
//泛型的用法 List<数据类型> list = new LinkedList<数据类型>();

list.add("万薪就业");       // 往集合中添加数据
list.add(0,"金科教育");     // 往下标0的位置插入数据
list.remove(0);             // 删除下标为0的元素
list.remove("金科教育");    // 删除元素为'金科教育'的元素
list.clear();               // 清空集合中的数据
list.size();                // 获取集合中元素的长度
list.get(0);                // 获取下标为0的元素
list.iterator();            // 获得迭代器,可以通过迭代器遍历集合中的元素

//List集合的两种遍历方法
//迭代器遍历
Iterator iterator = list.iterator();    // 拿到迭代器
while(iterator.hasNext()){              // 判断是否存在下一个元素
    //如果有下一个元素,通过next()方法拿到这个元素
    Object object = iterator.next();
    System.out.println(object);
}

//通过下标循环遍历
for(int i=0;i<list.size();i++){
    System.out.println(list.get(i));
}

```


## set常用方法
```Java
Set set = new HashSet();
set.add("金科教育");         // 往集合中添加数据,如果有重复的将添加不进去,对象类型的参数,记得要重写equals方法才行
set.remove("金科教育");      // 删除元素为'金科教育'的元素
set.clear();                // 清空集合中的元素
set.size();                 // 获得集合中元素的长度
set.iterator();             // 获得迭代器,Set集合只能通过迭代器遍历集合中的元素

//迭代遍历
Iterator iterator = set.iterator(); // 拿到迭代器
    while(iterator.hasNext()){      // 判断是否存在下一个元素
    // 如果有下一个元素,通过next()方法拿到这个元素
    Object object = iterator.next();
    System.out.println(object);
}
```
