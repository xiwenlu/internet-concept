# javascript代码

## ajax代码
```javascript

JSON.parse("",function(k,y){return});  //json字符串转js对象

$.ajax({ 
    url : "jsonPost",  
    type : "post",  
    //contentType :"application/x-www-form-urlencoded; charset=UTF-8",//form表单提交解决乱码
    //contentType:"application/json;charset=UTF-8",//json字符串提交解决乱码
    data : {
        "name" :  $("#name").val(),
        "password" : $("#password").val()
    },
    //data : $("#form").serialize();
    //data : JSON.stringify(Object);  //js对象转json对象
    async:false, // ajax 改为同步请求会导致阻塞；ajax 需要 进行异步请求 			 
    processData : false, //当设置为true的时候,jquery ajax 提交的时候不会序列化 data，而是直接使用data 
    dataType : "json", 	
    success : function(msg) {  
        //javascript已自动将返回的json数据转为对象了  
        alert("success:"+msg.name+"---"+msg.password);  
    },  
    error : function() {  
        alert("try again!");  
    }  
});
```