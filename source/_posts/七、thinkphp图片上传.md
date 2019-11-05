---
title: 七、thinkphp图片上传
date: 2017-09-06 18:32:10
tags: thinkphp
categories:
- php
desc: tp5_无限极分类，php无限极分类，thinkphp图片上传
---

## thinkphp图片上传、替换、删除

### 文件上传

- enctype="multipart/form-data" // form　表单添加enctype元素图片不会显示出来
- 我们使用[钩子函数](https://www.kancloud.cn/manual/thinkphp5/135195)

controller

```
use app\admin\model\Article as Modelname;　//引入模型

$model = new Modelname();//实例化模型
if(request()->isPost()){　
  $date = input('post.');//这里提交的数据是进过model层处理过的数据
  $res  = $Modelname -> save($date);//保存数据
  if ($res){
       $this -> success("成功！" , url('lst'));
  }else{
       $this -> error("失败!");
    }
}
```

model<!--more-->

```
protected static function init(){
  Modelname::event('before_insert' , function($date){
    if($_FILES['索引']['文件类型']){//判断是否有图片上传
      $file = request()->file('索引');//获取上传文件
       $info = $file->move(ROOT_PATH . 'public' . DS . 'uploads');//移动文件到指定文件夹
       if ($info) {
             $thumb =DS.'uploads'.'/'.$info->getSaveName(); //获取文件路径及文件名（这里决定你以后删除文件的方式）
             $data['thumb'] = $thumb;//图片数据重新赋值　
        } else {
                    // 上传失败获取错误信息
                    echo $file->getError();
          }
    }
  });
 
}
```

### 文件替换

- 查询需要修改的那条数据
- 获取旧图片的完整路经，如果存在就删除
- 获取新图片＝》移动到相应的文件夹，再把新路径传递给key值为image，之后保存数据

```
if (request()->isPost()){
            $resdate = Db::table("blog_test")->field('image')->find(input('post.id'));//找到当前数据的全部信息,通常在表单中设置一个隐藏域，把当前数据的　ｉｄ　传递过来
            $oldimage = $_SERVER['DOCUMENT_ROOT'].DS.'upload/'.$resdate['image'];//获取图片的完整路径
            if (file_exists($oldimage)){
                unlink($oldimage);
            }else{
                echo "图片不存在";//可以忽略
            }
            //更新新图片的地址
            //获取上传文件
            $file = request()->file('image');
            //移动到相应的目录
            $info = $file ->move(ROOT_PATH . 'public'.DS.'upload');
            if ($info){
                //获取上传信息
                $name = $info -> getSaveName();//文件保存信息
                $_POST['image'] = $name;
            }
            $res = Db::table('blog_test') -> update([$_POST , 'id' => 当前数据的ｉｄ);
            if ($res){
                echo "ok";
            }else{
                echo "false";
            }
        }
```

Tip: 图片展示：/upload/＋数据库图片地址（从网站的根目录开始访问）

### 文件删除

- 通过ｉｄ查询数据的图片地址，并删除
- 执行删除操作

```
 //获取当前的删除数据的ｉｄ，查询图片地址，先删除图片在删除数据
        if (request()->isPost()){
            $resdate = Db::table('blog_test')->find(input('post.id'));
            $oldimage = $_SERVER['DOCUMENT_ROOT'].DS.'upload/'.$resdate['image'];//获取图片的完整路径
            if (file_exists($oldimage)){
                unlink($oldimage);
            }else{
                echo "图片不存在";
            }
            //删除数据
            $a = Db::table('blog_test')->find(input('post.id'));
            if ($a){
                echo "del ok";
            }
            
        }
```

欢迎加入**[云南php&layui交流群](https://jq.qq.com/?_wv=1027&k=5FT8Mh5)**：511300462