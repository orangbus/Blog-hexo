---
title: 内联元素和块元素大全（html）
date: 2017-08-25 22:19:40
tags: html
categories: 
- html
from: http://www.missyuan.com/thread-438824-1-1.html
desc: 内联元素和块元素大全（html）,css,html内联元素和块元素详解
---

>内联元素和块元素区分

- a 是内联元素
- div 是快元素

```
a{color:#fff; background:#F93;width:100px;height:50px;
/*内联元素的宽度高度不起作用-->
<!--加了这个转换才有作用-->
<!--将内联元素转换为块元素*/
display: block;
}

没有加  display: block; 之前
width:100px;height:50px; 将对 a不起作用

总结：既是a标签为内联元素，直接设置它的 width和height将不起作用，需要将内联元素转换为 【块状元素】方可对 width和height进行设置
```

> 块元素<!--more-->

```
address - 地址
blockquote - 块引用
center - 举中对齐块
dir - 目录列表
div - 常用块级容易，也是CSS layout的主要标签
dl - 定义列表
fieldset - form控制组
form - 交互表单
h1 - 大标题
h2 - 副标题
h3 - 3级标题
h4 - 4级标题
h5 - 5级标题
h6 - 6级标题
hr - 水平分隔线
isindex - input prompt
menu - 菜单列表
noframes - frames可选内容，（对于不支持frame的浏览器显示此区块内容
noscript - 可选脚本内容（对于不支持script的浏览器显示此内容）
ol - 排序表单
p - 段落
pre - 格式化文本
table - 表格
ul - 非排序列表
```

> 内联元素

```
a - 锚点
abbr - 缩写
acronym - 首字
b - 粗体(不推荐)
bdo - bidi override
big - 大字体
br - 换行
cite - 引用
code - 计算机代码(在引用源码的时候需要)
dfn - 定义字段
em - 强调
font - 字体设定(不推荐)
i - 斜体
img - 图片
input - 输入框
kbd - 定义键盘文本
label - 表格标签
q - 短引用
s - 中划线(不推荐)
samp - 定义范例计算机代码
select - 项目选择
small - 小字体文本
span - 常用内联容器，定义文本内区块
strike - 中划线
strong - 粗体强调
sub - 下标
sup - 上标
textarea - 多行文本输入框
tt - 电传文本
u - 下划线
var - 定义变量
```