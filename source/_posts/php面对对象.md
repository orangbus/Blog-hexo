---
title: php面对对象学习笔记
date: 2017-10-13 23:49:12
tags: php
categories:
- php
desc: php,对象的访问与创建，php对象引用，php面对对象学习笔记，php面对对象编程
keword: php,对象的访问与创建，php对象引用，php面对对象学习笔记，php面对对象编程
---

[TOC]

## 对象创建与访问

```
<?php
class Person{ //创建一个Person的类
    public $name; //定义一个成员属性
    public function eat(){  //定义一个eat方法
        echo "I am eating...";
    }
}
$orange = new Person(); //实例化一个person的类
echo $orange->name = "orange"; //orange 访问一个成员属性
$orange->eat(); //I am eating... 问一个类方法

?>
```

## 面对对象： 属性+方法

**构造函数 (__construct )：**为对象成员属性定义一个初始值 , 实例化的时会自动调用实例化传递的参数。

**析构函数 (__destruct )：** 在程序执行结束的时候会被自动调用；<!--more-->

​		    可以把变量设置成 null 来调用析构函数；实际是变量不在被调用时被清理

​		    清理程序使用的资源；

```
<?php
class Person{
    public $name;
    public $age;
    public $hobby;

    public function __construct($name='orange',$age='21',$hobby='php'){ //定义一个初始值
        echo $this->name = $name;
        echo $this->age = $age;
        echo $this->hobby = $hobby;
    }
    public function deno(){
        echo $this->name;
        echo $this->age;
        echo $this->hobby;
    }
    //析构函数(destructor) 与构造函数相反，当对象结束其生命周期时（例如对象所在的函数已调用完毕），系统自动执行析构函数。
    public function __destruct(){
        echo $this->hobby . "被销毁了";
    }
}
//当我实例化 Person的时候就出自动调用 __construct
//实例化的参数会替换 __construct 对应的初始化参数
$orange = new Person("mucheng"); //mucheng 21 php
$orange->deno();////mucheng 21 php
?>
```

## 对象引用：

​	对象的引用用于访问对象的属性和方法。

```
$orange = new person();
$orange1 = $orange;     //$orange 和 $orange1 是两个独立的引用
#orange2 = &$orange;    //$orange2 是对 $orange的引用，都是用了对象的同一个引用，任何一个引用为 null 相当于删除这个引用
```

## 继承（extends）

​	简单说就是把**共同的属性和方法**写在一个类（父类）里面，把**不同的属性和方法**写在一个类（子类）中，**子类**通过 extends **父类**的属性和方法。

```
<?php
class Father{
    public $color = 'init red';
    public function show(){
        echo "class is father";
    }
}
//子类继承了父类，子类就可以直接调用父类的属性和方法
class Son extends Father{
    public function index(){
        echo $this->color; //这里的this指 Son这个对象
    }
    public function index2(){
        $this->show();
    }
}
$orange = new Son();
$orange->index(); //init red
$orange->index2(); //class is father
?>
```

## 访问控制

[TOC]

**public :**  公有类成员，可以在**任何地方、子类**访问。

**protected ：** 受保护的类成员，可以被**自身、子类**访问。

**private：**（只能被**自己内部**的属性和方法访问，不能被**外部、子类**访问访问）

```
<?php
class Person{
    public $name = "orange";
    protected $age = 21;
    private $hobby = 'php';

    public function demo(){
        echo $this->hobby;
    }
}
class Man extends Person{
    public function index(){
        echo $this->name;
    }
    public function index2(){
        echo $this->age;// 这是受保护的属性，子类可以访问
    }
    public function index3(){
        echo $this->hobby; //这是私有属性，只能被本类调用
    }
}
$orange = new Man();
$orange->index(); //orange
$orange->index2(); //21
$orange->index3();// Notice: Undefined property: Man::$hobby
$orange->demo(); //php
?>
```

