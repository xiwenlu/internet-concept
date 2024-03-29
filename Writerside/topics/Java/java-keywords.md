# java关键字

1. 在继承过程中，子类使用this关键字调用本类的方法或属性。


## 访问权限修饰符
访问范围从大到小：public>protected>default>private。

| 访问权限修饰符 |    关键字    |  意思  |       可以访问的范围        |  
|:-------:|:---------:|:----:|:--------------------:|  
|   公共    |  public   | 公共的  |         整个项目         |  
|   受保护   | protected | 受保护的 | 在同一个包中使用，能在不同包的子类中使用 |  
|   默认    |  default  | 默认的  |      只能在同一个包中使用      |  
|   私有    |  private  | 私有的  |     只能在当前类的内部使用      |


## java中continue和break的区别
在Java中，continue和break都是控制流程的语句。在使用时，应根据具体的需求和上下文选择适当的控制流程语句。
1. continue用于跳过当前循环的剩余代码并开始下一次循环
2. break用于终止整个循环并跳出循环体。

## java的this关键字
在Java中，this是一个关键字，用于引用当前对象的实例。
1. 引用当前对象的实例变量：当一个实例变量与一个局部变量或参数同名时，可以使用this关键字来引用当前对象的实例变量。
2. 调用当前对象的另一个构造方法：在一个构造方法中，可以使用this()来调用同一个类中的另一个构造方法。
3. 返回当前对象：在某些情况下，可以使用this关键字来返回当前对象的引用。

### java的this关键字哪些场景下不能使用 {id="java-this_not_use"}
this关键字不能用于静态方法或静态变量，因为静态成员属于类而不是实例。此外，this关键字也不能用于数组的实例。

## java的super关键字
在Java中，super关键字是一个引用变量，用于引用父类(超类)的一个对象。它提供了一种机制，使得子类可以在不改变现有代码的情况下扩展或修改父类的行为。
1. 访问父类的成员变量：当子类和父类具有相同的成员变量时，可以使用super关键字来引用父类的成员变量。
2. 调用父类的方法：当子类需要调用父类的被重写的方法时，可以使用super关键字。

### java的super关键字特点 {id="java-super_characteristic"}
1. super代表父类的引用，用于访问父类中的属性和方法。
2. 在子类中，当子类和父类存在同名的属性或方法时，可以使用super关键字来区分它们。
3. 在子类的构造方法中，可以使用super关键字来调用父类的构造方法，这是子类构造方法中的第一条指令。
4. super关键字不仅限于直接父类，如果子类有间接父类，且权限允许，可以使用super关键字进行调用。
5. super关键字不是引用类型的变量，它不存储内存地址，而是代表当前子类对象中的父类型特征。

## java中static关键字
在Java中，static是一个关键字，用于声明静态成员。静态成员属于类，而不是类的任何特定实例。这意味着无论创建了多少个类的实例，都只有一份静态成员的拷贝。所有的实例共享同一个静态成员。

### java中static关键字可以修饰什么？ {id="java-static_decorate"}
被static修饰的变量、方法和类都在类加载的时候创建，可以不需要创建对象通过类名直接调用。
1. 静态变量：静态变量也称为类变量，因为它们属于类而不是实例。所有实例共享同一个静态变量。这意味着，如果你更改了静态变量的值，所有实例都会看到这个更改。 
2. 静态方法：静态方法属于类而不是实例。因此，你可以在不创建类的实例的情况下调用它。静态方法只能直接访问静态成员（变量和方法）。 
3. 静态块：静态块在类加载时执行，只执行一次。它可以用于初始化静态变量。 
4. 静态内部类：静态内部类是定义在另一个类中的类，不需要外部类的实例就可以创建其对象。


### java中static关键字的局限性 {id="java-static_boundedness"}
由于静态成员不属于任何特定实例，因此不能直接访问非静态方法或非静态变量，因为后者需要一个对象实例才能存在。

### java中static关键字的应用 {id="java-static_apply"}
除了在类成员的上下文中，static还可以用于声明静态导入，使你能够导入其他类或接口的静态成员，而不必每次都写出完整的限定名。例如：import static java.lang.Math.*;。

### java中static关键字内存中的位置 {id="java-static_position"}
由于静态变量和静态方法在内存中只存在一份拷贝，因此它们通常用于实现“单例模式”或“工厂模式”等设计模式。


