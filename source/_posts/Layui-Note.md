---
title: Layui_Note
date: 2017-07-25 23:53:31
tags: html
categories: 
- html
from: http://www.layui.com/doc/
keyword: layui学习笔记,layer
---

# Layui_Note

[TOC]

源码获取地址：http://layer.layui.com/

 	使用的时候只需要引入源码包中的：CSS  and  JS  文件即可使用

## **1、字体图标的使用**

```
<i class="layui-icon">图标字符</i> //默认
<i class="layui-icon" style="font-size:30px; color:#ffff">图标字符</i> //自定义大小
```

## **2、按钮（layui-btn）**

```
claas="layui-btn" //通过class追加改变样式,主题-尺寸-圆角 可叠加
class="layui-btn layui-btn-normal"
```

- **图标(layui-icon)**

  ```
  <button class=">
  	<i class="layui-icon>icon_name</i>Button_name
  </button>
  ```

- **按钮组 (layui-btn-group)**

  ```
  <div class="layui-btn-group">
  	<button class="">
  		<i class="layui-icon>icon_name</i>Button_name1
  	</button>
  	<button class="">
  		<i class="layui-icon>icon_name</i>Button_name2
  	</button>
  </div>
  ```

<!--more-->

## 3、表单**

```
//依赖模块
<script type="text/javascript">
	layui.use('form',function(){
	var form = layui.form();
	//监听提交
	form.on('submit(lay-filter)',function(){
		layer.msg(JSON.stringify(date.false));
		return false;
	});
});
</script>
```

```
<form class="layui-form" action="">  //class 追加layui-form-pane改变表单风格
	<div class="layui-form-item">
		<label class="layui-form-label">name</label>
		<div class="layui-input-block">
			<input classs="layui-input" type="text" placeholder="plase input you name " >
		</div>
	</div>
</form>
```

**输入框**

​	*required*：注册浏览器所规定的必填字段 

 	lay-verify：注册form模块需要验证的类型 

 	class="layui-input"：layui.css提供的通用CSS类 	

**复选框 type="checkbox"**

 	属性*title*可自定义文本（温馨提示：如果只想显示复选框，可以不用设置title） 
 	属性*checked*可设定默认选中 
 	属性*lay-skin*可设置复选框的风格 
 	设置*value="1"*可自定义值，否则选中时返回的就是默认的on

**开关 lay-skin="switch"**

 	属性*checked*可设定默认开 
 	属性*disabled*开启禁用 
 	属性*lay-text*可自定义开关两种状态的文本 
 	设置*value="1"*可自定义值，否则选中时返回的就是默认的on

**单选 type="radio"**

 	属性title可自定义文本 
 	属性disabled开启禁用 
 	设置value="xxx"可自定义值，否则选中时返回的就是默认的on

**文本域 class="layui-textarea"**

**忽略美化渲染 *lay-ignore***

## **4、导航**

```
//依赖模块element
<script>
	layui.use('element' , function(){
 		var element = layui.element();
 });
</script>
```

```
<ul class="layui-nav">
	<li class="layui-nav-item"><a href="">nav1</a></li>
	<li class="layui-nav-item layui-this"><a href="">nav2</a></li>//设定layui-this来指向当前页面分类。
	<li>
		<a class="layui-nav-child">nav3</a>
		<dd><a href="">nav3.1</a></dd>
		<dd><a href="">nav3.2</a></dd>
		<dd><a href="">nav3.3</a></dd>
	</li>
</ul>
```

**设定*layui-this*来指向当前页面分类。**

**水平、垂直、侧边三个导航的HTML结构是完全一样的，不同的是：**

 	垂直导航需要追加class：l**ayui-nav-tree**
 	侧边导航需要追加class：**layui-nav-tree layui-nav-side**

- 面包屑

  ```
  //首页> 国际新闻> 亚太地区> 正文
   <span class="layui-breadcrumb">
          <a href=""> 首页</a>
          <a href=""><新闻/a>
          <a href=""><cite>正文</cite></a>
      </span>
  ```

