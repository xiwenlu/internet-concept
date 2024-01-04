# jquery


## 你为什么要使用jquery？ {id="jquery_2"}
query是一个js框架，拥有跨浏览器的特性，可以兼容各种浏览器，其宗旨是———“Write Less, Do More”,写更少的代码,做更多的事情。 它是一个快速和简洁的JavaScript 库，可以简化HTML 文档元素的遍历，事件处理，动画和Ajax 交互，各种使用性插件以实现快速Web 开发，它被设计用来改变编写JavaScript 脚本的方式。使用ajax可以实现html与js的脚本分离。

## 你知道jquery中的选择器吗，请讲一下有哪些选择器？ {id="jquery_3"}
1. id选择器 $("#+id名") 相当于 document.getElementById();
2. 标签选择器 $("标签名") 相当于 document.getElementsByTagName();
3. 类选择器 $(".+类名")
4. 通用选择器 $("*")

``选择器的扩展使用：``      
属性选择器 $("标签名[属性名='值']") document.getElementsByName();      
组合选择器 +(并列关系) >(包含关系 )


## jquery常用的方法
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
    <tr>
        <td>.first()</td>
        <td>取得数组里面第一个对象</td>
    </tr>
    <tr>
        <td>.last()</td>
        <td>取得数组里面第最后一个对象</td>
    </tr>
    <tr>
        <td>.prev()</td>
        <td>取得当前元素的前一个元素</td>
    </tr>
    <tr>
        <td>.next()</td>
        <td>取得当前元素的下一个元素</td>
    </tr>
    <tr>
        <td>.eq()</td>
        <td>获得数组下标的元素</td>
    </tr>
    <tr>
        <td>.attr()</td>
        <td>设置或返回被选元素的属性值</td>
    </tr>
    <tr>
        <td>.removeAttr()</td>
        <td>从每一个匹配的元素中删除一个属性</td>
    </tr>
    <tr>
        <td>.css()</td>
        <td>设置元素的样式属性</td>
    </tr>
    <tr>
        <td>.hide()</td>
        <td>隐藏显示的元素</td>
    </tr>
    <tr>
        <td>.show()</td>
        <td>显示隐藏的元素</td>
    </tr>
    <tr>
        <td>.text()</td>
        <td>获得文本内容 &nbsp;相当于js中的innerTEXT</td>
    </tr>
    <tr>
        <td>.html()</td>
        <td>获得格式化的文本内容相当于js中的innerHTML</td>
    </tr>
    <tr>
        <td>.val()</td>
        <td>获得对应的values的值</td>
    </tr>
    <tr>
        <td>.prop()</td>
        <td>获取在匹配的元素集中的第一个元素的属性值</td>
    </tr>
    </tbody>
</table>


## jquery常用的事件 {id="jquery_1"}
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
   <tr>
        <td>
            事件
        </td>
        <td>
            描述
        </td>
    </tr>
    <tr>
        <td>
            $(function(){})
        </td>
        <td>
            页面加载事件
        </td>
    </tr>
    <tr>
        <td>
            click()
        </td>
        <td>
            点击，单击事件
        </td>
    </tr>
    <tr>
        <td>
            blur()
        </td>
        <td>
            失去焦点事件
        </td>
    </tr>
    <tr>
        <td>
            change()
        </td>
        <td>
            内容改变事件
        </td>
    </tr>
    <tr>
        <td>
            dblclick()
        </td>
        <td>
            双击事件
        </td>
    </tr>
    <tr>
        <td>
            focus()
        </td>
        <td>
            获取焦点事件
        </td>
    </tr>
    </tbody>
</table>


## 请谈一下你对Ajax的认识 {id="ajax_2"}
AJAX 全称： 异步JavaScript及 XML（Asynchronous JavaScript And XML）          
Ajax的核心是JavaScript对象XmlHttpRequest(XHR)。          
``Ajax的优点的优点: ``
1. 提高用户体验度(UE)
2. 提高应用程序的性能
3. 进行局部刷新

