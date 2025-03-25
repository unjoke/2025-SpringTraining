首先得我的狡辩一下为啥交的这么晚，因为上回我的md里面的图片采用相对路径的方式去显示，效果并不好，所以我跑去研究图床，一步步按照网上的教程去做，一直上传失败，不断地调整设置，弄了4个多小时，不见效果，已经是给我气笑了，然后我破罐子破摔式的重启了机器，

然后，它，他妈的传上去了！

研究了一下，应该是这玩意的锅，我习惯用这个去打开steam和github

这下问题解决了，唉，菜是原罪

补充一下，我把笔记pr到本地仓库里面，图片还是显示不出来

弄了大半天，白干了.........

只能自我安慰，图片传到自己的仓库里，算是一种纪念吧

除此之外，基础太差，想要弄懂每道题的原理效率很低。

## 拓展的配置--hackbar

首先鼠鼠要看一下hackbar是怎么用的，因为鼠鼠更倾向于使用burpsuit，下载两个免费版本的hackbar,一个无法使用就切另一个

![在这里插入图片描述](https://raw.githubusercontent.com/30-STEPh/photos/main/20250324012154214.png)

csdn给出了详细的介绍

SQL：提供三种数据库的sql查询语句，以及一些方便联合查询的语句
XSS：提供xss攻击语句
string.fromcharcode()：将根据UNICODE 值来输出xss语句
html charactor ： 将XSS语句转化为HTML字符实体（以&开头）
alert(xss) statement : 构建一条xss测试语句，弹出一个框内容为xss，相当于alert(‘xss’);
Encryption：对所选字符进行加密，提供了MD5，SHA-1，SHA-256，ROT13等加密方式
Encoding：对所选字符进行编码解码，提供了Base64 Encode,Base64 Decode,URLencode,URLdecode,
HEX encoding, HEX decoding等方式
Other：
addslashes：在每个双引号前加反斜杠
stripslashes：除去所选字符中的反斜杠
strip space：除去所选字符中的空格
reverse：将所选字符倒序排列
usefull strings：提供了一些特殊的数值如圆周率PI,斐波那契数列等，其中buffer overflow 可以输入一定长度的字符造成缓存溢出攻击
大致是这样，以后会细细探究。

## [LitCTF 2023]Follow me and hack me--burpsuit
![image-20250318173723058](https://github.com/user-attachments/assets/8f753230-6476-4e96-ab89-15df88a3127e)


![image-20250318173723058](https://raw.githubusercontent.com/30-STEPh/photos/main/20250324012200368.png)

神经。。。。。。。。。。。。

![image-20250318184637559](https://raw.githubusercontent.com/30-STEPh/photos/main/20250324012205470.png)
![image-20250318184637559](https://github.com/user-attachments/assets/f9fed70f-f08d-4d50-8b05-e301eb71c0fd)

尝试向里面传参数， 在URL中使用?符号将参数附加到URL末尾，多个参数之间使用&符号分隔。

使用POST提交方法和GET类似，将GET改为POST，此时记得添加Content-Type: application/x-www-form-urlencoded

![image-20250318184735320](https://raw.githubusercontent.com/30-STEPh/photos/main/20250324012209739.png)

![image-20250318185055295](https://raw.githubusercontent.com/30-STEPh/photos/main/20250324012214290.png)

## [SWPUCTF 2021 新生赛]Do_you_know_http

![image-20250319214553466](https://raw.githubusercontent.com/30-STEPh/photos/main/20250324012219002.png)

![image-20250319215015202](https://raw.githubusercontent.com/30-STEPh/photos/main/20250324012221966.png)

![image-20250319215631798](https://raw.githubusercontent.com/30-STEPh/photos/main/20250324012228667.png)

修改user agent为WLLM   execute一下

![image-20250319220536064](https://raw.githubusercontent.com/30-STEPh/photos/main/20250324012225307.png)

不难发现，我们要伪造一个用WLLM浏览器的进入方式，即修改user agent,得到一个a.php的文件，这个文件只能在本地打开，我们要在<font color='green'>本地回环地址</font>中打开，得到答案。

## [LitCTF 2023]就当无事发生

![image-20250323153416596](https://raw.githubusercontent.com/30-STEPh/photos/main/20250324012232690.png)

进入了一个网站，不明白这道题想干嘛。

进入他的github主页也没找到上面的flag文件，但是找到了不少index文件，也看到了这位大佬的学习与工作过程。





## [SWPUCTF 2021 新生赛]easyrce

![image-20250324231237778](C:\Users\ASUS\Desktop\webs\week2\20250324231245020.png)



<font color='green'>error_reporting</font>函数的主要作用是设置PHP应该报告哪些类型的错误。格式为error_reporting(report_level)
参数代表错误报告的级别。

<font color='red'>isset() </font>函数用于检测变量是否已设置并且非 NULL。如果指定变量存在且不为 NULL，则返回 TRUE，否则返回 FALSE。

<font color='blue'>$_GET</font> 变量,从带有 GET 方法的表单发送的信息,会显示在浏览器的地址栏

<font color='orange'>eval()</font> 函数把字符串按照 PHP 代码来计算。

总得来说就是一个叫url的可以在网址中显示的变量，如果得到他的值不为空，就按php语法执行eval函数的语句,获取 **URL 查询参数** 的超全局变量（superglobal）。  返回url的值,这个时候可以通过url执行命令，找出文件的目录,通过system执行外部命令,**列出根目录内容，采用ls /**但是这是一个url里面执行的命令，直接输入**空格**会报错，因此空格经过**url编码**之后变成**%20**输入命令，可以看到一个可疑文件flag

![image-20250324232223206](C:\Users\ASUS\Desktop\webs\week2\20250324232223261.png)

选择查看这个文件，利用cat查看文件内容

![image-20250324232620671](C:\Users\ASUS\Desktop\webs\week2\20250324232620881.png)



![image-20250324232639303](C:\Users\ASUS\Desktop\webs\week2\20250324232639403.png)



## [SWPUCTF 2022 新生赛]奇妙的MD5

![image-20250324233115790](C:\Users\ASUS\Desktop\webs\week2\20250324233115841.png)

奇妙的字符串，又跟 MD5 有关

**一个是 [MD5 加密](https://so.csdn.net/so/search?q=MD5 加密&spm=1001.2101.3001.7020)后弱比较等于自身，这个字符串是 0e215962017** 

**另一个是 MD5 加密后变成万能密码，这个字符串是 ffifdyop**

![image-20250324233321694](C:\Users\ASUS\Desktop\webs\week2\20250324233321868.png)

![image-20250324233629927](C:\Users\ASUS\Desktop\webs\week2\20250324233630179.png)

右键看看源代码

![image-20250324234104113](C:\Users\ASUS\Desktop\webs\week2\20250324234104344.png)

首先从url的查询参数中获取x和y的值，再进行判断，xy值不等而且xy的md5值要相等时if成立

根据wp的提示，这是一个弱比较，方法包括且不仅限于使用数组绕过，传参?x[]=1&y[]=2

![image-20250325000402121](C:\Users\ASUS\Desktop\webs\week2\20250325000402205.png)

![image-20250325000424943](C:\Users\ASUS\Desktop\webs\week2\20250325000424987.png)

![image-20250325030446648](C:\Users\ASUS\Desktop\webs\week2\20250325030446690.png)

![image-20250325031936192](C:\Users\ASUS\Desktop\webs\week2\20250325031936239.png)

## [BJDCTF 2020]easy_md5

![image-20250325104552440](C:\Users\ASUS\Desktop\webs\week2\20250325104552633.png)

输入没什么反应，源码也没啥好看的，使用bp抓包试试

![image-20250325104856293](C:\Users\ASUS\Desktop\webs\week2\20250325104856358.png)

不做什么特别操作，随便按一下send，找到一个hint

hint: select * from 'admin' where password=md5($pass,true)

需要password=md5($pass,true)条件为真时，才会执行select * form admin

md5()函数会将我们输入的值，加密，然后转换成16字符的二进制格式，

由于ffifdyop被md5加密后  276f722736c95d99e921722cf9ed621c转换成16位原始二进制格式为'or’6\xc9]\x99\xe9!r,\xf9\xedb\x1c，这个字符串前几位刚好是' or '6

在mysql内，用作布尔型判断时，以1开头的字符串会被当做整型数。要注意的是这种情况是必须要有单引号括起来的，比如password=' or '1xxxx'，那么就相当于password=' or 1，所以返回值就是true

传入ffifdyop后相当于select * from admin where password=''or 1 

与之类似的还有129581926211651571912466741651878684928

这个[字符串转换](https://so.csdn.net/so/search?q=字符串转换&spm=1001.2101.3001.7020)后为�T0D��o#��'or'8

![image-20250325110017779](C:\Users\ASUS\Desktop\webs\week2\20250325110018000.png)

看看源码

![image-20250325110119468](C:\Users\ASUS\Desktop\webs\week2\20250325110119526.png)

if($a != $b && md5($a) == md5($b))

{header('Location: levell14.php');

保证ab不等且md5相等,弱类型比较。

QNKCDZO（0e830400451993494058024219903391）

![image-20250325111504228](C:\Users\ASUS\Desktop\webs\week2\20250325111504427.png)

post的param1和param2不能相等，然后强比较，

![image-20250325112210068](C:\Users\ASUS\Desktop\webs\week2\20250325112210272.png)

重新load以后，利用数组绕过，构造，传参

## [第五空间 2021]WebFTP

![image-20250325113618021](C:\Users\ASUS\Desktop\webs\week2\20250325113618176.png)

随意地输入一些东西进去，没啥效果，源码也没什么特别的，根据提示，用dirsearch扫描后台

![image-20250325114241341](C:\Users\ASUS\Desktop\webs\week2\20250325114241420.png)





里面有一个叫phpinfo的，进去看看。

![image-20250325114448136](C:\Users\ASUS\Desktop\webs\week2\20250325114448274.png)



这就进来了，懒得找flag,ctrl f直接搜。

![image-20250325114620693](C:\Users\ASUS\Desktop\webs\week2\20250325114620900.png)







## 一些知识的补充

### 本地回环地址

回环地址并非只有一个，所有127开头的都是回环地址。

而***<font color='red'>本地</font>回环地址***127.0.0.1指的是本机地址，不会跟着网络情况的变化而变化。它代表设备的本地虚拟接口，所以默认被看作是永远不会宕掉的接口。(实际上：127.0.0.1 —> 127.255.255.254（去掉0和255） 的范围都是本地回环地址)

**它的作用是**计算机以回环地址发送的消息，并不会由链路层送走，而是被本机网络层捕获。用处只有一个，就是自己发给自己，自娱自乐。

### **X-Forwarded-For**（**XFF**）

**X-Forwarded-For**（**XFF**）是用来识别通过[HTTP](https://baike.baidu.com/item/HTTP/0?fromModule=lemma_inlink)[代理](https://baike.baidu.com/item/代理/0?fromModule=lemma_inlink)或[负载均衡](https://baike.baidu.com/item/负载均衡/0?fromModule=lemma_inlink)方式连接到[Web服务器](https://baike.baidu.com/item/Web服务器/0?fromModule=lemma_inlink)的客户端最原始的[***IP地址***](https://baike.baidu.com/item/IP地址/0?fromModule=lemma_inlink)的<font color='red'>HTTP请求头字段。</font>

#### 用法

一般格式如下:

**<font color='red'>X-Forwarded-For: client1, proxy1, proxy2, proxy3</font>**

***其中的值通过一个 逗号+空格 把多个IP地址区分开***

最左边(client1)是**最原始客户端的IP地址**, 代理服务器每成功收到一个请求，就把**请求来源IP地址**添加到右边。 

在上面这个例子中，这个请求成功通过了三台代理服务器：proxy1, proxy2 及 proxy3。请求由client1发出，到达了proxy3(proxy3可能是请求的终点)。

请求刚从client1中发出时，XFF是空的，请求被发往proxy1；通过proxy1的时候，client1被添加到XFF中，之后请求被发往proxy2;通过proxy2的时候，proxy1被添加到XFF中，之后请求被发往proxy3；通过proxy3时，proxy2被添加到XFF中，之后请求的的去向不明，如果proxy3不是请求终点，请求会被继续转发。

鉴于伪造这一字段非常容易，应该谨慎使用X-Forwarded-For字段。正常情况下XFF中最后一个IP地址是最后一个代理服务器的IP地址, 这通常是一个比较可靠的信息来源。

### Index

我们先来理解下 Git 工作区、暂存区和版本库概念：

- **工作区：**就是你在电脑里能看到的目录。
- **暂存区：**英文叫 stage 或 index。一般存放在 **.git** 目录下的 index 文件（.git/index）中，所以我们把暂存区有时也叫作索引（index）。
- **版本库：**工作区有一个隐藏目录 **.git**，这个不算工作区，而是 Git 的版本库。

![img](https://raw.githubusercontent.com/30-STEPh/photos/main/20250324012236318.jpg)

### git泄露

#### git是什么

**Git**是一个开源的分布式版本控制系统，它能够高效地处理从小到大的项目。Git的核心功能是版本控制，它允许你跟踪文件的变化，保存不同版本的文件，并且在需要的时候可以回退到任何一个历史版本。这在软件开发中尤其有用，因为它允许多人协作开发同一个项目，同时保持各自的开发进度和变更记录。

**本地仓库**：是在开发人员自己电脑上的Git仓库
**远程仓库**：是在远程服务器上的Git仓库

Clone：克隆，就是将远程仓库复制到本地
Push：推送，就是将本地仓库代码上传到远程仓库
Pull：拉取，就是将远程仓库代码下载到本地仓库

### md5强比较，弱比较

#### md5万能密码为什么万能

**ffifdyop**，这个字符串经过[md5加密](https://so.csdn.net/so/search?q=md5加密&spm=1001.2101.3001.7020)后为276f722736c95d99e921722cf9ed621c

在转为字符串为'or'6�]��!r,��b，传入ffifdyop后相当于**select * from admin where password=''or 1** 

与之类似的还有**129581926211651571912466741651878684928**

这个[字符串转换](https://so.csdn.net/so/search?q=字符串转换&spm=1001.2101.3001.7020)后为**�T0D��o#��'or'8**

#### 弱类型

0e绕过md5($a)==md5($b)&&$a!=$b

原理是php在处理时会将0e开头的字符串理解为0，例如

QNKCDZO
QLTHNDT
s214587387a
s878926199a
s155964671a

#### 强类型

数组绕过md5($a)===md5($b)&&$a!==$b

原理：md5对数组加密结果为NULL

传入a[]=1&b[]=2即可绕过，数组绕过也可用于弱类型
