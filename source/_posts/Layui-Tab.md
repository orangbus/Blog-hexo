---
title: Layui_Tab
date: 2017-07-25 23:51:37
tags: html
categories: 
- html
from: http://www.layui.com/doc/
keyword: layui layui-tab使用教程
---

# Layui_Tab-Note

1、加载需要使用的模块。

```
layui-use('element',function(){
  var $ = layui-jquery,
  	  element = layui.element();
  	  
  	  //do something
});
```

2、添加出发事件。<!--more-->

```
var active = {
	//新增一个选项卡
  tabAdd:function(){
    element.tabAdd('demo',{ //获取<div lay-filter="demo">,也就是tab选项卡的div属性
      title:'add_title',
      content:'add_content',
      id:'add_id'
    });//获取添加的对象 ， 添加的内容加属性
}，
  //删除和切换同理
  element.tabDelment('demo','11'); //选中tab下id为11的选项卡
  othis.addClass('layui-btn-disabled');//新增的tab添加一个class属性
},
  element.tabChange('demo','22'); //选中tab下id为11的选项卡
},
```

3、监听按钮事件。

```
$('.site-demo-active').on('click', function(){ //获取button的class值，添加click事件
    var othis = $(this),
    type = othis.data('type');
    active[type] ? active[type].call(this, othis) : '';
  });
```



