# poi

## poi导出数据解决内存溢出 {id="poi_1"}
poi在3.8版本后开始支持大数据的导出，首先new一个SXSSFWorkbook，并设置内存中最多放5000条数据，之后的数据poi会自己写成临时文件存到到服务器的物理硬盘中，通过线程池将所有数据导完后会自动合成一个excel文件，然后调用workorkbook.dispose();清除服务器上的poi临时文件，然后我们通过IO流将这个excel响应给浏览器下载，最后将服务器上的excel删除掉。


## poi导出结合多线程使用
当时在使用线程池的时候呢我们是将常用的四个线程池封装作为一个工具类来使用的，在做导出数据的时候虽然封装了四个线程池，但是我们用的是定长线程池newFixedThreadPool，并且设置次线程池中线程的数量为50，同时创建了一个用户信息导出的线程类用来实现Runnable接口，传入了service业务处理层对象及用户对象两个私有的全局变量，重写run方法，建了一个有参构造方法，传入了两个参数，分别是service业务处理层对象，及用户对象，之后在run方法中通过service业务处理层对象调用业务处理层接口实现类中的poi导出方法，最后在Controller控制层中通过封装的线程池工具类对象调用FixedThreadPool定长线程池方法在方法中new一个线程实现类将其线程实现类中的方法调用过来返回给前台浏览器进行下载导出。

## 如果poi 导出 大批量数据。思路200字，核心代码5行以上。 {id="poi-200-5_1"}
首先我们要知道，在POI以前的版本中并不支持大数据量的处理，如果数据量过多还会常报OOM错误，这时候调整JVM的配置参数也不是一个好对策（注：jdk在32位系统中支持的内存不能超过2个G，而在64位中没有限制，但是在64位的系统中，性能并不是太好）,好在POI3.8版本新出来了一个SXSSFWorkbook对象，它就是用来解决大数据量以及超大数据量的导入导出操作的，但是SXSSFWorkbook只支持.xlsx格式，不支持.xls格式的Excel文件。        

在POI中使用HSSF对象时，excel 2003最多只允许存储65536条数据，一般用来处理较少的数据量，这时对于百万级别数据，Excel肯定容纳不了，而且在计算机性能稍低的机器上测试，就很容易导致堆溢出。而当我升级到XSSF对象时，它可以直接支持excel2007以上版本，因为它采用ooxml格式。这时excel可以支持1048576条数据，单个sheet表就支持近104万条数据了,虽然这时导出100万数据能满足要求，但使用XSSF测试后发现偶尔还是会发生堆溢出，所以也不适合百万数据的导出。      

刚才我说过了excel2007及以上版本可以轻松实现存储百万级别的数据，但是系统中的大量数据是如何能够快速准确的导入到excel中这好像是个难题，对于一般的web系统，我们为了解决成本，基本都是使用的入门级web服务器tomcat，既然我们不推荐调整JVM的大小，那我们就要针对我们的代码来解决我们要解决的问题。在POI3.8之后新增加了一个类，SXSSFWorkbook，采用当数据加工时不是类似前面版本的对象，它可以控制excel数据占用的内存，他通过控制在内存中的行数来实现资源管理，即当创建对象超过了设定的行数，它会自动刷新内存，将数据写入文件，这样导致打印时，占用的CPU，和内存很少。但有人会说了，我用过这个类啊，他好像并不能完全解决，当数据量超过一定量后还是会内存溢出的，而且时间还很长。对你只是用了这个类，但是你并没有针对你的需求进行相应的设计，仅仅是用了，所以接下来我要说的问题就是，如何通过SXSSFWorkbook以及相应的写入设计来实现百万级别的数据快速写入。      

我先举个例子，以前我们数据库中存在大量的数据，我们要查询，怎么办？我们在没有经过设计的时候是这样来处理的，先写一个集合，然后执行jdbc，将返回的结果赋值给list，然后再返回到页面上，但是当数据量大的时候，就会出现数据无法返回，内存溢出的情况，于是我们在有限的时间和空间下，通过分页将数据一页一页的显示出来，这样可以避免了大数据量数据对内存的占用，也提高了用户的体验，在我们要导出的百万数据也是一个道理，内存突发性占用，我们可以限制导出数据所占用的内存，这里我先建立一个list容器，list中开辟10000行的存储空间，每次存储10000行，用完了将内容清空，然后重复利用，这样就可以有效控制内存，所以我们的设计思路就基本形成了，所以分页数据导出共有以下3个步骤：
1. 求数据库中待导出数据的行数
2. 根据行数求数据提取次数
3. 按次数将数据写入文件

`使用excel2007以上版本_徐恒核心代码注释`  
```java
//求出要导出数据的总条数
int counts = mlsGoodsMapper.selectAllCount();
//设定一次导出1000条数据
final int threadSize=1000;
//求出要导出的次数
int count=(int)((float)counts/threadSize);
//创建100个线程
ExecutorService ThreadPool = Executors.newFixedThreadPool(100);
//子线程是否走完的标识
final CountDownLatch doneSignal = new CountDownLatch(count);
//设置默认文件名为当前时间：年月日时分秒
String fileName=new SimpleDateFormat("yyyyMMddhhmmss").format(new Date()).toString();
response.reset();
response.setContentType("application/vnd.ms-excel");        //改成输出excel文件
response.setHeader("Content-disposition","attachment; filename="+fileName+".xls" );
//HSSFWorkbook workbook = new HSSFWorkbook();
//final DaoChuExcel2<T> daoChuExcel2 = new DaoChuExcel2<T>(fieldMap, counts, 30000, workbook, "qwe", mlsGoodsMapper);
WritableWorkbook workbook = Workbook.createWorkbook(response.getOutputStream());
final DaoChuExcel<Object> daoChuExcel = new DaoChuExcel<>(fieldMap, counts, 30000, workbook, "q", mlsGoodsMapper);
for (int i = 0; i < count; i++) {
    final int j=i;
    ThreadPool.execute(new Runnable() {
        public void run() {
        daoChuExcel.getPage(j, threadSize);
        //标识减一
        doneSignal.countDown();
        }
    });
}
doneSignal.await();
ThreadPool.shutdown();
System.out.println("ok 了！！！");
//workbook.write(response.getOutputStream());  
//response.getOutputStream().close();
workbook.write();
workbook.close();
```
## 如果poi 导出 大批量数据。思路200字，核心代码5行以上。      
首先，后台查询要使用分页，避免内存溢出，循环查出每一页信息，开启Java多线程，Java中提供了单例、定长、缓存等线程池，使用起来比较方便，这个单例线程池里只有一个线程在工作，不会造成cpu资源过度占用，所以最好使用这个，把每一页查出的信息扔到这个线程池中生成对应的sheet工作簿，最后根据线程池的执行状态判断任务是否都执行完了，当执行完任务之后就直接把HSSFWorkBook下载到客户端浏览器上            
`具体代码：`
```java 
//首先创建一个线程池
SingleThreadPool pool = Executors.newSingleThreadExecutor();

//查询并计算出总页数
Int total = 10;

//创建workBook对象
HSSFWorkbook workbook = new HSSFWorkbook();

//For循环
For(int i = 0; i < total; i++) {

    //创建sheet工作薄
    HSSFSheet sheet = workbook.createSheet("hello" + i);
    
    //执行ecxute方法  new Runable()
    pool.execute(new Runable(){
    
        public void run(){
        
        try{
            //……
        }
    });
}



```
