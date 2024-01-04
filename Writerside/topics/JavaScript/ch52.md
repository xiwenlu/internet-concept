# easyui

## easyui分页的实现思路? {id="easyui_1"}
1. 导入jquery和easyui的js文件，第二，在body中声明一个table当载体也就是dategrid在页面上的位置，然后通过js方法初始化datagrid 首先，定义colomn这个来规定当前表格有几列，标题和字段名称。
2. 第二还有url 这个是后台返回json的数据 ，还有需要加入 pagination =true开启分页组件，同时 后台定义page rows 来接收前台传过来的 当前页 和每页几条，然后用持久层技术，求出 对应的数据，还有总条数。
3. 需要注意的是，返回的 list数据 必须叫rows 总条数必须叫total。

## easyui常用组件？ {id="easyui_2"}
Tree(树)、panel(面板)、Accordion(手风琴)、Tabs(选项卡)、Layout(布局)、DataGrid(数据表格)、ProgressBar(进度条)、Form(表单)、SearchBox(搜索框)、textbox(文本框)、FileBox(文件框)、comboBox(组合框)、NumberBox(号码框)、Messager(消息)


## easyui常用组件属性及方法？
Easyui tab判断选项卡是否已经存在exists          
Easyui 自动适应屏幕 fit:true          
Easyui datagrid 只能选择一行singleSelect：true          
Easyui tree 绑定点击事件 onClick:function(node){}          
Easyui datagrid 绑定点击事件 单击：onClickRow 双击：onDblClickRow          
Easyui tabs 动态添加选项卡$('#myTabs').tabs('add',{title: 标题,content:内容, closable:true//是否可关闭})          
Easyui dialog 动态打开弹框 $(‘#myDialog’).dialog(‘open’)          
Easyui 打开消息框 $.messager.alert('提示','请选择需要修改的数据','warning');          
Easyui 表单重置、提交 $('#addForm').form('reset'); $('#addForm').form(‘submit’);


## 分页步骤？
1. 前台封装一个显示分页的组件
2. 查询总条数
3. 后台封装分页工具类，计算开始位置、结束位置、总页数
4. 后台写支持分页的sql语句
5. 前台包含分页组件，实现分页效果          
   注意:查询总条数的where和查询列表信息的where条件要保证一致。


## EasyUI如何阻止tabs重复打开页签问题？说一下解决思路？
这里要用到Tabs组件的exists方法，有参数为'which'，表明指定的面板是否存在，'which'参数可以是选项卡面板的标题或索引。返回类型为Boolean,如果返回的值为true，则表名选项卡已经存在，则调用select方法，切换选项卡，如果返回false，则表名选项卡不存在，则调用Tabs组件的add方法，创建选项卡。
