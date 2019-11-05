---
title: 二、tp5_后台登陆
date: 2017-09-06 18:30:25
tags: thinkphp
categories:
- php
desc: tp5_后台登陆
---

- Ajax提交表单数据（或者`$.post(url , date , function(){})`）

  ```
  $(function(){
    $("button").on("click" , function(){
      $.ajax({
        type: "post",
        url: "url",
        data: $("form").seriaze();//序列化表单
        dataType: 'json', //数据类型
        success: function(){ //成功回调函数
          //doing something
        }
      });
    });
  });
  ```

- $.post( url , date , function() { })方式提交表单数据<!--more-->

  ```
  $("button"),click(function () {
          var data = $("form").serializeArray();
          postData = {};
          var url = "";
          $.post(url , postData ,function (res) {
              //对后台返回的数据进行处理
          });
      });
  ```

#### PHP后台处理

```
//登陆验证
    public function checklogin(Request $request){
        $status = 0;
        $result = "";
        $data = $request -> param();
        //验证规则
        $rule = [
            'name|用户名' => 'require',
            'passwd|密码' => 'require',
        ];
//        $msg = [
//            'name'=> ['require'=>"用户名不能为空"],
//            'passwd'=>['require'=>"密码不能为空"],
//        ];
        //validate($data , $rule,$msg);
        $result = $this-> validate($data , $rule);
        //数据库进行校验
        if($result === true){
            $map = [
                'name' => $data['name'],
                'passwd' => md5($data['passwd']),
            ];
            $user = adminModel::get($map);
            if ($user == null){
                $result = "该用户不存在,或者密码不正确";
            }else{
                $status = 1;
                $result = "登陆成功";
                //设置用户的session信息
                Session::set('user_id',$user->id);
                Session::set('user_info',$user -> getData());//获取用户所有信息
            }
        }
        return ["status"=>$status , "result"=>$result];
    }
    
```

#### Ps：自己封装的一个简单的layer文件，可直接复制使用

> 到 layer 下载官方的css & js ， 并引入到项目文件中。
>
> 新建一个 default.js 文件，复制以下代码到其中，并引入到项目文件中就可以使用了。
>
> 调用方式：`dialog.error("错误信息")`

```
var dialog = {
    // 错误弹出层
    error: function(message) {
        layer.open({
            content:message,
            icon:2,
            title : '错误提示',
        });
    },

    //成功弹出层，并跳转url页面
    success : function(message,url) {
        layer.open({
            content : message,
            icon : 1,
            yes : function(){
                location.href=url;
            },
        });
    },

    // 确认弹出层
    confirm : function(message, url) {
        layer.open({
            content : message,
            icon:3,
            btn : ['是','否'],
            yes : function(){
                location.href=url;
            },
        });
    },

    //无需跳转到指定页面的确认弹出层
    toconfirm : function(message) {
        layer.open({
            content : message,
            icon:3,
            btn : ['确定'],
        });
    },
}
```
欢迎加入**[云南php&layui交流群](https://jq.qq.com/?_wv=1027&k=5FT8Mh5)**：511300462