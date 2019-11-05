---
title: Ajax提交表单数据
date: 2017-08-26 21:30:28
tags:
categor: 
- html
desc: Ajax提交表单数据
keyword: Ajax提交表单数据
from: 
---

> serialize() :序列化表单数据，获取到的是一个对象，然后通过对象访问到里面的值。
>
> $.ajax( url , date , function(){}); 数据提交地址，xml数据，回调函数（接受返回值）

### 简单的数据提交

- form表单

  ```
  <form>
  	name : <input type=""type" name="title">
  	name : <input type=""type" name="pw">
  	<input type="submit" value="提交">
  </form>
  ```

- ajax提交数据

  ```
  $("submit").click(function(){
    var date = $("form").serializeArray();//获取到表单里面所有的对象
    postDate = {};
    $(date).each(function(i){
      postDate[this.name] = this.value;//循环赋值，转化为json数据
  });
  //可以使用 console.log(postDate) 查看提交的数据
    $.post(url , postDate , function(result){
        if(result.status == 1){
          alert("ok");
        }else if(result.status == 0){
          alert("false");
        }
      }
    },"JSON");
  ```

### FormData使用<!--more-->

- form

  ```
  <form id="myform" enctype="multipart/form-data">
      <input type="text" name="text1">
      <input type="text" name="text2">
      <input type="file" name="file">
      <input id="submit" type="button" name="提交" value="提交">
  </form>
  ```

  ```
  <script>
      $('#submit').on('click',function () {
          var formdata =new FormData($('#myform')[0]);//构造FormData
          $.ajax({
              url:'/test/index/test',
              data:formdata,
              type:'post',
              contentType:false, //使用正确的contentType类型 （必须
              processData:false,//data选项既可以包含一个查询字符串，（必须）比如 key1=value1&amp;key2=value2 ，也可以是一个映射，比如 {key1: 'value1', key2: 'value2'} 。如果使用了后者的形式，则数据再发送器会被转换成查询字符串。这个处理过程也可以通过设置processData选项为false来回避
              success:function (msg) {
                  alert(msg);
              }
          });
      });
  </script>
  ```

- 后台打印

  ```
  var_dump($_POST);
  var_dump($_FIFLES);
  ```

### FormData上传文件实例

- form

  ```
  <form id="form1">
      <input type="file" name="fileUpload" multiple="true">
      <!-- 这里不能使用button标签，不然你懂得 -->
      <input type="button" onclick="test();" value="提交">
  </form>
  ```

  ```
  <script type="text/javascript">
      function test(){
          var form = new FormData(document.getElementById("form1"));

          $.ajax({
              url:"/test/index/test",
              type:"post",
              data:form,
              processData:false,
              contentType:false,
              success:function(data){
                  alert(data);
  //              alert("pass");
              },
              error:function(e){
                  alert("错误！！");
              }
          });
      }
  </script>
  ```

- 后台打印

  ```
  var_dump($_POST);
  var_dump($_FIFLES);
  ```

###  