---
title: Jquery&Ajax学习笔记
date: 2017-08-21 13:55:57
tags:
categories: 
- html
from: http://www.orange.net.oschina.io/categories/html/
desc: Jquery&Ajax学习笔记 , ajax. html
keyword: jquery学习笔记，ajax学习笔记，html
---

# jquery学习笔记

[TOC]

## **1、引入问题**

​	js外部文件引入最好使用双标签，不然会出现引入了但是没有效果的现象

```
    <script type="text/javascript" src="js/jquery2.0.0.min.js" ></script> //推荐方式
    <script type="text/javascript" src="js/jquery2.0.0.min.js" /> //引入没反应，不推荐
```

## **2、Ajax请求与返回**

​	**$(selector).load(URL,data,callback);**

​	**必需的 *URL* 参数规定您希望加载的 URL。**

​	**可选的 *data* 参数规定与请求一同发送的查询字符串键/值对集合。**

​	**可选的 *callback(response , stutas)* 参数是 load() 方法完成后所执行的函数名称.**

- **get请求**

```
 function demo() {
            $('#box').load("test.php?name=orange");
        }
```

test.php<!--more-->

```
if ($_GET['name'] == 'orange'){
    echo "请求成功";
    }
```

- **post请求**

```
 function demo() {
            $('#box').load("test.php",{
              name:'orange';  //这里有一个引号
            });
        }
```

test.php

```
if ($_POST['name'] == 'orange'){
	echo "请求成功";
	}
```

- **callback**

```
 function demo() {
            $('#box').load("test.php",{
              name:'orange';  //这里有一个引号
            },function(){
              $('p').html(response + "new content"); //把返回的结果传递给其它地方
            });
        }
```

## **3、.post / .get方法**

- **.get(url , date , callback , type)方法**

```
 $('#demo').click(function () {
            //1、直接传值
            $.get('test.php?name=orange' , function (response , stutas , xhr) {
                $('#box').html(response);
            })
            //2、拼接字符串
            $.get('test.php?name=orange' ,"name=orange", function (response , stutas , xhr) {
                $('#box').html(response);
            })
            //3、键值对的方式
            $.get('test.php?name=orange' ,{
               name:"orange"
            } ,function (response , stutas , xhr) {
                $('#box').html(response);
            })
        });
```

- **.post(url , date , callback , type)方法**

```
$('#demo').click(function () {
            //1、拼接字符串
            $.post('test.php?name=orange' ,"name=orange", function (response , stutas , xhr) {
                $('#box').html(response);
            });
            //2、键值对的方式
            $.post('test.php' ,{
               name:"orange"
            } ,function (response , stutas , xhr) {
                $('#box').html(response);
            })
        });
```

## 4、Ajax数据处理技术

```
$(function () {
        $('#demo').click(function () {
            $.ajax({
                type:'POST',//提交的方式
                url:'test.php', //请求的地址
                data:{//请求的数据
                    name:'orange'
                },
                success:function (response) {//返回值
                    $('#box').html(response);
                }
            })
        });

    });
```

test.php

```
if ($_POST['name'] == 'orange'){
	echo "请求成功";
	}
```

**一个表单实例**

```
<form>
    <input type="text" name="name">
    <input type="submit" value="add" id="add">
</form>
```

```
$('#add').click(function () {
            $.ajax({
                type:'POST',//提交的方式
                url:'test.php', //请求的地址
                data:$('form input[name]').serialize(),//使用了序列化
                
                //最简单的方式
//                data:{//请求的数据
//                    name:$('form input[name=name]').val()
//                },

                success:function (response) {//返回值
                    $('#box').html(response);
                }
            })
        });
```

```
<?php
//test.php
	echo $_POST['name'];
?>
```

## **5、加载提示与错误处理**

```
<form>
    <input type="text" name="name">
    <input type="submit" value="add" id="add">
    <span id="londing" style="display: none">数据提交中....</span>
</form>
```

```
 $('#add').click(function () {
            $.ajax({
                type:'POST',//提交的方式
                url:'test.php', //请求的地址
                data:$('form input[name]').serialize(),
                success:function (response) {//返回值
                    $('#box').html(response);
                },
                timeout:1000,//设置请求时间
                error:function (xhr) {//错误处理
                    alert(xhr.status + ":" + xhr.statusText)
                }
            })
        })
        //这是一个全局捕获错误的方法
        /*
        	$(document).ajaxError(function (evet,xhr,settings,info) {
            alert() //弹出相应的参数看看
        })
        */
            $(document).ajaxStart(function () {
                $('#londing').show();
            })
            $(document).ajaxStop(function () {
                $('#londing').hide();
                
           //罗列几个全局变量
//         $(document)
//            .ajaxSend(function(){
//                alert("发送请求之前执行");
//            })
//            .ajaxComplete(function(){
//                alert("请求完成之后执行，不论成功失败");
//            })
//            .ajaxSucces(function(){
//                alert("请求成功调用");
//            })

        });
```