1. AJAX不是一种新的编程语言，而是一种用于创建更好更快以及交互性更强的 Web 应用程序的技术。
2. 通过 AJAX，我们的 JavaScript 可使用JavaScript的XMLHttpRequest对象来直接与服务器进行通信。通过这个对象，我们的JavaScript 可在不重载页面的情况与Web服务器交换数据，即可局部刷新。
3. AJAX 在浏览器与 Web 服务器之间使用异步数据传输（HTTP请求），这样就可使网页从服务器请求少量的信息，而不是整个页面，减轻服务器的负担，提升站点的性能。
4. AJAX 可使因特网应用程序更小、更快，更友好，用户体验（UE）好。
5. Ajax是基于标准化并被广泛支持的技术，并且不需要插件和下载小程序

``Ajax包含下列技术：``            
基于web标准（standards-basedpresentation）XHTML+CSS的表示；使用 DOM（Document ObjectModel）进行动态显示及交互；使用 XML 和 XSLT 进行数据交换及相关操作；使用 XMLHttpRequest 进行异步数据查询、检索；使用 JavaScript 将所有的东西绑定在一起。

## ajax 三种使用方式
1. $.ajax({})
2. $.get()
3. $.post()



## ajax 常用属性 {id="ajax_1"}
1. ``url: ``要求为String类型的参数，（默认为当前页地址）发送请求的地址。
2. ``type: ``要求为String类型的参数，请求方式（post或get）默认为get。注意其他http请求方法，例如put和delete也可以使用，但仅部分浏览器支持。
3. ``async: ``要求为Boolean类型的参数，默认设置为true，所有请求均为异步请求。如果需要发送同步请求，请将此选项设置为false。注意，同步请求将锁住浏览器，用户其他操作必须等待请求完成才可以执行。
4. ``data:``要求为Object或String类型的参数，发送到服务器的数据。
5. ``dataType: ``要求为String类型的参数，预期服务器返回的数据类型。如果不指定，JQuery将自动根据http包mime信息返回responseXML或responseText，并作为回调函数参数传递。可用的类型如下：          
   ``xml：``返回XML文档，可用JQuery处理。       
   ``html：``返回纯文本HTML信息；包含的script标签会在插入DOM时执行。       
   ``script：``返回纯文本JavaScript代码。不会自动缓存结果。除非设置了cache参数。注意在远程请求时（不在同一个域下），所有post请求都将转为get请求。       
   ``json：``返回JSON数据。       
   ``jsonp：``JSONP格式。使用SONP形式调用函数时，例如myurl?callback=?，JQuery将自动替换后一个“?”为正确的函数名，以执行回调函数。       
   ``text：``返回纯文本字符串。
6. success：要求为Function类型的参数，请求成功后调用的回调函数，有两个参数。
(1)由服务器返回，并根据dataType参数进行处理后的数据。      
(2)描述状态的字符串。

```
function(data, textStatus){
    //data可能是xmlDoc、jsonObj、html、text等等
    this; //调用本次ajax请求时传递的options参数
}
```

7. error:       
   要求为Function类型的参数，请求失败时被调用的函数。该函数有3个参数，即XMLHttpRequest对象、错误信息、捕获的错误对象(可选)。ajax事件函数如下：

```
function(XMLHttpRequest, textStatus, errorThrown){
    //通常情况下textStatus和errorThrown只有其中一个包含信息
    this; //调用本次ajax请求时传递的options参数
}
```

8. JQuery 总结          
   由于 jQuery 是为处理 HTML事件而特别设计的，那么当您遵循以下原则时，您的代码会更恰当且更易维护：          
   n把所有 jQuery 代码置于事件处理函数中          
   n把所有事件处理函数置于文档就绪事件处理器中          
   n把 jQuery 代码置于单独的 .js 文件中          
   n如果存在名称冲突，则重命名 jQuery 库          
   jQuery 名称冲突 jQuery 名称冲突          
   jQuery 使用 $ 符号作为 jQuery 的简介方式。          
   某些其他 JavaScript 库中的函数（比如 Prototype）同样使用 $ 符号。          
   jQuery 使用名为 noConflict() 的方法来解决该问题。          
   var jq=jQuery.noConflict()，帮助您使用自己的名称（比如 jq）来代替 $ 符号。

9. Json          
   Json（JavaScript Object Notation）特点：
