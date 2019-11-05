---
title: Layui子窗口操作父窗口
date: 2017-09-06 16:31:40
tags: layui
categories: 
- html
desc: Layui子窗口操作父窗口,layer使用技巧，layui弹窗
keyword: Layui子窗口操作父窗口,layer使用技巧，layui弹窗
---

### 父弹窗代码（默认 css & js and jquery已经引入）

```
<button class="layui-btn" onclick="demo()">demo</button>

<script>
    layui.use('layer',function () {
        var layer = layui.layer;
    });
    function demo() {
       layer.open({
           type: 2,
           fixed: false, //不固定
           maxmin: true,
           content: 'demo.html'
       });
    }
}
</script>
```

- 子类属性<!--more-->

  ```
  <button id="new">父类弹层</button>
  <button id="msg">给父页传值</button>
  <input type="text" id="val"> //输入子窗口传给父类的值
  <button id="out">关闭弹窗</button>
  ```

- 父类属性

  ```
  <p id="parentVal">一会这里会有子类传过来的值，我会改变</p>
  ```

## 1、子让父弹窗

```
var index = parent.layer.getFrameIndex(window.name);//获取窗口索引

$('#new').on('click',function () {
        parent.layer.open({
            type: 1,
            content: "http://www.baidu.com"
        });
    });
```

## 2、子传值给父

```
var index = parent.layer.getFrameIndex(window.name);//获取窗口索引

$('#msg').on('click',function () {
        parent.$("#parentVal").text("我是子类传过来的值");// #parentVal 是父类的属性 id
        parent.layer.close(index);
    });
```

## 3、带值关闭弹窗

```
 $("#out").click(function () {
        var val = $("#val").val(); //这里是【子窗口】输入的值
        parent.layer.msg("子类的内容： " + val);
        parent.layer.close(index);
    });
```

## 4、子弹窗自适应

```
$('#add').on('click', function(){
    $('body').append('可以加载的时候添加一些东西');
    parent.layer.iframeAuto(index);//主要是这条语句
});
```

Ps：其它暂时没有想到，欢迎加入【云南php&layui交流群】讨论：[511300462](https://jq.qq.com/?_wv=1027&k=5OiptEd)

**参考：**http://layer.layui.com/