# Ajax学习笔记

[TOC]

## 1、创建对象

```
var xhr = new XMLHttpRequest();
console.log(xhr); //打开firebug查看
------------------------------------
```

## 2、发起服务器请求

```
<button type="button" onclick="demo()">请求数据</button>
```

```
function demo() {
            var xml = XMLHttpRequest();
            xml.open('GET' , "test.php" , true);//创建请求方式、地址、开启异步
            xml.send();//发送请求
        }
```

## 3、接受服务器返回数据

Aja小对象的成员

属性：responseTest:义字符串的形式接受服务器的信息

​	   readyState：

​		0：创建对象

​		1：已经调用open方法

​		2：已经调用send方法

​		3：已经返回部分数据

​		4：请求完成，数据完全返回

onreadystatechange:当Ajax状态readystate发生变化时出发，最好在创建Ajax对象后设置

```
 <script type="text/javascript">
        //想服务器发送请求
        function demo() {
            var xhr =new XMLHttpRequest();//①创建对象
            xhr.onreadystatechange = function () {
				//console.log(xhr.readyState)调试输出
                //④获取请求的状态，一共有 4 中状态
                if (xhr.readyState == 4){
                  //console.log(xhr.response);
                    document.getElementById('result').innerHTML = xhr.responseText;
                }
            }
            xhr.open('GET' , "/test.php" , true);//②创建请求方式、地址
            xhr.send();//③发送请求
        }
    </script>
```

response

```
echo "<div style='color: #1E9FFF;'> html </div>"
```

## 4、POST | GET请求

特殊信息的处理

- php可以使用函数 urlencode() / urldecode()
- javascript通过encodeURLComponent()
  - [ ] **GET请求**

```
<input type="text" id="username" onblur="checkname()">
<button type="button" onclick="demo()">请求数据</button>
```

```
function checkname() {
            //获取用户名信息
            var nm = document.getElementById('username').value;

            nm = encodeURI(nm);//特殊符号（& = ）和中文进行编码处理

            var xhr =new XMLHttpRequest();//①创建对象
            xhr.onreadystatechange = function () {
//                console.log(xhr.readyState)调试输出
                //④获取请求的状态，一共有 4 中状态
                if (xhr.readyState == 4){
//                    console.log(xhr.response);
//                    document.getElementById('result').innerHTML = xhr.responseText;
                    alert(xhr.responseText);
                }
            };
            xhr.open('GET' , "/test.php?name=" +nm , true);//②创建请求方式、地址
            xhr.send();//③发送请求
        }
```

```
//接受get传来的信息
print_r($_GET);
```

- [ ] **POST请求**

```
<input type="text" id="username" onblur="checkname()">
<button type="button" onclick="demo()">请求数据</button>
```

```
 function checkname() {
            //获取用户名信息
            var nm = document.getElementById('username').value;
            nm = encodeURI(nm);//特殊符号（& = ）和中文进行编码处理
            var info = "name=" + nm;
//          alert(info);

            var xhr =new XMLHttpRequest();//①创建对象
            xhr.onreadystatechange = function () {
//                console.log(xhr.readyState)调试输出
                //④获取请求的状态，一共有 4 中状态
                if (xhr.readyState == 4){
//                    console.log(xhr.response);
//                    document.getElementById('result').innerHTML = xhr.responseText;
                    alert(xhr.responseText);
                }
            };
            xhr.open('POST' , "/test.php" , true);//②创建请求方式、地址
                //setRequestheader()方法必须在open()方法后调用
//            xhr.setRequestHeader("content-type" , "application/x-www-form-urlendcoded")
            xhr.setRequestHeader("content-type", "Ajax");
            xhr.send(info);//③发送请求
        }
```

```
//接受xhr.open 传来的 POST信息
print_r($_POST);
```

## 5、Ajax对XMl信息处理

请求文件

