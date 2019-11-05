---
title: jQuery_confirm使用教程
date: 2017-09-19 19:33:18
tags: html
categories: 
- html
desc: jQuery_confirm入门教程，jQuery_confirm实例
keyword: jQuery_confirm入门教程，jQuery_confirm实例
---

> 官网链接：https://craftpip.github.io/jquery-confirm/（打不开多刷新几次，不行就直接Google）
>
> Desc：一个很好看的消息提示
>
> 演示地址：http://karle.vip/jc.html

#### 前期准备

```
<link rel="stylesheet" href="dist/confirm.css">
<link rel="stylesheet" href="dist/bootstrap.css">//添加样式的，虽然可选，但是不添加就畸形了

<script src="https://cdn.bootcss.com/jquery/1.12.4/jquery.min.js "></script>
<script src="dist/confirm.js"></script>
```

#### 简单的使用

```
<button type="button" class="btn">demo</button>
```

```
<script>
	$(function(){
      $('.btn').on('click',function(){
        	$.alert({
              title: "this is title",
              content: "content:",
              //你还可以添加其他的 option
        	});
      });
	});
</script>
```

#### 借助翻译工具翻译的 option（不对的地方还望指正）<!--more-->

| 名称                             | 类型                          | 默认                                       | 描述                                       |
| ------------------------------ | --------------------------- | ---------------------------------------- | ---------------------------------------- |
| title                          | string , function           | `'Hello'`                                | 对话框的标题 还接受返回一个字符串的函数。                    |
| titleClass                     | string                      | `''`                                     | 将应用于标题的类。                                |
| type                           | string                      | `'default'`                              | 颜色模态给用户一个成功/失败/警告的提示，可用的选项是：“蓝色，绿色，红色，橙色，紫色和黑色” |
| typeAnimated                   | boolean                     | `true`                                   | 为颜色添加一点动画。                               |
| draggable                      | boolean                     | `true`                                   | 使对话框拖动，拖动点是模型的标题，如果标题未定义，模型将不会拖动。当使用draggable时，将alignMiddle设置为false。 |
| dragWindowGap                  | number                      | `15`                                     | 模态和窗口之间的可拖动差距，默认为15px                    |
| dragWindowBorder               | boolean                     | `true`                                   | 如果模式应该被限制在窗口内                            |
| animateFromElement             | boolean                     | `true`                                   | 从点击的元素动画模态                               |
| alignMiddle                    | boolbean                    | `true`                                   | 重要事项`@deprecated` 模型将位于屏幕的中心。 当模型中的内容发生变化时，模型会自动重新定位。 |
| smoothContent                  | boolean                     | `true`                                   | 内容在模态变化时平滑的高度转换。                         |
| content                        | string  , function          | `'Are you sure to continue?'`            | 对话框的内容。接受返回字符串或ajax承诺的函数。                |
| contentLoaded                  | funtion                     | `function(data,status,xhr){}`            | 仅在内容通过Ajax加载时才使用。被称为总是回调$ .ajax          |
| buttons                        | object                      | `{}`                                     | 按钮的多个定义 请参见按钮属性的 [按钮定义](https://craftpip.github.io/jquery-confirm/#defining-buttons) |
| icon                           | string                      | `''`                                     | 标题前面加上图标类。例如：'fa fa-icon'                |
| lazyOpen                       | boolean                     | `false`                                  | 不立即打开模态。需要调用open（）函数来打开模态                |
| bgOpacity                      | float                       | `null`                                   | 如果为null，则应用主题的默认bg不透明度。                  |
| theme                          | string                      | `'light'`                                | 对话框的颜色主题。可能的选择是“轻”，“黑”，“物”和“引导”          |
| animation                      | string                      | `'zoom'`                                 | 对话框的打开动画。可能的选项是右，左，底，顶，旋转，无，不透明度，缩放，缩放，scaleY，scaleX，rotateY，rotateYR（反向），rotateX，rotateXR（反向） “模糊”动画在v1.1.2 |
| closeAnimation                 | string                      | `'scale'`                                | 对话框的紧密动画。与动画相同的选项。                       |
| animationSpeed                 | number                      | `400`                                    | 动画持续时间（毫秒）。                              |
| animationBounce                | float                       | `1`                                      | 添加反弹打开动画，1 =无反弹                          |
| escapeKey                      | string , boolean            | `false`                                  | 如果是false，escapeKey不会工作，如果为true，将会工作，但没有回调，如果字符串，将被分配到按钮。 |
| RTL                            | boolean                     | `false`                                  | 使用右边的文本布局。                               |
| container                      | string                      | `'body'`                                 | 指定生成的jconfirm的HTML内容应该附加在哪里。默认情况下，它附加在文档的<body>中。 |
| containerFluid                 | boolean                     | `false`                                  | 如果为true，将使用容器流体布局，使用完整的浏览器宽度。            |
| backgroundDismiss              | boolean , string , function | `false`                                  | 如果为false，用户将无法通过点击退出。如果真的，用户将能够点击出来，没有回调。如果字符串，将被分配到按钮。如果功能，将被用作回调。 |
| backgroundDismissAnimation     | string                      | `'shake'`                                | 当用户点击开箱即可执行抓取用户注意的动画。                    |
| autoClose(自动关闭)                | string                      | `false`                                  | 如果用户没有响应，在指定的时间内自动关闭对话框。可能的选择 buttonName' 字符串用管道分成两半，第一部分指定要触发的按钮名称。 另一半指定等待时间（以毫秒为单位）。 |
| closeIcon                      | boolean                     | `null`                                   | 默认情况下，如果两个按钮均为false，则CloseIcon可见。（对话模式）。可以通过将此值设置为true来显示closeIcon。 |
| closeIconClass                 | string                      | `false`                                  | 默认情况下，jQuery确认使用×html实体来获取此关闭符号。你可以在这里提供图标类来改变它。 |
| watchInterval                  | number                      | `100`                                    | 观察模式的变化，并以屏幕为中心。 ** 在2.5.0版中添加           |
| columnClass                    | string                      | `'col-md-4 col-md-offset-4 col-sm-6 col-sm-offset-3 col-xs-10 col-xs-offset-1'` | 提供更好的方式设置自定义宽度和响应。您还可以使用Bootstraps网格为不同的显示尺寸设置自定义宽度。 |
| useBootstrap                   | boolean                     | `true`                                   | 如果为true，则将在模态上设置引导类。'columnClass'将被设置在框上。如果为false，则引导类不会被设置，而是在框上设置“boxWidth”。 |
| boxWidth                       | string                      | `'50%'`                                  | 当您不打算在项目中使用引导时，此选项设置框的宽度 ** 只有在'useBootstrap'设置为false时才会工作， |
| scrollToPreviousElement        | boolean                     | `true`                                   | 滚动到jconfirm模型打开之前聚焦的元素。                  |
| scrollToPreviousElementAnimate | boolean                     | `true`                                   | 动画到上一个元素的滚动。                             |
| offsetTop                      | number                      | `40`                                     | 该模型将保持至少50px的窗口顶部。                       |
| offsetBottom                   | number                      | `40`                                     | 该模型将从窗口的底部至少保持50px。                      |
| bootstrapClasses               | object                      | `{ container: 'container', containerFluid: 'container-fluid', row: 'row', }` | 这些是在使用引导时设置的默认类，对于使用命名空间引导类的人员来说，此选项可用。  |
| onContentReady                 | function                    | `function(){}`                           | 当内容放在DOM中并且模态被打开时被调用。（当模态完成时）            |
| onOpenBefore                   | function                    | `function(){}`                           | 当模态将被打开时被调用。                             |
| OnOpen                         | function                    | `function(){}`                           | 当模态完成打开时被调用。                             |
| OnClose                        | function                    | `function(){}`                           | 当模态将被关闭时被调用。                             |
| onDestroy                      | function                    | `function(){}`                           | 在模态元素从DOM中删除之后被调用。                       |
| OnAction                       | function                    | `function(buttonName){}`                 | 当任何按钮被点击时调用。buttonName作为参数提供。            |

#### 简单的几个实例：

## demo2(消息提示框+倒计时操作)

```
function confirm_alert_ok_goto_dj(msg, url) {
    $.alert({
        title: '系统提示',
        content: msg,
        icon: 'fa fa-comment',
        type: 'green',
        autoClose: '好的|3000',
        escapeKey: '好的',
        buttons: {
            "好的": {
                btnClass: 'btn-blue',
                action: function() {
                    window.location.href = url;
                }
            }
        }
    });
}123456789101112131415161718
```

![demo3](http://img.blog.csdn.net/20170910165935923?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMTkyNjAwMjk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

------

## demo3(弹出框接收输入参数，Ajax提交)

```
function ajax_submit(id) {
    $.confirm({
        icon: 'fa fa-plus',
        type: 'blue',
        title: '添加数据',
        content: '<div class="form-group"><input type="text" placeholder="请输入名称" class="param_name form-control" /></div>' +
            '<div class="form-group"><input type="text" placeholder="请输入年龄" class="param_age form-control"/></div>',
        buttons: {
            '取消': function() {},
            '添加': {
                btnClass: 'btn-blue',
                action: function() {
                    var param_age = this.$content.find('.param_age').val();
                    var param_name = this.$content.find('.param_name').val();
                    confirm_alert("你提交的数据：id：" + id + "，名称：" + param_name + "，年龄：" + param_age);
                    /*//此处可写Ajax请求
                    $.get(url, {
                            name: param_name,
                            id: id,
                            age: param_age
                        },
                        function(result) {
                            //处理结果
                        });*/
                }
            }
        }
    });
}1234567891011121314151617181920212223242526272829
```

![demo3](http://img.blog.csdn.net/20170910170211327?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMTkyNjAwMjk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

后续更新。。。