```
<?php
class Person{
    public function demo(){
        echo "fun_pulic";
    }
    protected function demo2(){
        echo "fun_protected";
    }
    private function demo3(){
        echo "fun_private";
    }
}
class Man extends Person{
    public function index(){
        echo $this->demo();
    }
    public function index2(){
        echo $this->demo2();// 这是受保护的方法，子类可以访问
    }
    public function index3(){
        echo $this->demo3(); //这是私有方法，只能被本类调用
    }
}
$orange = new Man();
$orange->index(); //fun_public
$orange->index2(); //fun_protected
$orange->index3();// Fatal error: Call to private method Person::demo3() from context 'Man'
?>
```

## 关键字

- **static** : **可以**用来标识属性和方法，类似于全局变量，**不能**使用 this 来引用。

  ​**访问**：类名 :: 静态成员属性 / 方法名（）

  ​	  **self ::** 静态成员属性 / 方法名（）

- **final :** 父类中的方法被声明为 final，则子类无法覆盖该方法。如果一个类被声明为 final，则不能被继承。

- **重写：****子类**的方法名和**父类**的方法名**一致**，**子类**的方法就会**覆盖父类**的方法

  ​	使用 final 可以**防止父类被重写**，也就是说，当父类定义了final关键字之后，同样的函数名，子类的函数不会覆盖父类的函数，我们照样可以访问父类的函数。

  ​	**访问被重写的父类方法：**parent :: 父类被重写的方法名；

- **const** : 将子类的成员属性定义为**常量**，**不能**重新赋值。

  ​     const声明常量不用使用 “ $ ” 符号，而且常量名都是大写的。（const NAME = "orange"）

```
<?php
class Person{
    public static $name = "orange";
    final public function index(){
        echo "this is Person final";
    }
    const HOBBY = "php";
    //重写演示
    public function demo(){
        echo "Person-demo";
    }
}
class Son extends Person{
//    public function index(){
//        echo "this is Son index";
//    }
    public function index2(){
        //访问父类的 static 属性
        echo self::$name;
    }
    public function index3(){
        echo self::HOBBY;
    }
    //重写父类的 denmo 方法
    public function demo(){
        echo "Son-demo";
    }
}

$orange = new Son();
//直接访问父类的
print (Person::$name); //orange
$orange->index2();//orange

$orange->index();//Fatal error: Cannot override final method Person::index()

//直接访问Person的 const
echo Person::HOBBY;//php
$orange->index3(); //php

//重写
$orange->demo();//Son-demo
?>
```

## 重写：

​	子类的方法名和父类的方法名一致，子类的方法就会覆盖父类的方法叫重写。

​	使用 final 可以**防止父类被重写**，也就是说，当父类定义了final关键字之后，同样的函数名，子类的函数不会覆盖父类的函数，我们照样可以访问父类的函数。

​	**访问被重写的父类方法：**parent :: 父类被重写的方法名；

## self ::

- 访问静态属性 / 方法	【self :: $name】
- 调用本类的其他方法 【self :: otherfunction ( ) 】
- 访问 const 定义的常量 【self :: NAME】

## 接口

- 定义接口：interface
- 实现接口： implements
- 判断某个对象是否实现了某个接口：instanceof 
- 接口可以使用  extends 继承 ，子类实现接口时，父类接口定义的方法也需要在这个类里面具体实现

```
<?php
interface person{
    public function eat();
    public function sleep();
}
class Man implements Person{
    public function eat()
    {
        echo "eating...";
    }
    public function sleep()
    {
        echo "sleep....";
    }
}
$orange = new Man();
$orange->eat();
$orange->sleep();
```

------

接口是类的模板，类是对象的模板，模板定义的方法（空方法）类必须全部实现。

------

## 抽象类（abstract）

- 抽象类不能实例化。

- 抽象类定义的方法，子类必须实现这个抽象类里面的所有方法。

- 调用：通过子类**继承**，实例化子类进行调用。