```
<?xml version="1.0" encoding="UTF-8"?>
<weather>
    <city>
        <name>昆明</name>
        <temp>开大</temp>
        <wind>2017.0.1</wind>
    </city>
    <city>
        <name>昆明2</name>
        <temp>开大</temp>
        <wind>2017.0.1</wind>
    </city>
    <city>
        <name>昆明3</name>
        <temp>开大</temp>
        <wind>2017.0.1</wind>
    </city>
</weather>
```

客户端页面

```
<h2>Ajax and javascript处理xml 信息</h2>
<input type="submit" onclick="ft()" value="请求">
<div id="result"></div>
```

```
function ft() {
            var xhr = new XMLHttpRequest();
            xhr.onreadystatechange = function () {
                if (xhr.readyState == 4){
                    var xmldom = xhr.responseXML;
                    var citys = xmldom.getElementsByTagName('city');
                    var s= '';
                    for(var i = 0;i<citys.length;i++){
                        var nm = citys[i].getElementsByTagName('name')[0].firstChild.nodeValue;
                        var temp = citys[i].getElementsByTagName('temp')[0].firstChild.nodeValue;
                        var wind = citys[i].getElementsByTagName('wind')[0].firstChild.nodeValue;
                        s+='城市：'+nm+"---tile:"+temp+"---time:"+wind+"<br/>";
                    }
                    document.getElementById('result').innerHTML=s;
                }
            }
            xhr.open('get' , './xml.xml' )
            xhr.send(null);
        }
```

## 6、Ajax对缓存的处理

- 添加一个随机数【**推荐**】：xhr.open('get' , 'xxx.php?' +Math.random());

- 设置一个head头：head("Cache-Control:no-cache");

    				head("Pragma:no-cache");

    ​				head("Expires:-1");

## 7、Ajax对Json文件处理 & javascript 与 json

**JSON.parse(text[],callback)** ： 将JSON数据转化为javascript对象

```
Myobj = JSON.parse(this.responseTxt)
document.getElementById("demo").innerHTML = myObj.name;
```

实例

```

```

**JSON.stringify(value[ ] )** : 将JavaScript对象转化为JSON字符串

```
<script>
	var arr = ['google' , 'taobao' , 'baidu'];
	var myjson = JSON.stringif(arr);
	document.getElementById('demo').innerHTML = myJSON;
	//["Google","Runoob","Taobao","Facebook"]
</script>
```

**eval(string)**  : 参数是一个字符串，如果字符串是一个表达式，eval()会对表达式进行求值；

必须把文本包围在括号中

```
var obj = eval("(" + txt + ")"); //obj就是JSON数据，正常的访问JSON里面的数据就可以
```

## 8、PHP处理JSON数据

- **php数组转化为JSON数据(json_encode())**

```
<?php
$arr = array("name"=>"orange" , "age"=>20 , "job"=>"PHP工程师");
echo json_encode($arr);
//{"name":"orange","age":20,"job":"PHP"}
```

**索引 => js数组**

```
$color = array('red','blue','green');
echo json_encode($color);	//["red","blue","green"]
```

```
$color = array('frist'=>'red','secend'=>'blue','three'=>'green');
echo json_encode($color);//  {"frist":"red","secend":"blue","three":"green"}
```

**关联、索引 => json对象**

```
$color = array('frist'=>'red','secend'=>'blue','three'=>'green' ,'yellow');
echo json_encode($color);//  {"frist":"red","secend":"blue","three":"green","0":"yellow"}
```

 **json_decode() 函数用于对 JSON 格式的字符串进行解码，并转换为 PHP 变量**



# 6、JSON笔记（格式：.JSON）

- 数据在名称/值对中："name" : "orange" / {"age" : 20}

- 数据由逗号分隔：{"name" : "orange" , "url" : "http://www.orange.net.oschina.io"}

- 大括号保存对象：({"name" : "orange" , "url" : "http://www.orange.net.oschina.io"})

- 中括号保存数组：

  ```
  {

  "status" :[

  	{"name" : "orange" , "web" : "JSON"}

  	{"emil" : "3228989667@qq.com" , "web" : "php"}

  ]

  }
  ```

- JSON使用javascript语法

  ```
  var site = [
    	{"name" : "orange" , "web" : "JSON"}
  	{"emil" : "3228989667@qq.com" , "web" : "php"}
  ]
  ```

我们可以这样访问：

```
site[0].name;  //orange
site["name"]
 document.getElementById('demo').innerHTML =site["name"]
```

这样修改数据

```
site[0].name = "http://www.orange.net.oschina.io"
```