可以通过设置属性 **lay-separator="-"**来自定义分隔符。如： [首页-](http://www.layui.com/doc/element/nav.html) [国际新闻-](http://www.layui.com/doc/element/nav.html) [亚太地区-](http://www.layui.com/doc/element/nav.html) 正文

## **5、选项卡 layui-tab**

```
//依赖模块element
<script>
	layui.use('element' , function(){
 		var element = layui.element();
 });
</script>
```

```
 <div class="layui-tab">
        <ul class="layui-tab-title">
            <li class="layui-this">tab1</li> //layui-this默认选中
            <li>tab2</li>
            <li>tab3</li>
        </ul>
        <div class="layui-tab-content">
            <div class="layui-tab-item layui-this">tab1-content</div>
            <div class="layui-tab-item">tab2-content</div>
            <div class="layui-tab-item">tab-content</div>
        </div>
    </div>
```

**通过追加class：*layui-tab-brief* 来设定简洁风格。**

**通过追加class：*layui-tab-card*来设定卡片风格**

**对父层容器设置属性 *lay-allowClose="true"* 来允许Tab选项卡被删除**

值得注意的是，如果存在 *layui-tab-item* 的内容区域，在切换时自动定位到对应内容。如果不存在内容区域，则不会定位到对应内容。你通常需要设置过滤器，通过 *element*模块的监听tab事件来进行切换操作。

- **ID定位**

你可以通过对选项卡设置属性 lay-id="xxx" 来作为唯一的匹配索引，以用于外部的定位切换，如后台的左侧导航、以及页面地址 hash的匹配.

```
    <div class="layui-tab" lay-filter="test1">
        <ul class="layui-tab-title">
            <li layui-id="111" class="layui-this">tab1</li>
            <li layui-id="222">tab2</li>
            <li layui-id="333">tab3</li>
        </ul>
        <div class="layui-tab-content">
            <div class="layui-tab-item layui-show">tab1-content1</div>
            <div class="layui-tab-item">tab2-content2</div>
            <div class="layui-tab-item">tab-content3</div>
        </div>
    </div>
```

```
<script>
        layui.use('element',function () {
            var element();
            //获取hash来切换选项卡，假设当前地址的hash为lay-id对应的值
            var layid = location.hash.replace(/^#test1=/ , "");
            element,tabChange('test1',layid); //假设当前地址为：http://web.com#test1=111,name选项卡会自动切换到“tab2 ”这一项
            element.on('tab(test1)',function () {
                location.hash = 'test='+this.getAttribute('lay-id');
            });
        });
    </script>
```

属性 lay-id 是扮演这项功能的主要角色，它是动态操作的重要凭据

同样的还有增加选项卡和删除选项卡，都需要用到 lay-id，更多动态操作请阅读：[element模块](http://www.layui.com/doc/modules/element.html)



## **6、表格 layui-table**

```
<table class="layui-table">
        <!--划分列-->
        <colgroup>
            <col width="150">
            <col width="150">
            <col>
        </colgroup>
        <!--表单分类-->
        <thead>
        <tr>
            <th>title1</th>
            <th>title2</th>
            <th>ttle3</th>
        </tr>
        </thead>
        <!--表单内容-->
        <tbody>
        <tr>
            <td>content1</td>
            <td>content2</td>
            <td>content3</td>
        </tr>
        </tbody>
    </table>
```

| 属性名            |       属性值        | 备注                                       |
| :------------- | :--------------: | :--------------------------------------- |
| lay-skin="属性值" | line / row / nob | *line*：定义*行边框* 风格表格 *row*：定义*列边框* 风格表格 *nob*：定义 *无边框* 风格表格 |
| lay-even       |        no        | 用于开启 *偶数行* 背景，可与其它属性一起使用                 |

## **7、[进度条](http://www.layui.com/doc/element/progress.html)**

```
//依赖模块element
<script>
	layui.use('element' , function(){
 		var element = layui.element();
 });
</script>
```

```
<div class="layui-progress">
        <div class="layui-progress-bar" lay-percent="百分比"></div>
    </div>
```

属性 *lay-percent* ：代表进度条的初始百分比，你也可以动态改变进度，详见：[进度条的动态操作](http://www.layui.com/doc/modules/element.html#progress)

当对元素设置了**class= layui-progress-big** 时，即为大尺寸的进度条风格。默认风格的进度条的百分比如果开启，会在右上角显示，而大号进度条则会在内部显示。

## **8、面板**

```
//依赖模块element
<script>
	layui.use('element' , function(){
 		var element = layui.element();
 });
</script>
```

```
<div class="layui-collapse" lay-accordion> // lay-accordion 来开启手风琴
        <div class="layui-colla-item">
            <h2 class="lay-cola-title">title</h2>
            <div class="layui-colla-content layui-show">content</div> //layui-show默认展开
        </div>
    </div>
```

在折叠面板的父容器设置属性 *lay-accordion* 来开启手风琴，那么在进行折叠操作时，始终只会展现当前的面板。



## **9、辅助 layui-elem-quote / layui-quote-nm**

```
<blockquote class="layui-elem-quote">引用区域的文字</blockquote>
<blockquote class="layui-elem-quote layui-quote-nm">包含区域的文字</blockquote>
```



有些事你不去做永远不知道有多美妙！ --by:Orange

本文由：**橙子工作室**  整理推出