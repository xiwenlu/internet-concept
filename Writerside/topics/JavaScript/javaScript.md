# JavaScript


## 页面上有一个dom对象，使用js代码实现在点击此dom对象时调用一个js函数
首先先给dom对象一个属性onclick=”函数名()”,在javascript定义一个一个函数，一定要和dom对象绑定的函数名一致，点击就触发此函数!

## js和java的区别
1. Java是需要编译才能执行，js不需要编译只需要有支持他的浏览器解析并执行。
2. Java和js都运用于服务器和客户端，Java主要运用于服务器，js主要运用于客户端。
3. Java是严谨型数据类型，js是松散型数据类型。Java数据类型是强类型，js数据类型是弱类型。 

## js的基本数据类型 {id="js_1"}
1. 一般数据类型：string字符串 boolean布尔 number数值
2. 复合数据类型：array数组 Object
3. 特殊数据类型：null undefined      

## js中的事件以及意义 {id="js_2"}
一般事件
```javascript
onfocus() // 获取焦点事件      
onblur()  // 失去焦点事件        
onchange() // 内容改变事件
```
页面事件  
```javascript
onload()    // 页面加载事件   
onunload()  // 页面卸载事件  
```    
表单事件    
```javascript
onsubmit()  // 表单提交事件     
onreset()   // 表单重置事件  
```    
键盘事件    
```javascript
onkeyup()   // 键盘抬起事件      
onkeydown() // 键盘按下事件     
onkeypress()// 键盘按下并抬起事件
```             
鼠标事件
```javascript
onclick()       // 鼠标单击事件      
ondblclick()    // 鼠标双击事件    
onmousedown()   // 鼠标按下事件       
onmouseup()     // 鼠标抬起事件     
onmousemove()   // 鼠标移动事件       
onmouseover()   // 鼠标移入事件       
onmouseout()    // 鼠标移出事件	
```


## js中的内置对象 {id="js_3"}
math对象
```javascript
ceil()  // 向上取整     
floor() // 向下取整    
abs()   // 绝对值   
round() // 四舍五入    
random() // 随机数

```
                     
string对象
```javascript
concat()    // 将两个或多个字符串拼接在一起形成新的字符串       
bold()      // 给字符串加粗        
replace()   // 将字符串中的目标字符替换成新的字符         
substr()    // 按下标和个数截取字符串            
substring() // 按下表截取字符串  
```
          
date对象
```javascript
getfullyear()   // 获取系统当前年	    
getmonth()      // 获取系统当前月份         
getday()        // 获取系统当前星期数
getdate()       // 获取系统当前日期  
tolocalestring() // 根据当前区域设置并转化为字符串返回  
toString()      // 将日期转化为字符串返回   
```
       
array对象
```javascript
concat()    // 将两个数组或多个数组拼接成一个新的数组            
pop()       // 移除数组中最后一个元素并返回该元素     
push()      // 向数组中添加一个元素并返回数组的长度       
tostring()  // 将数组以字符串的形式返回         
join()      // 返回一个字符串，字符串是以指定的符号分隔   
```
  
document
```javascript
document.getElementById("id")           // 根据id获取元素(Element)
document.getElementsByName("name")      // 根据name获取元素集合
document.getElementsByTagName("标签名")  // 根据标签名获取元素集合
```

window
```javascript
alert("信息")                     // 提示框，无返回值
confirm()                           // 确定框，返回boolean类型，确定：true；取消：false
prompt("提示信息", [初始默认值])     // (提示信息, [默认值])文本输入弹框，点击取消：返回null，点击确定：返回所输入的字符串
```

## js的内建函数
```javascript
number()    // 将参数转化为数值类型
isnan()     // 判断参数是否是数值类型 是返回false 不是则返回true
parseInt()  // 返回参数中的整数
parseflot() // 返回参数中的小数
eval()      // 将参数字符串作为代码在上下文中运行
```
    
