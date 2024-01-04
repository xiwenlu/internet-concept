# java字符串

## String
1. 是不可变的;最终类;无法被继承;底层实现是char数组;知道有串池的存在
2. 尽量不要使用String进行字符串拼接的操作,也就是用+号拼接
```Java
// 赋值的两种形式
String str1 = new String("金科教育");
String str2 = "万薪就业";

// 字符串常用方法
// replace,replaceAll,substring,trim,toUpperCase,toLowerCase方法都是返回新的字符串，并不是修改了当前字符串
str1.indexOf("金");      // 返回指定字符串在str1对象中第一次出现的位置,如果不存在则返回-1
str1.length();          // 获得字符串的长度
str1.replace("a","b");
str1.replaceAll("a","b");// 将str1对象中出现的'a'全部替换成'b'并返回
str1.split(",");        // 将str1中的字符串用','分隔为一个String数组返回
str1.substring(5,10);   // 返回下标5到10之间的字符串(不包括10)
str1.toCharArray();     // 将字符串转换成char数组返回
str1.trim();            // 将字符串两端的空格去掉后返回
str1.toUpperCase();     // 将字符串中的英文全部转换成大写字母返回
str1.toLowerCase();     // 将字符串中的英文全部转换成小写字母返回

```

## String中常用的转义符 {id="string_trans"}

| 转义符 |  描述   |  
|:---:|:-----:|  
| \n  |  回车   |  
| \t  | 水平制表符 |  
| \b  |  空格   |  
| \r  |  换行   |  
| \’  |  单引号  |  
| \”  |  双引号  |  
| \\  |  反斜杠  |


## StringBuilder和StringBuffer
都是可变字符串类型,拼接效率远高于String的拼接
1. StringBuilder 线程不安全
2. StringBuffer 线程安全
```Java
sb.append("");//拼接字符串方法
sb.toString();//返回拼接好的字符串

```

## StringBuffer、StringBuilder和String区别

<table border="1" cellpadding="1" cellspacing="1">
    <tbody>
    <tr>
        <td style="width:500px;">类</td>
        <td style="width:500px;">线程安全性</td>
        <td style="width:500px;">可变性</td>
        <td style="width:500px;">用途</td>
    </tr>
    <tr>
        <td style="width:500px;">String</td>
        <td style="width:500px;">安全</td>
        <td style="width:500px;">不可变</td>
        <td style="width:500px;">只读</td>
    </tr>
    <tr>
        <td style="width:500px;">StringBuffer</td>
        <td style="width:500px;">安全</td>
        <td style="width:500px;">可变</td>
        <td style="width:500px;">多线程</td>
    </tr>
    <tr>
        <td style="width:500px;">StringBuilder</td>
        <td style="width:500px;">不安全</td>
        <td style="width:500px;">可变</td>
        <td style="width:500px;">单线程</td>
    </tr>
    </tbody>
</table>

性能

1. 在单线程环境中，由于StringBuilder的非同步性，它的性能通常优于StringBuffer。
2. 在多线程环境中，由于StringBuffer的同步性，它可能会引发性能问题，因为每次调用方法都需要等待锁。
3. String因为是不可变的常量，每次赋值都会创建一个新的对象。StringBuffer和StringBuilder都是可变的，当进行字符串拼接时采用append方法，在原来的基础上进行追加，所以性能比String要高。


## 字符串创建了多少个对象？

```Java
# 1个对象。对于常量，编译时就直接存储它们的字面值，而不是它们的引用。
String s = "a" + "b" + "c" + "d" + "e";

# StringBuilder、new char[capacity]和new String(value,0,count)一共3个对象，但只有一个String对象。
# 由于编译器的优化，最终代码为通过StringBuilder完成。会分配一个16字节长度的char数组。总长度超过16，容量增加为当前(长度+1)*2。会使用Arrays.arraycopy的复制方法，产生了一个新的copy的char数组，长度为新的长度。
String s = a+b+c+d+e;

# 如果字符串常量池中不存在“abc”,该语句执行时会先在字符串常量池中创建一个“abc”对象，在执行new语句时在堆去开辟新的空间，创建“abc”字符串，同时栈区会有一个引用s指向堆区的对象，此时如果要算上栈区的引用，共创建3个对象，不算，则创建2个对象。
# 如果字符串常量池中存在“abc”,则只会在堆区创建一个“abc”字符串，同时栈区有一个引用指向堆中的对像。如果算上栈中的引用，共创建了两个对象，不算，则创建了1个对象。
String s=new String("abc");

```

