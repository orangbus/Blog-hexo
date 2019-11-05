---
title: 一、tp5开发流程及常用函数
date: 2017-09-06 18:06:43
tags: thinkphp
categories:
- php
desc: tp5开发流程及常用函数
---



[TOC]

#### 1、模板样式文件引入、分离、继承

- 样式文件

  ```
  1、通过配置文件加载
  return[
    'view_replace_str' =>[
      'INDEX_CSS' => "/statc/index.css",
    ],
  ]
  ----------------------------------------------
  2、使用系统方法
  {load href="/static/css(js)/index.css" /}
  {css href="/stativ/css/index.css" /}
  {js href="/static/js/index.js" /}
  ```

- 模板分离

  ①把公共部分的代码提取出来，放在 view/public里面，然后使用{ include file='public/xxx' } 引入即可

  ②使用 `{ block name='des' } 相同功能或相关联的代码 {/block} ` 模块化管理

  ​	我们将公共代码进行模块化作为父模板（view/public），然后通过模板继承使用。**变化的部分子模板会替换父模板**<!--more-->

  ```
  view/public
  -----------------------------------
  {block name='seo'} 
  	{inlucde file="public/header"}
  {/block}
  {block name='seo'}
  	{$title|default="标题"}
  {/block}
  ```

- 模板继承（继承父类的模板）

  ```
  {extends name="public/base"}
  //继承后即可删除原来include导入的文件和
  ```

#### 2、管理员登陆

验证码处理

```
<a href="javascript:captcha_refresh();"></a>
//自动刷新验证码
function captcha_refresh(){
  var str = Date.parse(new Date())/1000;
  $("code_image").attr("src" , "/captcha?id="+str);
}
```

Ajax提交表单数据

```
$(function(){
  $("button").on('click' , function(){
    $.ajax({
      type:"post",
      url:"",
      data: $("form").seriaze(),//序列化表单
      dataType: 'json',
      success: function(){
        //返回信息处理
      }
    });
  });
})
```

#### 3、退出登陆

- 登陆验证通过时设置 `session`

  ```
  Session::set('user_id' , $user->id);//用户的id
  Session::set('user_info' , $user->getData());//用户的所有信息
  ```


- 注销session

  ```
  Session::delete("user_id");
  Session.delete("user_info");
  $this->success("注销成功，返回登陆页面！" ，'login');
  ```


- 获取session值

  ```
  {:session('user_info.name')}
  ```

#### 4、防止用户重复登陆

- 在controller公共文件中定义一个初始化函数

  ```
  use think\Session;

  protected function _initialize(){
    parent::initialize(); //继承父类中初始化操作
    define('USER_ID' , Session::has('user_id')?Session::get('user_id):null);
  }
  //判断用户是否登陆，放在入口文件:index/index , 直接调用 $this->isLogin() 方法即可
  protected function isLogin(){
    if(is_null(USER_ID)){
      $this -> error("你尚未登陆，无权访问" , url("user/login"));
    }
  }
  //防止用户重复登陆，放在：index/login , 直接调用 $this -> alreadyLogin() 方法
  protected function alreadyLogin(){
  if(USER_ID){
    $this -> error("你已登陆，请勿重新登陆" , url("index/index"));
  } 
  }
  ```

#### 5、请求参数

```
$Request.方法名.参数
-----------------------

{$Request.get.id}// 调用Request对象的get方法 传入参数为id

{$Request.param.name}// 调用Request对象的param方法 传入参数为name

{$Request.param.user.nickname}// 调用Request对象的param方法 传入参数为user.nickname

{$Request.root}// 调用Request对象的root方法

{$Request.root.true}// 调用Request对象的root方法，并且传入参数true

{$Request.path}// 调用Request对象的path方法

{$Request.module}// 调用Request对象的module方法

{$Request.controller}// 调用Request对象的controller方法

{$Request.action}// 调用Request对象的action方法

{$Request.ext}// 调用Request对象的ext方法

{$Request.host}// 调用Request对象的host方法

{$Request.ip}// 调用Request对象的ip方法

{$Request.header.accept-encoding}// 调用Request对象的header方法

{$Request.session.user_info.login_count}//统计登陆次数
```

**系统变量：**系统变量的输出通常以**{$Think** 打头，例如：

```
{$Think.server.script_name} // 输出$_SERVER['SCRIPT_NAME']变量
{$Think.session.user_id} // 输出$_SESSION['user_id']变量
{$Think.get.pageNumber} // 输出$_GET['pageNumber']变量
{$Think.cookie.name}  // 输出$_COOKIE['name']变量
{$Think.PHP_VARSION}
{$Think.PHP_OS}
{:session_id()}
{:count($_SESSION)}
{:date("Y-m-d H:i:s") , $Think.session.user_info.login_time}

------------常量输出------------------------
{$Think.APP_PATH}

----------------配置输出----------------------
{$Think.config.default_module}
{$Think.config.default_controller}
```

#### 6、管理员角色管理

- ##### 添加管理员，并实时进行ajax验证

- ##### 编辑管理员

- ##### 删除管理员

- ##### 软删除（时间戳）

  ##### 通过记录`is_Delete` 字段实现的

  ```
  use traits\model\SorftDelete;
  use SoftDelete;

  protected $dateFormt = 'Y-m-d'; //设置当前默认日期现实格式
  protected $deleteTime = 'tableName(delete_time)'; //记录删除时间,如果delete_time的值为 null则前端显示，不为null则不显示

  protected $autoWriteTimestamp = true;//开启时间戳
  protected $createTime = 'tableName(creat_time)';
  protected $updateTime = 'update_time';
  ```

  ##### 软删除操作

  ```
  public function delete(Request $request){
    $id = $request -> param('id');
    Model::update(['is_delete' => 1] , ['id' => $id]);
    Model::destroy($id);
  }
  ```

  **软删除恢复操作**

  ```
  public function undelete(){
    //(更新值 ， 条件); 如果delete_time的值为 null ， is_delete的值更新为 1
    Model::update(['delete_time' => null] , ['is_delete' => 1]);
  }
  ```

- ##### 角色管理

#### 7、关联模型

​	自动完成  `protected $insert = ['status' => 1]`

- 一对一

  ```
  public function teacher(){ //当前班级表关联教师表
    return $this -> hasOne('tableName(Teacher)');
  }
  ```

- 一对多

  ```
  public function student(){ //当前班级表关联学生表
    return $this -> hasMany('Student');
  }
  ```

- 反关联 ( bolongsto ) 

  ```
  public function grade(){
    return $this -> belongsto('Grade');
  }
  ```

如何调用

```
$userName = Model::all(); //获取表单所有数据
$count = TeacherModel::count(); //统计条数

foreach ($userName as $value){
  $data = [
    'id' => $value -> id,
    'name' => $value->name,
     ..........
    'otherTableName' => ($value->otherTableName -> name); //连表查询
  ];
  $userNameList[] = $data;
}
```



[TOC]

### 二、用户管理

- 连表查询

- 更新字段

  ```
  $data = $request -> except('otherTableName');//提出不是本数据表的字段
  ```

- ​