- 不能在抽象类中创建对象，或者函数体。

- 抽象类通常具有抽象方法，方法中没有大括号。

- 抽象方法中可以有参数，但是如果有的话，子类实现这个抽象方法的话，必须和抽象类中的参数个数相同。​​

  ```
  //计算矩形面积
  <?php
  abstract class Shape{
      abstract protected function getArea();//没有大括号
      abstract protected function test();//这是一个假设的抽象方法
  }
  // $tip = new Shape(); //你不能这样实例化一个抽象类
  class Rectangle extends Shape{ //通过子类继承实例化
      private $w;
      private $h;

      function __construct($w=0,$h=0){
          $this->w = $w;
          $this->h = $h;
      }
      public function getArea(){
          echo $this->w * $this->h;
      }
      public function test(){
          //这里可以留空，目的是子类必须实现Shape抽象类里的所有方法，不然你懂的
      }
  }
  $result = new Rectangle(10,5);
  $result->getArea(); //50
  ?>
  ```

## 调用父类构造方法

PHP 不会在子类的构造方法中自动的调用父类的构造方法。要执行父类的构造方法，需要在子类的构造方法中调用 **parent::__construct()** 。

```
<?php
class BaseClass {
   function __construct() {
       print "BaseClass 类中构造方法" . PHP_EOL;
   }
}
class SubClass extends BaseClass {
   function __construct() {
       parent::__construct();  // 子类构造方法不能自动调用父类的构造方法
       print "SubClass 类中构造方法" . PHP_EOL;
   }
}
class OtherSubClass extends BaseClass {
    // 继承 BaseClass 的构造方法
}

// 调用 BaseClass 构造方法
$obj = new BaseClass();

// 调用 BaseClass、SubClass 构造方法
$obj = new SubClass();

// 调用 BaseClass 构造方法
$obj = new OtherSubClass();
?>
```

执行以上程序，输出结果为：

```
BaseClass 类中构造方法
BaseClass 类中构造方法
SubClass 类中构造方法
BaseClass 类中构造方法
```

## 魔术变量

__get(\$var) :  通过外部传递私有属性值访问私有属性

__set(\$var , $value) ; 赋值私有属性值

__isset() : 检查私有属性是否存在

__unset() :  删除私有属性

__call (\$fun , $value): 调用的方法不存在的时候自动调用\_\_call 方法

__callStaic(\$fun , $val) : 当静态方法不存在的时候自动被调用

__invoke($val):当你把对象当函数使用的时候自动被调用

__toString() : 当对象呗当成变量输出的时候被调用

[TOC]

------

- __get() :  通过外部传递私有属性值访问私有属性

  ```
  class Person{
      private $name = "orange";
      
      public function __get($name){ //通过外部传递私有属性值访问私有属性
          return $this->$name;
      }
  }
  $orange = new Person("MuCheng",'10');
  //var_dump($orange); //object(Person)#1 (1) { ["name":"Person":private]=> string(7) "MuCheng" }
  var_dump($orange->name); //MuCheng
  ?>
  ```

- __set() ; 赋值私有属性值

  ```
  class Person{
      private $name = "orange";
      public function __set($name,$value){ //通过外部传递私有属性值访问私有属性
          return $this->$name = $value;
      }
      public function __get($name){ //外部访问私有属性需要通过__get($val)
          return $this->$name;
      }
  }
  $orange = new Person();
  $orange->name = "new val"; //给私有属性赋值
  echo $orange->name; //new val
  ?>
  ```

- __isset : 检查私有属性是否存在

  ```
  <?php
  class Demo{
      private $name = "orange";
  }
  $orange = new Demo();
  var_dump(isset($orange->name)); //false
  ----------------------------
  <?php
  class Demo{
      private $name = "orange";

      public function __isset($name){
          return isset($this->$name);
      }
  }

  $orange = new Demo();
  var_dump(isset($orange->name)); //true
  var_dump(isset($orange->age)); //false
  ```

