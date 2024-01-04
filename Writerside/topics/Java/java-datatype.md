# java数据类型

## java的数据类型 {id="java_1"}
Java是一种强类型语言，这意味着在声明变量时必须指定其数据类型。Java支持多种数据类型，包括基本数据类型和引用数据类型。

### 基本数据类型
原始数据类型也称为基本数据类型，它们是Java语言中预定义的一些简单数据类型。
1. 整型包含byte（字节型）、short（短整型）、int（整型）和long（长整型）。
   - byte：8位有符号二进制整数，范围从-128到127。
   - short：16位有符号二进制整数，范围从-32768到32767。
   - int：32位有符号二进制整数，范围从-2^31到2^31-1。
   - long：64位有符号二进制整数，范围从-2^63到2^63-1。
2. 浮点型包含float（单精度浮点型）和double（双精度浮点型）。
   + float：32位IEEE 754单精度浮点数。
   + double：64位IEEE 754双精度浮点数。
3. 字符型char。16位Unicode字符，范围从'\u0000'（即0）到'\uffff'（即65,535）。
4. 布尔型boolean。只有两个可能的值，true和false。

### 引用数据类型
1. 类（Class）：用于定义对象的蓝图或模板。
2. 接口（Interface）：是类的一个类型，用于定义一组方法，这些方法可以在类中被实现。
3. 数组（Array）：可以包含多个相同类型的元素。

请注意，所有基本数据类型都是按值传递的，而对象引用是按值传递对象的引用。这意味着当你将一个对象作为参数传递给一个方法时，实际上传递的是对象的引用，而不是对象本身。

## 基本数据类型的转换
1. 默认转换。发生在数据类型较小的变量转换成数据类型较大的变量时
2. 强制转换。当较大的类型转换成较小的类型时,可能会出现损失精度
3. boolean类型不能和任何类型发生转换

## long转换为int
long是64位，int是32位。当使用强制类型把long转换为int类型有可能会出现负数问题。

## 基本类型对应的包装类


|  基本类型   |    包装类    |  
|:-------:|:---------:|  
|  byte   |   Byte    |  
|  short  |   Short   |  
|   int   |  Integer  |  
|  long   |   Long    |  
|  float  |   Float   |  
| double  |  Double   |  
|  char   | Character |  
| boolean |  Boolean  |

### 常用方法
```Java
对象名.xxxValue();   //xxx为具体数据类型,如:对象.intValue();转换成相应语句类型的数据
包装类名.parseXxx(); //Xxx为具体数据类型,此方法为静态方法,将字符串类型的数字,转变成数值型的数字
```


## 什么是java的自动拆装箱？ {id="java_pack"}
Java的自动拆装箱是指基本类型和对应的包装类之间的自动转换。
当基本类型作为参数传递给方法时，Java会自动将其转换为对应的包装类对象；当包装类对象被赋值给基本类型变量时，Java会自动将其拆箱为基本类型值。这种自动转换功能简化了代码编写，提高了开发效率。

## 什么时候java的自动装箱不起作用？ {id="java_pack_error"}
1. 当存在重载的方法，其中一个方法使用基本类型作为唯一参数，而另一个方法使用该基本类型的包装类作为唯一参数时，自动装箱可能不会按预期工作。在这种情况下，编译器可能会选择使用基本类型的方法，而不是包装类的方法，从而导致自动装箱不起作用。
```Java
class Test {  
    public static void add(int i) {  
        System.out.println("int add");  
    }  
      
    public static void add(Integer i) {  
        System.out.println("Integer add");  
    }  
}
```
2. 当涉及到方法的返回类型时，自动装箱也可能不起作用。如果方法的返回类型是基本类型，而该方法被重写为返回包装类对象时，编译器可能会选择使用基本类型的方法，而不是包装类的方法，从而导致自动装箱不起作用。
```Java
class Test {  
    public static int add(int i) {  
        return i + 1;  
    }  
      
    public static Integer add(Integer i) {  
        return i + 1;  
    }  
}
```

## 下面程序打印结果
```Java
    public static void main(String[] args) {
        Integer n1 = new Integer(30);
        Integer n2 = new Integer(30);
        int n3 = 30;
        Integer n4 = 30;
        Integer n5 = 30;
        Integer n6 = 128;
        Integer n7 = 128;
        System.out.println(n1 == n2);   //         false
        System.out.println(n1.equals(n2));  //         true
        System.out.println(n1 == n3);   //         true
        System.out.println(n1.equals(n3));  //         true
        System.out.println(n1 == n4);   //         false
        System.out.println(n4 == n5);   //         true
        System.out.println(n6 == n7);   //         false

    }


```
