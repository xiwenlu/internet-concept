# 填空题

## jsp填空题

1. jsp的本质为：java servlet
2. jsp中EL表达式的语法为： ${expression}
3. jsp中jstl中常用的判断和循环所对应的标签为：
```javascript
<c:if test="?" var="?"></c:if>
<c:forEach></c:forEach>
````
4. web.xml中，通常需要配置servlet和servlet-mapping
5. servlet中，需要重写：doGet()方法和doPost()方法
6. jsp中通过哪个jsp页面元素`<%@taglib%>`声明当前页面使用的标签
7. 后台通过request提供的`getParameter("uname")`方法获取前台form表单中name属性值为"uname"的输入框的值
8. 后台通过request提供的`setAttribute(key,value)`方法将值放入request对象中传到前台
9. servlet类要继承哪个类`httpServlet`
10. Servlet需要继承HttpServlet这个类
11. 通过调用request对象的request.setCharacterEncoding(“utf-8”)方法来设置编码格式
12. Javabean中的属性名如果开头是大写字母，例如：private String FreeID; 或者private String fRee;  在页面使用EL表达式取值时会造成什么后果页面内容不显示项目部署在了tomcat下的哪个文件夹下webapps
13. Jstl标签中的循环标签是`<c:forEach></c:forEach>`
14. 后台通过request提供的request.setAttribute(key,value)方法将值放入request对象中传到前台
15. 使用request对象的request.getParameter()方法获取表单提交的参数


## js填空题
1. js通过var声明变量
2. 删除数组最后一个元素的方法pop()
3. string对象的常用属性length
4. 正则表达式的格式/^正则表达式$/
5. 通过parseInt("123rfs")方法将字符串"123rfs"转换为数值型
6. 表单提交事件onsubmit="return(具体函数())"
7. 通过location的href属性跳转页面
8. js中获取某一个节点的父级节点通过哪个属性parentNode
9. js中获取某一个节点的子级节点通过哪个属性childNodes
10. js中获取tr下标的属性是哪个rowIndex
11. 表单的事件（2个） onsubmit 提交 onreset 重置
12. 框架中获取其他窗口的window对象 window.parent.name
13. Js中声明变量关键字var
14. Js中number("123abc")的结果是NaN
15. 页面有一个标签 `<input type="text" id="userName">`，Js中怎么样获取到该标签的值document.getElementById("userName").value;
16. 如何使用js获得p标签中的内容innerHTML、innerText
17. Js中设置定时器的两个方法是setTimeout()、setInterval()
18. 写出location对象的两个常用属性href、host
19. Js中通过调用setAttribute(key,value)方法来设置一个元素节点的属性
20. 有一个字符串”吃得苦中苦,方为人上人”，通过Js截取字符串想要得到”苦,方为”，应该怎么操作substr(4,4)
21. 正则表达式中\d代表什么[0-9]匹配数字
22. Js中使用dom创建节点的方法document.createElement()
23. Js操作表格时，使用tr.insertCell()方法创建单元格



## db填空题
1. oracle排序关键字 order by  desc降序 asc升序
2. 分组关键字 group by    ......  having......筛选
3. Oracle数据库默认端口号是1521
4. Oracle数据库中怎样查询系统日期select sysdate from dual;
5. Oracle数据库中去除重复项的关键字distinct
6. Oracle数据库中授权的关键字是grant
7. Oracle中拼接字符串的函数是concat()
8. sql中模糊查询关键字like
9. where语句的执行顺序从后往前
10. 可以通过distinct去重，还可以通过group by去重（分组）
11. where条件中交集的关键字and并集的关键字 or  
12. 在jdbc中可以使用占位符，占位符的下标从1开始
13. 判断ResultSet中有没有下一个元素的方法是rs.next()
14. 有表格为T_USER,字段包括：id,userName,userPwd,age,sex。 写一个sql语句，查询出所有人中，年龄在18到25（包含25）岁之间的所有男性的名字。        
```sql
select userName from t_user where sex='男' and age>18 and age<=25;
```
15. 将A表和B表通过A表中a_bid和B表中bid做sql语句
```sql
# 左链接:  
`A left join B on A. a_bid = B. bid
# 内连接：
A inner join B on A.外键=B.主键
# 全连接：
A full join B on A.外键=B.主键
# 右外连接：
A right join B on A.外键=B.主键

```
    
## Struts2填空题
1. Struts2中action要继承哪个类(ActionSupport)