- __unset :  删除私有属性

  ```
  <?php
  class Demo{
      private $name = "orange";

      public function __get($name){
          return $this->$name;
      }
      public function __unset($name){
          echo "unset:".$name;
          unset($this->$name);
      }
  }
  $orange = new Demo();
  var_dump($orange->name); //orange
  unset($orange->name);//unset:name
  ```

- __call : 调用的方法不存在的时候自动调用\_\_call 方法

  ```
  <?php
  class Demo{

  }
  $orange = new Demo();
  $orange->index(); //Fatal error: Call to undefined method Demo::index()
  -----------------------------------------------
  <?php
  class Demo{
      public function __call($fun , $value){
      echo $fun . "br>";
      var_dump($value) ;
      }
  }
  $orange = new Demo();
  $orange->index(); //indexbr>array(0) { }
  ```

- __callStaic : 当静态方法不存在的时候自动被调用

  ```
  <?php
  class Demo{
      public static function __callStatic($name, $arguments)
      {
          echo $name."<br>";
          var_dump($arguments);
      }
  }
  $orange = new Demo();
  Demo::index(); //index  array(0) { }
  ```

- __invoke():当你把对象当函数使用的时候自动被调用

  ```
  <?php
  class Demo{
      public function __invoke($val){
          var_dump($val);
      }
  }
  $orange = new Demo();
  $orange("orange"); //string(6) "orange"
  ```

- __toString() : 当对象呗当成变量输出的时候被调用

  ```
  <?php
  class Demo{
      public function __toString(){
          var_dump("you cannot echo object");
      }
  }
  $orange = new Demo();
  echo $orange; //string(22) "you cannot echo object"
  ```

## 自动加载__spl_autoload_register(fun)

- mina.php

  ```
  <?php
  class Mian{
      public function mian(){
          echo "this is other class";
      }
  }
  ```

- index.php

  ```
  <?php
  spl_autoload_register(function ($className){
      require $className . ".php";
  });
  //或者使用下面的方法
  function index($className){
      require $className . ".php";
  }
  spl_autoload_register('index');

  $orange = new Mian();
  $orange->mian();//this is other classthis is other class
  ```

## 克隆 clone

- 浅复制：传递的是指向同一个地址的对象属性和方法

  ```
  <?php
  class A{
      public $name = "orange";
  }
  $a = new A();
  $b = $a;
  $b->name = "MUCheng";
  var_dump($a->name);//string(7) "MUCheng"
  ```

- 深复制：复制一份对象属性和方法

  ```
  <?php
  class A{
      public $name = "orange";
  }
  $a = new A();
  $b = clone $a;
  $b->name = "MUCheng";
  var_dump($a->name);//string(6) "orange"
  ```

- __clone

  ```
  <?php
  class A{
      public $name = "orange";
      public $obj = null;
      public function __clone(){
          $this->obj = clone $this->obj;
      }
  }
  class B{
      public $sex = "man";
  }
  $a = new A();
  $a->obj = new B();
  $b = clone $a;
  $b->name = "MUCheng";
  $b->obj->sex = 'woman';
  var_dump($a->obj->sex);//string(6) "orange"
  ```

## 类型约束

​	当我们传递一个变量必须是一个类的返回值的时候

```
<?php
class A{
    public function index(){
        echo "A-index";
    }
}
function demo(A $a){
    $a->index();
}
demo(new A);
```

## trait --use 代码的重用性

可以同时引用多个trait ， use traitA,traitB , trait 里面也支持嵌套 trait

```
<?php
trait Main{
    public function one(){
        echo "hello ";
    }
    public function two(){
        echo "word";
    }
    public function index(){
        $this->one();
        $this->two();
    }
}
class B{
    use Main;
}
$orange = new B();
echo $orange->index(); //hello woed
```

[TOC]

<p style="text-align:center">知识是用来分享的<p>

转载请注明原文地址：http://www.orange.net.oschina.io/