json分为两种格式:json对象(就是在{}中存储键值对，键和值之间用冒号分隔，键值对之间用逗号分隔);          
json数组(就是[]中存储多个json对象，json对象之间用逗号分隔)          
（两者间可以进行相互嵌套）数据传输的载体之一          
``对象转json：``          
页面的ajax，dataType:"json", 规定返回值是 json对象          
Java中调用String jsonStr = JSON.toJSONString(对象);获得 json字符串

## jQuery与js的联系与区别？
js是脚本语言，JQuery是建立在这个语言上的一个基本库，利用JQuery可以更简单的使用js。Js直接支持


## $(this) 和 this 关键字在 jQuery 中有何不同？
$(this) 返回一个 jQuery 对象，你可以对它调用多个 jQuery 方法，比如用 text() 获取文本，用val() 获取值等等。          
而 this 代表当前元素，它是 JavaScript 关键词中的一个，表示上下文中的当前 DOM 元素。你不能对它调用 jQuery 方法，直到它被 $() 函数包裹，例如 $(this)。


## jQuery 里的 each() 是什么函数？你是如何使用它的？
each() 函数就像是 Java 里的一个 Iterator，它允许你遍历一个元素集合。你可以传一个函数给 each() 方法，被调用的 jQuery 对象会在其每个元素上执行传入的函数。有时这个问题会紧接着上面一个问题，举个例子，如何在 alert 框里显示所有选中项。我们可以用上面的选择器代码找出所有选中项，然后我们在 alert 框中用 each() 方法来一个个打印它们


## JavaScript window.onload 事件和 jQuery ready 函数有何不同？
JavaScript window.onload 事件和 jQuery ready 函数之间的主要区别是，前者除了要等待 DOM 被创建还要等到包括大型图片、音频、视频在内的所有外部资源都完全加载。如果加载图片和媒体内容花费了大量时间，用户就会感受到定义在 window.onload 事件上的代码在执行时有明显的延迟。       
另一方面，jQuery ready() 函数只需对 DOM 树的等待，而无需对图像或外部资源加载的等待，从而执行起来更快。使用 jQuery $(document).ready() 的另一个优势是你可以在网页里多次使用它，浏览器会按它们在 HTML 页面里出现的顺序执行它们，相反对于 onload 技术而言，只能在单一函数里使用。鉴于这个好处，用 jQuery ready() 函数比用 JavaScript window.onload 事件要更好些。


## JavaScript、jQuery、Ajax、Json的关系?
javascript是实现ES标准的编程语言，就是很多小招数，比如扎马步啊、各种基本剑法刀法等等，现在正雄霸天下，准备一统江湖，各大门派都争相学习。        
jquery是对js一些方法的封装，是js的一个库，可以看作是各种招数的集合组成了各种大招，比如上来就是辟邪剑法，一招搞定DOM操作，比js提供的小招数好用多了，现在全球绝大数网站都使用了jquery库。          
json是一种数据格式，数据可以看作物品比如苹果，json就是装载物品的一种方式，有了json这种内力，你再使用隔空取物ajax这招，你就可以轻而易举拿到很多苹果，而且排列有序，不仅是苹果，还可以同时拿梨子香蕉等等，想拿什么就拿什么。            
ajax是js其中一个招数，jquery库也对其进行了封装，使之使用更加简单，ajax是异步传输数据，可以看作是隔空取物，一招隔空取物过去就能拿到一些苹果，但是很麻烦很费力费时。
## 使用jQuery能做什么？ {id="jquery_4"}
1. 方便快捷获取DOM元素
2. 动态修改页面样式
3. 动态改变DOM内容
4. 响应用户的交互操作
5. 为页面添加动态效果
6. 统一Ajax操作
7. 简化常见的JavaScript任务。


## 请介绍一下XMLhttprequest对象。
Ajax的核心是JavaScript对象XmlHttpRequest。该对象在Internet Explorer 5中首次引入，它是一种支持异步请求的技术。简而言之，XmlHttpRequest使您可以使用JavaScript向服务器提出请求并处理响应，而不阻塞用户。通过XMLHttpRequest对象，Web开发人员可以在页面加载以后进行页面的局部更新。

