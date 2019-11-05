---
title: laravel-tinker学习笔记
date: 2017-11-08 14:58:11
tags: laravel
categories:
- laravel
desc: laravel,tinker使用教程
keyword: laravel,tinker使用教程
---

## 默认进入laravel主目录下，

```
php artisan tinker

$obj = new \App\xxxModel; //给表中写入数据

$obj->表字段 = "content";

$obj->name = "orange";

$obj->save();
```

## 使用时会出错原因：<!--more-->

```
1、单词写错了，或者命名空间不对。

2、除字符串以外的内容都应该是英文输入，不然识别不了

3、自己的粗心，退出【exit()】重新来。  
```

## 使用tinker向数据库中进行数据填充：

开源库Faker：https://github.com/fzaninotto/Faker

- 新建一张articles表(加 S )

  ```
  php artisan make:migration create_article_table 
  ```

- 打开新建的articles表，添加数据表字段：App/database/migrations/articles

  ```
  public function up()
      {
          Schema::create('article', function (Blueprint $table) {
              $table->increments('id');
              $table->string('title',30)->default('');
              $table->text('content');
              $table->timestamps();
          });
      }
  ```

  ```
  //数据填充
  php artisan migrate
  //建立模型关联
  php artisan make:model Article
  --------------------------------------------
  class Model extends EloquentModel{
        //如果model名与实际表名不一致，需要制定
       // protected $table="posts"; //如果我们的表不是posts，则须填写
        //自动写入时间戳
       //public $timestamps=true;
        protected $guarded = [];//不可以往数据库中写入数据的字段
      //protected $fillable = [
    //        'title','content',
    //    ];//可以从数据库中写入的字段
  }
  ```

- 配置factory : App/database/factories/Userfac tory.php

  ```
  $factory->define(App\Article::class,function (Faker\Generator $faker){
      return [
          'title'=>$faker->sentence(6),
          'content'=>$faker->paragraph(10),
      ];
  });
  ```

- 使用tinker添加测试数据

  ```
  php artisan tinker
  //测试配置
  factory(App\Article::class,10)->make(); //创建10条测试数据，并没有写入数据库

  factory(App\Article::class,10)->create();/创建10条数据并写入数据库
  ```

来自沐橙的Blog：http://www.orange.net.oschina.io/