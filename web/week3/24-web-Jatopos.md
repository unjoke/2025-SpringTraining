# 文件包含-学习笔记

程序开发人员一般会把重复使用的函数写到单个文件中，需要使用某个函数时直接调用此文件，而无需再次编写，这种文件调用的过程一般被称为文件包含。可以创建供所有网页引用的标准页眉或菜单文件。

通常会把被包含的文件设置为变量，进行动态调用，从而导致客户端可以调用任意文件，造成文件包含漏洞。

## 文件包含函数
### include()
假设我们有一个定义变量的包含文件,这些变量可用在调用文件中.

```
include 'filename';

vars.php:
<?php
$color='red';
$car='BMW';
?>

<?php 
include 'vars.php';
echo "I have a $color $car"; // I have a red BMW
?>
```

找不到被包含的文件时只产生警告 ，脚本将继续执行。(require则会停止运行)

包含文本文件时可以直接输出，如果静态储存则不能

![image](https://github.com/user-attachments/assets/b28695f4-a214-46ac-a2ce-35e1e37ec6b7)

### include_once()
此语句和 include() 语句类似，唯一区别是如果该文件中的代码已经被包含，则不会再次包含。

![image](https://github.com/user-attachments/assets/0ee19351-1749-44b4-8012-d7394677e858)

### require()
找不到被包含的文件时会产生致命错误，并停止脚本

![2e72bba026f4a2f1ff6aa9264d131246](https://github.com/user-attachments/assets/8aacfe0d-d1e1-4961-94a5-821eaea42e51)

### require_once()
同include_once

文件包含的文件都会被解析为php

## 本地文件包含——日志文件包含
日志注入是通过插入恶意数据到应用程序的日志文件来实施的攻击。当这些日志被其他系统或应用程序读取和解析时，攻击者的恶意数据可能会被执行。这通常利用了不安全的日志处理机制和文件包含漏洞的组合。产生原因是 PHP 语言在通过引入文件时，引用的文件名，用户可控，由于传入的文件名没有经过合理的校验，或者校验被绕过，从而操作了预想之外的文件，就可能导致意外的文件泄露甚至恶意的代码注入。当被包含的文件在服务器本地时，就形成的本地文件包含漏洞。

可以通过burpsuite将命令写入日志文件中，比如向日志文件中写入“phpinfo”

![image](https://github.com/user-attachments/assets/0a08e165-a4ba-4ca9-b597-3c2030a3b935)

![image](https://github.com/user-attachments/assets/f37a57ed-5062-42b3-8f59-44cd454e201d)

再次包含日志文件，phpinfo被解析

## 远程文件包含
如果PHP的配置选项allow_url_include、allow_url_fopen状态为ON的话，则include/require函数是可以加载远程文件的，这种漏洞被称为远程文件包含(RFI)

利用webshell（可以使用蚁剑构建）植入木马

![image](https://github.com/user-attachments/assets/e0304d2e-deb5-4635-bf19-c8f5796ff377)


## PHP伪协议
PHP伪协议是PHP自己支持的一种协议与封装协议，简单来说就是PHP定义的一种特殊访问资源的方法。

文件包含的时候，可能遇到的文件包含函数：
```
include
require
include_once
require_once
highlight_file
show_source
flie
readfile
file_get_contents
file_put_contents
fopen (比较常见)
```

php支持的协议和封装协议，可用于文件包含函数
```
file:// — 访问本地文件系统
http:// — 访问 HTTP(s) 网址
ftp:// — 访问 FTP(s) URLs
php:// — 访问各个输入/输出流（I/O streams）
zlib:// — 压缩流
data:// — 数据（RFC 2397）
glob:// — 查找匹配的文件路径模式
phar:// — PHP 归档
ssh2:// — 安全外壳协议 2
rar:// — RAR
ogg:// — 音频流
expect:// — 处理交互式的流
```

### file://
访问本地文件系统
file:///flag

### php://
访问各个输入/输出流（I/O streams），PHP中最为复杂和强大的协议

### php://filter
php://filter 是一种元封装器， 设计用于数据流打开时的筛选过滤应用。 这对于一体式（all-in-one）的文件函数非常有用，类似 readfile()、 file() 和 file_get_contents()， 在数据流内容读取之前没有机会应用其他过滤器。简单通俗的说，这是一个中间件，在读入或写入数据的时候对数据进行处理后输出的一个过程。

php://filter可以获取指定文件源码。当它与包含函数结合时，php://filter流会被当作php文件执行。所以我们一般对其进行编码，让其不执行。从而导致任意文件读取。

resource=<要过滤的数据流> 这个参数是必须的。它指定了你要筛选过滤的数据流。resource=flag.php
read=<读链的筛选列表> 该参数可选。可以设定一个或多个过滤器名称，以管道符（|）分隔。php://filter/read=A|B|C/resource=flag.php
write=<写链的筛选列表> 该参数可选。可以设定一个或多个过滤器名称，以管道符（|）分隔。php://filter/write=A|B|C/resource=flag.php
<;两个链的筛选列表> 任何没有以 read= 或 write= 作前缀 的筛选器列表会视情况应用于读或写链。php://filter/A|B|C/resource=flag.php

格式：
?变量名=filter/read=/resource=/flag

常用：
```
php://filter/x=A|B|C|/resouce=xxx
php://filter/read=convert.base64-encode/resource=index.php
php://filter/resource=index.php
```

利用filter协议读文件，将index.php通过base64编码后进行输出。这样做的好处就是如果不进行编码，文件包含后就不会有输出结果，而是当做php文件执行了，而通过编码后则可以读取文件源码。而使用的convert.base64-encode，就是一种过滤器。

### 字符串过滤器
用来处理所有的流数据，单纯的字符串过滤器如rot13变换解决不了php文件
用base64的转换滤器解决php文件
string.rot13 rot13变换
string.toupper 转大写字母
string.tolower 转小写字母
string.strip_tags 去除html、PHP语言标签 (本特性已自 PHP 7.3.0 起废弃)

### php://input
php://input可以访问请求的原始数据的只读流，将post请求的数据当作php代码执行。当传入的参数作为文件名打开时，可以将参数设为php://input,同时post想设置的文件内容，php执行时会将post内容当作文件内容。从而导致任意代码执行。

```
php://input + [POST DATA部分]
```

例如：
http://127.0.0.1/cmd.php?cmd=php://input
POST数据：<?php phpinfo()?>
注意：
当enctype="multipart/form-data"的时候 php://input` 是无效的

遇到file_get_contents()要想到用php://input绕过。

使用burp suite软件抓包使用php://input伪协议将一句话木马写入。将数据包发送后，观察到一句话木马被包含。

![8f2a2c31f092192a5f8856e44439a648](https://github.com/user-attachments/assets/c31f0095-4765-465f-8a42-abe0106f820e)

### data://
数据流封装器，以传递相应格式的数据。可以让用户来控制输入流，当它与包含函数结合时，用户输入的data://流会被当作php文件执行。
如果传入的数据是PHP代码，就会执行代码
简单来说就是data://text/plain+,<?php 命令;?>

text/html → 按 HTML 解析

image/png → 按 PNG 图片处理

text/plain → 视为纯文本，直接显示内容，不执行任何解析。

```
示例用法：
1、data://text/plain,
http://127.0.0.1/include.php?file=data://text/plain,<?php%20phpinfo();?>
 
2、data://text/plain;base64,
http://127.0.0.1/include.php?file=data://text/plain;base64,PD9waHAgcGhwaW5mbygpOz8%2b

data://text/plain,<?php phpinfo();?>
data://text/plain;base64,PD9waHAgcGhwaW5mbygpOz8+

Example #1 打印 data:// 的内容
<?php
// 打印 "I love PHP"
echo  file_get_contents ( 'data://text/plain;base64,SSBsb3ZlIFBIUAo=' );
?>

Example #2 获取媒体类型
<?php
$fp    =  fopen ( 'data://text/plain;base64,' ,  'r' );
$meta  =  stream_get_meta_data ( $fp );

// 打印 "text/plain"
echo  $meta [ 'mediatype' ];
?>
```

### http://
访问 HTTP(s) 网址
例如
```
https://raw.githubusercontent.com/ProbiusOfficial/PHPinclude-labs/main/RFI
```

## 文件包含漏洞的防御
PHP:配置php.ini关闭远程文件包含功能(allow_url_include = Off)

严格检查变量是否已经初始化。

建议假定所有输入都是可疑的，尝试对所有输入提交可能可能包含的文件地址，包括服务器本地文件及远程文件，进行严格的检查，参数中不允许出现../之类的目录跳转符。

严格检查include类的文件包含函数中的参数是否外界可控。

不要仅仅在客户端做数据的验证与过滤，关键的过滤步骤在服务端进行。

在发布应用程序之前测试所有已知的威胁。

路径限制：限制被包含的文件只能在某一文件夹内，一定要禁止目录跳转字符，如：“../”；

包含文件验证：验证被包含的文件是否是白名单中的一员；

尽量不要使用动态包含，可以在需要包含的页面固定写好，如：include("head.php");，不要把被包含的写成变量。

## 其他文件系统函数
PHP 带有很多内置 URL 风格的封装协议，可用于类似 fopen()、 copy()、 file_exists() 和 filesize() 的文件系统函数。(类似于include函数)
比如 file_put_contents(), file_get_contents(), file(), readfile()

file_get_contents — 将整个文件读入一个字符串

file_put_contents — 将数据写入文件
和依次调用 fopen()，fwrite() 以及 fclose() 功能一样。
fopen — 打开文件或者 URL
fwrite — 写入文件（可安全用于二进制文件）
fclose — 关闭一个已打开的文件指针

# PHPinclude-labs
## Level 0
进入后发现源代码中含有include函数，同时界面有提示，backdoor.txt 内容为: <?php @eval($_POST['ctf']); ?>，含有危险的eval函数就可以操作一下

![QQ_1743090821722](https://github.com/user-attachments/assets/70aaf189-2c55-43b3-9aa1-2a8d18ed7126)

直接通过get方式访问backdoor.txt文件，并post一下system函数，通过ls查看目录

![image](https://github.com/user-attachments/assets/bdf3cb7c-db87-4b43-9748-f969e6805ac5)

直接使用cat查看flag目录下的文件，得到flag flag{TEST_Dynamic_FLAG}
![image](https://github.com/user-attachments/assets/803b39af-ecda-4257-8d86-2b694cb14f9a)

## Level 1
进入后提示使用file协议

![image](https://github.com/user-attachments/assets/4f2ada16-8d70-4d09-a4e3-b18c14cb43f6)

首先尝试直接访问，发现失败了，检索得知flag.php 中 flag以静态变量形式存储。由于并没有输出操作，它只会被加载到服务器或者说容器的内存中，这种形式的FLAG您无法通过单纯包含得到。所以无法通过flag.php文件获得。

![image](https://github.com/user-attachments/assets/383066b6-a138-416a-b87f-f36c3ca23a6a)

尝试访问根目录下文本形式的flag文件，得到flag

![image](https://github.com/user-attachments/assets/294cd057-d006-4b25-99e6-8c41d2372567)

## Level 2
这一关提示只能使用data协议

![image](https://github.com/user-attachments/assets/bd29f741-6b14-4c44-a5ac-b8ab47deb5f9)

先查看目录，直接访问，得到flag

![image](https://github.com/user-attachments/assets/3aa9da09-a40f-471f-9a5e-85fa2a785d20)

![image](https://github.com/user-attachments/assets/d17c6acc-c914-4545-ada1-ff3f724bc1d2)

## Level 3
这一关还是使用data协议，但是发现过滤掉了/flag|\~|\!|\@|\#|\\$|\%|\^|\&|\*|\(|\)|\-|\_|\+|\=|\

![image](https://github.com/user-attachments/assets/261fce22-6bad-45ea-81c3-d11745bb96a3)

可以通过Base64的输入形式进行绕过,直接将<?php system("cat /flag");?>编码

![image](https://github.com/user-attachments/assets/d4cea6c2-9216-4226-8cdb-cc75cd8b9bd2)

?wrappers=;base64,PD9waHAgc3lzdGVtKCJjYXQgL2ZsYWciKTs/Pg，得到flag

![image](https://github.com/user-attachments/assets/4ded4c59-932f-495f-93d9-b744447e3d9b)

## Level 4
进入后提示本关卡只能使用http/https 协议

![image](https://github.com/user-attachments/assets/0970147e-42b0-4cc9-a854-8a85b78de88c)

在大多数情况下，localhost 只是默认情况下引用 127.0.0.1 的简写，即本地主机.可以直接通过http/https 协议访问127.0.0.1/backdoor.txt（localhost/backdoor.txt也可以）

![image](https://github.com/user-attachments/assets/75553369-f2b7-4abd-8d00-1e34591ef513)

直接cat /flag即可

![image](https://github.com/user-attachments/assets/71b45716-5e18-45a7-a325-da7bcd2f8ace)

## Level 5
这一关还是使用http/https 协议，提示访问challenge.txt

![image](https://github.com/user-attachments/assets/155ae9c5-5469-4fdf-a590-a4245dc2d84f)

访问失败了，说明当前目录在不再有后门文件了,可以考虑使用远程文件包含

![image](https://github.com/user-attachments/assets/2dfaed63-799c-4970-8423-797f19e3286f)

尝试自己构建一个webshell但是不知道为什么没有运行,之后会用蚁剑再次尝试

这里直接使用作者的一句话木马进行远端包含，得到flag

![image](https://github.com/user-attachments/assets/f2dfc98e-26eb-46f6-8a65-22a70fc7a4a8)

## Level 6
这一关要求使用php://协议

![image](https://github.com/user-attachments/assets/9970b187-fcb8-46d7-9328-1b0fbca35e94)

这里选择用php://filter方式直接访问flag
php://filter/read=/resource=/flag

![image](https://github.com/user-attachments/assets/23bd4dd8-830f-4209-9d7f-5b7a0f1d28d5)

php://input没有成功

## Level 7
这一关要求用php://input
php://input作为include的直接参数时请求的参数格式是原生(Raw)的内容，无法使用hackbar提交，因为hackbar不支持raw方式
先访问php://input，先用bursuite打开代理

![image](https://github.com/user-attachments/assets/7d0314e5-2398-4612-bef4-7fe082318137)

发现抓完包里面是乱码，尝试失败了，php://input这个没有解出来，回头看一下大佬的

## Level 8
这一关要求使用php://filter，通过rot13变换访问flag.txt
?wrappers=filter/string.rot13/resource=/flag

![image](https://github.com/user-attachments/assets/abc44140-88fd-40ff-b757-900bd410a9b9)

## Level 9
本题也是php://filter，但是不存在文本格式的flag的了，只能用base64的转换滤器解决php文件

![image](https://github.com/user-attachments/assets/19326150-cf5e-45f6-af21-b025bfd6fe03)

之后进行解码即可得到flag

## Level 10
这道题使用file_get_contents() 函数，这个函数会将整个文件读入一个字符串,同样的他支持封装协议(wrappers)的使用
可以将这个看为include函数，然后直接输入?file=php://filter/string.rot13/resource=/flag

![image](https://github.com/user-attachments/assets/d6cb207f-ba7d-45f2-a3e4-bb175fd4aeba)

解码后即可得到flag

![image](https://github.com/user-attachments/assets/9d9025dc-10f8-4737-99e8-1db3de1e24fb)

## Level 11
这关考察_file_put_contents()函数，这将对应过滤器中的Write方法，用于将数据写入文件。

![image](https://github.com/user-attachments/assets/73fdee4c-9955-440a-bd94-285abe2e011d)

但是又失败了

