# junit

## junit单元测试 {id="junit_1"}
我在编写完自己的功能模块后，为了保证代码的准确性，一般都会使用junit进行单元测试，当时使用的是junit4这种基于注解的方式来进行单元测试。为了和spring集成获取配置的bean,通常使用 ``@RunWith来加载springjunit这个核心类，使用 @ContextConfiguration来加载相关的配置的文件，通过 @Resource按名字来注入具体的bean``,最后在需要测试的方法上面加上@Test 来进行单元测试。并且在编写单元测试的时候还要遵守一定的原则如：源代码和测试代码需要分开；测试类和目标源代码的类应该位于同一个包下面，即它们的包名应该一样；测试的类名之前或之后加上@Test，测试的方法名通常也以test开头。

```java
//运行spring相关环境 相当于spring监听功能
@RunWith(SpringJUnit4ClassRunner.class)
//读取spring配置文件 不识别* 只能识别具体文件 多个配置文件使用string数据传递
@ContextConfiguration(locations={"classpath:spring-common.xml","classpath:spring-datasource.xml"})
public class TestSpring {
    //注入Service层
    private @Resource UserService userService;
    
    @Test
    public void testFind(){
        List<User> userList = userService.findAllUserInfo();
        for (User user : userList) {
            System.err.println(user.toString());
        }
    }
}
```

## junit
junit是一个开放源代码的java测试框架，继承TestCase类，用于编写和运行可重复的测试。他是用于单元测试框架体系xUnit的一个实例（用于java语言）。它包括以下特性：      

1. 用于测试期望结果的断言（Assertion）
2. 用于共享共同测试数据的测试工具
3. 用于方便的组织和运行测试的测试套件
4. 图形和文本的测试运行器

## 什么是单元测试？
单元测试属于白盒测试。单元测试主要是针对一个类的public的方法来做测试，验证所设计的类是否符合需求，对软件设计的最小单位——程序模块进行正确性检验的测试。

## 什么黑盒测试？
黑盒测试也称功能测试，它是通过测试来检测每个功能是否都能正常使用。

## 什么是白盒测试？
白盒测试也成为代码测试，用来找出代码中存在的问题，保证代码能通过编译，正确运行。

## junit行顺序 {id="junit_2"}
BeforeClass----Before-----Test-----After-----Before-----Test-----After-----AfterClass

## junit和main的区别
1. junit与main方法相比的优势：代码量少、结构清晰、灵活性更好
2. main：一个类中只能有一个main方法，层次结构方面不够清晰，运行具体某一个方法时，要将其他的方法注释掉


## 断言能在生产使用吗？
在Java中，断言默认是禁用的，所以在生产环境中，你不需要做任何事情来禁用断言。如果你想在开发或测试环境中启用断言，你可以在启动Java应用时添加-ea或-enableassertions参数。