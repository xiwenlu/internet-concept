# bootstrap

## bootstrap常用组件？
bootstrap-treeview（树）、bootStrap-addTabs（选项卡）、bootstrap-table（表格）、bootstrap-datetimepicker（时间）、bootstrap-bootbox（弹框）、bootstrap-fileinput（上传）
## bootstrap常用css样式？
``布局容器``
.container 类用于固定宽度并支持响应式布局的容器。          
.container-fluid 类用于 100% 宽度，占据全部视口（viewport）的容器。

``表格``
.table          
.table-striped类可以给<tbody>之内的每一行增加斑马条纹样式。          
.table-bordered类为表格和其中的每个单元格增加边框。          
.table-hover类可以让<tbody>中的每一行对鼠标悬停状态作出响应。          
.table-condensed类可以让表格更加紧凑，单元格中的内补（padding）均会减半。

将任何.table元素包裹在.table-responsive元素内，即可创建响应式表格，其会在小屏幕设备上（小于768px）水平滚动。当屏幕大于 768px 宽度时，水平滚动条消失。          
``状态类``
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
    <tr>
        <td>Class</td>
        <td>描述</td>
    </tr>
    <tr>
        <td>.active</td>
        <td>鼠标悬停在行或单元格上时所设置的颜色</td>
    </tr>
    <tr>
        <td>.success</td>
        <td>标识成功或积极的动作</td>
    </tr>
    <tr>
        <td>.info</td>
        <td>标识普通的提示信息或动作</td>
    </tr>
    <tr>
        <td>.warning</td>
        <td>标识警告或需要用户注意</td>
    </tr>
    <tr>
        <td>.danger</td>
        <td>标识危险或潜在的带来负面影响的动作</td>
    </tr>
    </tbody>
</table>

``表单``                       
.form-group 表单组            
.form-inline 内联表单            
.form-inline类可使其内容左对齐并且表现为inline-block级别的控件。只适用于视口（viewport）至少在 768px 宽度时（视口宽度再小的话就会使表单折叠）。            
``水平排列表单``            
.form-horizontal类，并联合使用 Bootstrap             预置的栅格类，可以将label标签和控件组水平并排布局。这样做将改变.form-group的行为，使其表现为栅格系统中的行（row），因此就无需再额外添加.row了。            
.btn 按钮            
.btn-default 默认样式            
.btn-primary 主题样式（深蓝）            
.btn-success 成功样式（绿）            
.btn-info 一般信息样式（淡蓝）            
.btn-warning 警告样式（黄）            
.btn-danger 危险样式（红）            
.btn-lg 大按钮            
.btn-sm 小按钮            
.btn-xs 超小按钮            
.img-rounded 图片样式方形圆角            
.img-circle 图片样式圆形            
.img-thumbnail 图片样式方形圆角边框            
``栅格系统``
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
    <tr>
        <td>
            &nbsp;
        </td>
        <td>
            超小屏幕&nbsp;手机(&lt;768px)
        </td>
        <td>
            小屏幕&nbsp;平板(≥768px)
        </td>
        <td>
            中等屏幕&nbsp;桌面显示器(≥992px)
        </td>
        <td>
            大屏幕&nbsp;大桌面显示器(≥1200px)
        </td>
    </tr>
    <tr>
        <td>
            栅格系统行为
        </td>
        <td>
            总是水平排列
        </td>
        <td colspan="3">
            开始是堆叠在一起的，当大于这些阈值时将变为水平排列C
        </td>
    </tr>
    <tr>
        <td>
            .container 最大宽度
        </td>
        <td>
            None（自动）
        </td>
        <td>
            750px
        </td>
        <td>
            970px
        </td>
        <td>
            1170px
        </td>
    </tr>
    <tr>
        <td>
            类前缀
        </td>
        <td>
            .col-xs-
        </td>
        <td>
            .col-sm-
        </td>
        <td>
            .col-md-
        </td>
        <td>
            .col-lg-
        </td>
    </tr>
    <tr>
        <td>
            列（column）数
        </td>
        <td colspan="4">
            12
        </td>
    </tr>
    <tr>
        <td>
            最大列（column）宽
        </td>
        <td>
            自动
        </td>
        <td>
            ~62px
        </td>
        <td>
            ~81px
        </td>
        <td>
            ~97px
        </td>
    </tr>
    <tr>
        <td>
            槽（gutter）宽
        </td>
        <td colspan="4">
            30px（每列左右均有15px）
        </td>
    </tr>
    <tr>
        <td>
            可嵌套
        </td>
        <td colspan="4">
            是
        </td>
    </tr>
    <tr>
        <td>
            偏移（Offsets）
        </td>
        <td colspan="4">
            是
        </td>
    </tr>
    <tr>
        <td>
            列排序
        </td>
        <td colspan="4">
            是
        </td>
    </tr>
    </tbody>
</table>

