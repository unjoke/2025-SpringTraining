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

### php://filter
php://filter 是一种元封装器， 设计用于数据流打开时的筛选过滤应用。 这对于一体式（all-in-one）的文件函数非常有用，类似 readfile()、 file() 和 file_get_contents()， 在数据流内容读取之前没有机会应用其他过滤器。

简单通俗的说，这是一个中间件，在读入或写入数据的时候对数据进行处理后输出的一个过程。

php://filter可以获取指定文件源码。当它与包含函数结合时，php://filter流会被当作php文件执行。所以我们一般对其进行编码，让其不执行。从而导致任意文件读取。

resource=<要过滤的数据流> 这个参数是必须的。它指定了你要筛选过滤的数据流。
read=<读链的筛选列表> 该参数可选。可以设定一个或多个过滤器名称，以管道符（|）分隔。
write=<写链的筛选列表> 该参数可选。可以设定一个或多个过滤器名称，以管道符（|）分隔。
<；两个链的筛选列表> 任何没有以 read= 或 write= 作前缀 的筛选器列表会视情况应用于读或写链。

常用：
```
php://filter/read=convert.base64-encode/resource=index.php
php://filter/resource=index.php
```
利用filter协议读文件，将index.php通过base64编码后进行输出。这样做的好处就是如果不进行编码，文件包含后就不会有输出结果，而是当做php文件执行了，而通过编码后则可以读取文件源码。而使用的convert.base64-encode，就是一种过滤器。

### php://input
php://input可以访问请求的原始数据的只读流，将post请求的数据当作php代码执行。当传入的参数作为文件名打开时，可以将参数设为php://input,同时post想设置的文件内容，php执行时会将post内容当作文件内容。从而导致任意代码执行。

php.ini 中的 allow_url_include设置为On 才能成功执行

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

```
示例用法：
1、data://text/plain,
http://127.0.0.1/include.php?file=data://text/plain,<?php%20phpinfo();?>
 
2、data://text/plain;base64,
http://127.0.0.1/include.php?file=data://text/plain;base64,PD9waHAgcGhwaW5mbygpOz8%2b


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