### java中static关键字与final的关系
当一个字段被声明为static final时，它就是一个常量。一旦赋值后，其值就不能更改。这通常用于定义编译时常量。

### java中static关键字的性能考虑 {id="java-static_performance"}
使用静态变量和静态方法可以节省内存和CPU时间，因为不需要为每个对象实例创建和存储这些成员的副本。但这也可能导致线程安全问题，因为多个线程可能同时访问和修改同一个静态变量。因此，在使用静态变量时需要特别注意线程安全问题。


## java中final关键字
在Java中，final是一个关键字，用于声明常量或变量、方法和类。它可以提高代码的安全性和性能，但应谨慎使用以避免过度限制代码的灵活性。

### java中final关键字可以修饰什么？ {id="java-final_decorate"}
1. 常量（常量变量）：当一个变量被声明为final时，它的值一旦被初始化后就不能再被修改。这种常量变量通常用于定义编译时常量。
2. 方法：将一个方法声明为final意味着该方法是最终的方法，不能被子类重写（override）。
3. 类：将一个类声明为final意味着该类是最终类，不能被继承。
4. 内部类：对于局部内部类和匿名内部类，可以将其声明为final。这通常用于实现某些设计模式或确保代码的特定行为。

### java中final关键字性能 {id="java-final_performance"}
使用final关键字可以确保数据不会被改变，这对于某些性能敏感的场景是有益的。例如，当某些变量在初始化后不会被改变时，将其声明为final可以提高缓存命中率，从而提高性能。

### java中final关键字安全性 {id="java-final_safety"}
通过限制方法和类的可继承性，使用final关键字可以提高代码的安全性，防止某些不期望的行为发生。

### java中final关键字与其他关键字的组合 {id="java-final_compose"}
final可以与其他关键字（如static）结合使用，以实现更复杂的效果。例如，static final常用于定义编译时常量。

### java中final关键字注意事项 {id="java-final_note"}
过度使用final可能会导致代码灵活性降低，因此应谨慎使用。在某些情况下，使用设计模式或其他方法可能是更好的选择。

### java中final关键字与其他语言的比较 {id="java-final_compare"}
在其他一些编程语言中（如C++），也有类似于final的关键字或概念，但其具体用法和含义可能有所不同。

## java中abstract关键字
1. 在Java中，abstract是一个关键字，用于声明抽象类或抽象方法。 
2. 当一个类继承抽象类时，它必须提供抽象类中所有抽象方法的实现。如果子类没有实现所有的抽象方法，那么子类也必须声明为抽象类。

### java中abstract关键字可以修饰什么？ {id="java-abstract_decorate"}
1. 抽象类：使用abstract关键字声明的类被称为抽象类。抽象类不能被实例化，即不能直接使用new关键字创建其对象。抽象类通常作为其他类的基类，用于定义通用的属性和方法。只能通过继承使用，使用多态的特性用子类给父类赋值。
2. 抽象方法：抽象方法是一种只有声明没有实现的方法，它的实现由抽象类的子类提供。抽象方法使用abstract关键字声明，并且没有方法体。抽象方法只能出现在抽象类中，但是抽象类中可以有普通方法。

> 更多参阅：[什么是java的抽象类？](java-class.md#java_abstract)
> 更多参阅：[什么是java的抽象方法？](java-method.md#java_abstract_method)

## ## java的extends继承关键字
1. 必须出现在子类中
2. 子类继承父类时，可以继承父类的属性和方法，也就是拥有了父类的特性
3. 子类不能继承父类的private特性和构造方法
4. 一个子类只能有一个父类，但是一个父类可以有多个子类
5. 实例化子类对象时默认先调用父类的构造方法
6. public class 子类 extends 父类


## java中private关键字
在Java中，private是一个访问修饰符，用于限制类的成员（变量、方法等）的可见性和访问权限。当一个类成员被声明为private时，它只能在声明它的类内部被访问，而不能从其他类中访问。

### java中private关键字的好处 {id="java-private_1"}
1. 封装：通过将类的内部实现细节隐藏起来，只暴露必要的接口，可以提高代码的可维护性和安全性。
2. 简化设计：将类的内部实现细节与外部接口分离，使得类的设计更加清晰和简单。
3. 防止外部错误：如果一个类的内部状态被外部直接访问和修改，可能会引发各种错误和问题。通过将类的成员设置为private，可以防止外部代码直接修改它们，从而减少错误的可能性。