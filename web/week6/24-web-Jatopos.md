# 文件上传-学习笔记
**绕过方式：**

1.如果没有过滤图片格式

先上传.jpg文件再通过burpsuite改为.php，蚁剑连接

如果被$deny_ext过滤了，可以改为.php3等格式

2.如果没有过滤<font style="color:rgb(77, 77, 77);">.htaccess文件</font>

<font style="color:rgb(77, 77, 77);">先导入.htaccess文件，再导入.jpg文件</font>

<font style="color:rgb(77, 77, 77);"></font>

<font style="color:rgb(77, 77, 77);">3.如果</font>**<font style="color:rgb(77, 77, 77);">没有</font>**<font style="color:rgb(77, 77, 77);">函数 $file_ext =</font>**<font style="color:rgb(77, 77, 77);"> strtolower</font>**<font style="color:rgb(77, 77, 77);">($file_ext); //转换为小写</font>

<font style="color:rgb(77, 77, 77);">可以采用大小写绕过的方法：cmd.PHP</font>

<font style="color:rgb(77, 77, 77);"></font>

<font style="color:rgb(77, 77, 77);">4.如果</font>**<font style="color:rgb(77, 77, 77);">没有</font>**<font style="color:rgb(77, 77, 77);">函数 $file_ext = </font>**<font style="color:rgb(77, 77, 77);">trim</font>**<font style="color:rgb(77, 77, 77);">($file_ext); //首尾去空</font>

<font style="color:rgb(77, 77, 77);">文件名后缀加空格绕过：'cmd.php '</font>

<font style="color:rgb(77, 77, 77);"></font>

<font style="color:rgb(77, 77, 77);">5.如果</font>**<font style="color:rgb(77, 77, 77);">没有</font>**<font style="color:rgb(77, 77, 77);">函数 $file_name = </font>**<font style="color:rgb(77, 77, 77);">deldot</font>**<font style="color:rgb(77, 77, 77);">($file_name);//删除文件名末尾的点</font>

<font style="color:rgb(77, 77, 77);">在文件格式后面加点. ：cmd.php.</font>

<font style="color:rgb(77, 77, 77);">Windows会将 </font>`<font style="color:rgb(77, 77, 77);">cmd.php.</font>`<font style="color:rgb(77, 77, 77);"> 解释为没有扩展名的文件，因为末尾的点会被忽略。文件的实际名称会变成 </font>`<font style="color:rgb(77, 77, 77);">cmd.php</font>`<font style="color:rgb(77, 77, 77);">，但扩展名会被视为不存在</font>

<font style="color:rgb(77, 77, 77);"></font>

<font style="color:rgb(77, 77, 77);">6.如果</font>**<font style="color:rgb(77, 77, 77);">没有</font>**<font style="color:rgb(77, 77, 77);">函数 $file_ext = </font>**<font style="color:rgb(77, 77, 77);">str_ireplace</font>**<font style="color:rgb(77, 77, 77);">('::$DATA', '', $file_ext);//去除字符串::$DATA</font>

<font style="color:rgb(77, 77, 77);">上传PHP一句话文件，抓包改后缀cmd.php::$DATA  
</font><font style="color:rgb(77, 77, 77);">然后使用蚁剑连接cmd.php</font> `（注意蚁剑连接路径不要加上::$DATA）`

在window的时候如果文件名+`"::$DATA"`会把`::$DATA`之后的数据当成文件流处理,不会检测后缀名，且保持`::$DATA`之前的文件名，他的目的就是不检查后缀名

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"></font>

7.如果**有**函数 $file_name = **deldot**($file_name);//删除文件名末尾的点

可以构造cmd.php. .

deldot()函数从后向前检测，当检测到末尾的第一个点时会继续它的检测，即从字符串的尾部开始，从后向前删除点`.`，直到该字符串的末尾字符不是`.`为止但是遇到空格会停下来

deldot()函数遇到空格会停止



8.如果**有**函数 $file_name = **str_ireplace**($deny_ext,"", $file_name); //必须替换成空格

传`cmd.php` 然后用bp改后缀为`.pphphp`

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"></font>

<font style="color:rgb(79, 79, 79);">9.00截断——</font>如果**有**函数：(get和post同理)

```plain
$img_path = $_GET['save_path']."/".rand(10, 99).date("YmdHis").".".$file_ext;
 
if(move_uploaded_file($temp_file,$img_path)){

或

$img_path = $_POST['save_path']."/".rand(10, 99).date("YmdHis").".".$file_ext;
 
if(move_uploaded_file($temp_file,$img_path)){
```

含有函数move_uploaded_file 且白名单中最终文件的存放位置是以拼接的方式

**get方式：**

上传`cmd.png`用BP抓包修改参数，把upload/后面加上`cmd.php%00`

**post方式：**

上传`cmd.png`用BP抓包修改参数，在`../upload/` 路径下加上`cmd.php+` `+号`是为了方便后面修改`Hex`

`+号`的Hex是`2b`，这里我们要把它改为`00`



10.图片马绕过

图片字节标识：

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744823413667-c311aca4-2f38-423a-a7b6-6721d5edb4b4.png)

如果含有以下代码，说明检查字节标识

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744827833746-f12ca2f0-9152-4c5f-9b83-3f8d23f6fbda.png)

或

```plain
getimagesize()
或
exif_imagetype()
```

有以下两种方法绕过：

**法一：字节标识绕过**

将.php改为.jpg后用记事本打开，在一句话木马前加上占位符aa

用010editor打开，修改头两个字节为jpg对应的FF D8,保存一下

上传后，用文件包含刚上传的文件，比如?file=./upload/8888.jpg

用蚁剑连接

**法二：图片马绕过**

通过cmd生成图片马 使用 copy normal.jpg/b + cmd.php tupianma.jpg 制作图片马

上传后，用文件包含刚上传的文件，用蚁剑连接



11.<font style="color:rgb(79, 79, 79);">二次渲染绕过</font>

<font style="color:rgb(79, 79, 79);">如果出现以下代码，需要使用二次渲染绕过</font>

```plain
二次渲染：后端重写文件内容
basename(path[,suffix]) ，没指定suffix则返回后缀名，有则不返回指定的后缀名
strrchr(string,char)函数查找字符串在另一个字符串中最后一次出现的位置，并返回从该位置到字符串结尾的所有字符。
imagecreatefromgif()：创建一块画布，并从 GIF 文件或 URL 地址载入一副图像
imagecreatefromjpeg()：创建一块画布，并从 JPEG 文件或 URL 地址载入一副图像
imagecreatefrompng()：创建一块画布，并从 PNG 文件或 URL 地址载入一副图像

重写后会删去原图片马中的php部分，所以需要找到渲染后的图片里面没有发生变化的Hex地方，添加一句话，通过文件包含漏洞执行一句话

一般通过.gif格式文件执行
```

将上传重写后的gif下载，通过010里面的比较工具对比（在tools里面）

点击match，随便找一个公共部分插入木马即可

直接上传然后进行文件包含，蚁剑连接



12.条件竞争——burpsuite爆破（参考P17\P18）

# upload-labs
## Pass-01
提交得知需要上传图片类型文件

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744705276762-9b9aa345-90c9-4ba3-9405-b5fca4329a49.png)

改为png上传成功

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744705413822-f24cf3e5-ad79-4d76-a81e-565638b6f883.png)

用burpsuite抓包并改格式为php

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744705471350-06cc9b59-14d4-4ff6-8426-73b4a8d63acd.png)

打开该文件

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744705974448-3e8228fc-1405-458a-99db-e22049ebd510.png)

蚁剑连接，成功

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744706006487-08fa86f3-5b62-4a59-bf68-35ef58eec0b0.png)



## Pass-02
第二关同第一关

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744706178383-7b15bdd2-3d90-48cb-a7ff-c321d2bd81c7.png)

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744706463636-b9c99ac5-c06d-4b6e-94c6-156183ba0770.png)

## Pass-03
第三关就不能修改文件格式为.php了，因为被过滤掉了

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744706523357-67b3049e-725b-45f0-81b7-391450994bfc.png)

<font style="color:rgb(77, 77, 77);">.php3 、Phtml等都可以被解析，改成.php3即可</font>

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744708015422-b0405819-ec97-4af6-804a-c87716b7354e.png)

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744707953622-838ae753-46ac-4acb-af76-c30b51e5fc2e.png)

## Pass-04
本关过滤很多文件格式 .php|.php5|.php4|.php3|.php2|php1|.html|.htm|.phtml|.pHp|.pHp5|.pHp4|.pHp3|.pHp2|pHp1|.Html|.Htm|.pHtml|.jsp|.jspa|.jspx|.jsw|.jsv|.jspf|.jtml|.jSp|.jSpx|.jSpa|.jSw|.jSv|.jSpf|.jHtml|.asp|.aspx|.asa|.asax|.ascx|.ashx|.asmx|.cer|.aSp|.aSpx|.aSa|.aSax|.aScx|.aShx|.aSmx|.cEr|.sWf|.swf后缀文件！  

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744708056702-bde6f305-812f-47f4-8db9-cbbefb285f19.png)

可以使用<font style="color:rgb(77, 77, 77);">.htaccess文件，该文件会将后来上传的jpg文件解析成php，shell代码得以执行</font>

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744708549643-27f525e2-13a4-48c3-bb4c-bdef7c7dd303.png)

<font style="color:rgb(77, 77, 77);">先上传.htaccess文件，再上传.jpg文件，连接成功</font>

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744708611004-c744e387-8f6b-4b6f-99e1-b08523b46d63.png)

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744708651511-55e1ddd9-cfe7-4905-8603-16fc7a77fca7.png)

## Pass-05
第五关把.htaccess也过滤了

<font style="color:rgb(77, 77, 77);">没有使用strtolower()函数，可以使用大小写绕过黑名单,</font>可以采用大小写绕过的方法:cmd.PHP

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744709757669-4684f3a1-96fe-4f29-9220-6be653027b3d.png)

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744709778211-ec5037fe-33ff-474d-8df6-697d890c59aa.png)

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744708992125-b8859284-16f5-4935-a209-384cff2d6ba1.png)

<font style="color:rgb(77, 77, 77);">文件后缀大写绕过</font>

<font style="color:rgb(77, 77, 77);">上传.php文件</font>

<font style="color:rgb(77, 77, 77);">修改后缀名为.PHP</font>

<font style="color:rgb(77, 77, 77);">上传成功</font>

<font style="color:rgb(77, 77, 77);">解析执行</font>

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744709354318-4c5f2d52-3d13-4d73-838c-8e199ef5ae85.png)

## Pass-06
<font style="color:rgb(77, 77, 77);">这一关黑名单，没有使用trim()去除空格，可以使用空格绕过黑名单</font>

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744710505898-06f96309-791b-4fdd-a3d6-077de9720d77.png)

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744710523818-895df08f-b628-4050-afac-db99626f354b.png)

文件名后缀加空格绕过：'cmd.php '

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744727132701-6b31262a-3279-497d-b590-55a7cdf2f31a.png)

连接成功

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744727188987-dd5c3154-d3a9-4e8c-8b78-2ad478967028.png)

## Pass-07
<font style="color:rgb(77, 77, 77);">这一关黑名单，没有使用deldot()过滤文件名末尾的</font>`<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">点</font>`<font style="color:rgb(77, 77, 77);">，可以使用文件名后加.进行绕过</font>

<font style="color:rgb(77, 77, 77);">cmd.php.</font>

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744727410465-eb77b3ed-8626-497f-b70e-2dd7ef85aedb.png)

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744727586255-439a3f57-9213-4f44-8180-2c30438cdd47.png)

连接成功

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744727615439-5ee5c5f7-4715-418f-a358-dcf857d8411e.png)

## Pass-08
这一关黑名单，没有对::D A T A 进 行 处 理 ， 可 以 使 用::DATA进行处理，可以使用::DATA绕过黑名单

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744728152408-7b7a0007-c9f4-40b7-a079-91baad1ce206.png)

<font style="color:rgb(77, 77, 77);">上传PHP一句话文件，抓包改后缀cmd.php::$DATA</font>  
<font style="color:rgb(77, 77, 77);">然后使用蚁剑连接cmd.php </font>`<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">（注意蚁剑连接路径不要加上::$DATA）</font>`

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744728426230-aceef439-96dd-4544-a5fa-07f0c997c919.png)

连接成功**（注意蚁剑连接路径不要加上::$DATA）**

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744728472490-c013d9e7-88c3-4bb2-b252-8d94138243de.png)

## Pass-09
<font style="color:rgb(77, 77, 77);">filename=deldot(file_name)操作去除文件名末尾的点，构造后缀绕过黑名单</font>

deldot()函数从后向前检测，当检测到末尾的第一个点时会继续它的检测，即从字符串的尾部开始，从后向前删除点`.`，直到该字符串的末尾字符不是`.`为止但是遇到空格会停下来

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744730216450-12b57c85-932f-490e-876c-442145ea743d.png)

<font style="color:rgb(77, 77, 77);">更改文件名为 index.php. . 即.php点 空格 点</font>

<font style="color:rgb(77, 77, 77);">运行php时，首先删除文件名末尾的点，</font>

<font style="color:rgb(77, 77, 77);">最后一步检查，去除最后的空格，</font>

<font style="color:rgb(77, 77, 77);">此时文件名为index.php.</font>

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744730728583-f4c2bf29-b518-4c4b-b75d-ee873223eaad.png)

连接成功

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744730777839-e04a62e9-c586-4248-997d-7bbaecc20030.png)

## Pass-10
<font style="color:rgb(77, 77, 77);">使用str_ireplace()函数寻找文件名中存在的黑名单字符串，将它替换成空（即将它删掉），可以使用双写绕过黑名单</font>

str_ireplace(find,replace,string,count) 函数替换字符串中的一些字符（不区分大小写）

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744733180539-d2f9a063-b973-460d-99de-a3ffb14962f8.png)

<font style="color:rgb(77, 77, 77);">传</font>`<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cmd.php</font>`<font style="color:rgb(77, 77, 77);"> 然后用bp改后缀为</font>`<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">.pphphp</font>`<font style="color:rgb(77, 77, 77);">使用蚁剑连接</font>`<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cmd.php</font>`

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744733420256-54543760-c758-49ab-a98c-59ea364ba7a2.png)

连接成功

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744733379585-498d10be-03f0-4161-9b3d-df3d8d39d667.png)

## Pass-11
<font style="color:rgb(79, 79, 79);">思路——00截断问题（只存在于php5.2版本中）——get传参（url编码方式）</font>

```plain
$is_upload = false;
$msg = null;
if(isset($_POST['submit'])){
    $ext_arr = array('jpg','png','gif');
    
    //$_FILES['upload_file']['name']：获取上传文件的原始文件名
    //strrpos($_FILES['upload_file']['name'], ".")：查找文件名中最后一个点（.）的位置
    //substr(..., strrpos(...) + 1)：从最后一个点的下一个字符开始截取，得到文件扩展名
    $file_ext = substr($_FILES['upload_file']['name'],strrpos($_FILES['upload_file']['name'],".")+1);
    if(in_array($file_ext,$ext_arr)){
        
        //$_FILES['upload_file']['tmp_name']：获取上传文件的临时存储路径
        $temp_file = $_FILES['upload_file']['tmp_name'];
        
        //$_GET['save_path']：从 URL 参数中获取保存路径
        //rand(10, 99)：生成一个 10 到 99 之间的随机数
        //date("YmdHis")：生成当前日期时间字符串，格式为 年月日时分秒
        //最终路径格式：保存路径/随机数+时间戳.扩展名
        $img_path = $_GET['save_path']."/".rand(10, 99).date("YmdHis").".".$file_ext;
 
        if(move_uploaded_file($temp_file,$img_path)){
            $is_upload = true;
        } else {
            $msg = '上传出错！';
        }
    } else{
        $msg = "只允许上传.jpg|.png|.gif类型文件！";
    }
}
```

<font style="color:rgb(77, 77, 77);">这</font>一关白名单，最终文件的存放位置是以拼接的方式，可以使用%00截断，但需要`php版本<5.3.4`，并且`magic_quotes_gpc`关闭。

<font style="color:rgb(68, 68, 68);background-color:rgb(249, 249, 249);">move_uploaded_file</font><font style="color:rgb(17, 17, 17);"> 函数用于将上传的文件从临时目录移动到指定的新位置。此函数在确保文件是通过 HTTP POST 上传后，将其移动到新位置。</font>

<font style="color:rgb(77, 77, 77);">php的一些函数的底层是C语言，而move_uploaded_file就是其中之一，遇到0x00会截断，0x表示16进制，URL中%00解码成16进制就是0x00</font>

<font style="color:rgb(77, 77, 77);">使用%00使截断代码，使后面代码不再运行</font>

<font style="color:rgb(77, 77, 77);">当一个字符串中存在空字符的时候，在被解析的时候会导致空字符后面的字符被丢弃</font>

<font style="color:rgb(77, 77, 77);">路径变成了/upload/cmd.php 后面的连接字符都被丢弃了，</font>

<font style="color:rgb(77, 77, 77);">之后将文件上传文件cmd.png的文件内容移动到cmd.php中，这样恶意代码就可以被解析了</font>

<font style="color:rgb(77, 77, 77);"></font>

<font style="color:rgb(77, 77, 77);">上传</font>`<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cmd.php</font>`<font style="color:rgb(77, 77, 77);">用BP抓包修改参数，把upload/后面加上</font>`<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cmd.php%00</font>`<font style="color:rgb(77, 77, 77);">,下面的</font>`<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">filename=”cmd.php”</font>`<font style="color:rgb(77, 77, 77);">改为</font>`<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cmd.png</font>`

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744735513392-f4b9ed5c-5d71-44b2-a442-19c7d600d6d5.png)

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744735533257-56c96874-538b-4dc6-bd7c-20015da8f661.png)

<font style="color:rgb(77, 77, 77);">这里有一个细节，</font>`**蚁剑连接时要把下面蓝色的部分删掉**`

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744735589221-23612f75-98fa-421a-91dd-7e241e784cfb.png)

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744735627668-b753ce0c-0e3a-45b1-93ce-5d341379e502.png)

## Pass-12
思路——00截断——save_path是post传参

这一关白名单，文件上传路径拼接生成，而且使用了post发送的数据进行拼接，我们可以控制post数据进行0x00截断绕过白名单

POST不会对里面的数据自动解码，需要在Hex中修改。



上传`cmd.php`用BP抓包修改参数，然后修改后的结果为如下

在`../upload/` 路径下加上`cmd.php+` `+号`是为了方便后面修改`Hex`

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744736568930-349e3430-b722-4adc-bc6b-d1f7b4835993.png)

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744736578140-f08567fa-28c4-4792-9e44-9464082a65dc.png)

`+号`的Hex是`2b`，这里我们要把它改为`00`

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744736601282-6f3b02ff-276c-4f27-99a1-3d4378e9e9d3.png)

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744736612680-66418fd0-c1d2-4df8-a79f-ff39244ebf59.png)

<font style="color:rgb(77, 77, 77);">然后就可以放包了，复制图片地址并用蚁剑进行连接(注意删掉后面多余部分)，连接成功</font>

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744736650932-c084ad2f-6351-4a1d-b754-e6b6e56e3165.png)

## Pass-13
这关设置了限制， 会检查图标内容开头2个字节，所以通过burpsuite抓包改后缀的方式就不可行了，有以下两种方法  

### 法一：字节标识绕过
![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744823413667-c311aca4-2f38-423a-a7b6-6721d5edb4b4.png)

将.php改为.jpg后用记事本打开，在一句话木马前加上占位符aa

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744824040168-c4a02503-2c5d-477f-8652-eba00cb9ea6c.png)

用010editor打开

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744824138102-1cd40720-ff13-4fb1-9aed-acd0158f8216.png)

修改头两个字节为jpg对应的FF D8,保存一下

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744824192201-eeb7d290-f2fd-4222-9038-6cdbf6603160.png)

直接上传，成功了

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744824226814-422f7286-b9a7-4412-b27b-95a224e988cd.png)

但蚁剑无法连接，说明解析为jpg格式了

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744824271051-61704475-b08e-49d1-947d-df8325d8b115.png)

这时候要用文件包含，会直接执行.jpg中的php语句

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744824432343-6f977ede-9578-4466-a636-04ef817b2be3.png)

此时再连蚁剑就成功了

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744824500028-fd8838bb-56d6-4997-a2d0-976ae68942c9.png)

### 法二：图片马绕过
通过cmd生成图片马 使用 `copy normal.jpg/b + cmd.php tupianma.jpg` 制作`图片马`

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744824789015-cb9bd5e7-f746-45c4-aec8-d6f459aed30a.png)

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744825621768-df740c92-709e-4550-9254-998131cfa7d8.png)

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744825648254-26cd215f-59d5-4e68-bf92-f45e1f105208.png)

直接上传图片马，上传成功

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744825662340-463e38e9-fc13-456c-a909-a225f0db37b1.png)

但此时解析为jpg格式，还需要使用文件包含，将路径get上去

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744825713418-258176c6-d0f9-446f-864b-b0de2dcb5a6e.png)

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744825681560-84a64994-bcf1-4a73-be36-0dd3e15f9d6d.png)

蚁剑连接，成功

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744825734946-7631d083-3c63-4397-8e4f-1c0a87662ab3.png)

`.png`,`.gif`与`.jpg`方法一样

## Pass-14
这关用<font style="color:rgb(77, 77, 77);">getimagesize()检查是否为图片文件，所以还是可以用上一关的两种方法绕过，以图片马为例</font>

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744825981986-332f83a3-89a8-4197-9434-de0967f1528c.png)

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744826106992-dffc471c-ebcc-489d-a237-a4efeb7c4e4d.png)

## Pass-15
本关用exif_imagetype()读取一个图像的第一个字节并检查其后缀名。同样可以用图片马

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744826249627-a648bef9-0daf-4f51-adae-6208e17c7bf9.png)

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744826265160-b887c3a2-cf7e-4eab-82a6-d90afa48298f.png)

## Pass-16
> 补充知识：
>
> 二次渲染：后端重写文件内容
>
> basename(path[,suffix]) ，没指定suffix则返回后缀名，有则不返回指定的后缀名
>
> strrchr(string,char)函数查找字符串在另一个字符串中最后一次出现的位置，并返回从该位置到字符串结尾的所有字符。
>
> imagecreatefromgif()：创建一块画布，并从 GIF 文件或 URL 地址载入一副图像
>
> imagecreatefromjpeg()：创建一块画布，并从 JPEG 文件或 URL 地址载入一副图像
>
> imagecreatefrompng()：创建一块画布，并从 PNG 文件或 URL 地址载入一副图像
>

重写后会删去原图片马中的php部分，所以<font style="color:rgb(77, 77, 77);">需要找到渲染后的图片里面没有发生变化的Hex地方，添加一句话，通过文件包含漏洞执行一句话</font>

<font style="color:rgb(77, 77, 77);">一般通过.gif格式文件执行</font>

<font style="color:rgb(77, 77, 77);">将上传重写后的gif下载，通过010里面的比较工具对比</font>

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744827355423-959e3a3f-cf72-44af-a4a4-6ca54ed27f59.png)

点击match，随便找一个公共部分插入木马即可

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744827457467-8fb3f4b4-31dd-4254-b1fd-5e503a18bdc8.png)

直接上传然后进行文件包含

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744827541907-cbb3ae80-d3f8-4a86-b3c3-8092cd900c88.png)

蚁剑连接，成功

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744827558737-d19a9c7c-3db5-4993-b298-8728c448511e.png)

## Pass-17
审计代码，发现文件先从临时目录被上传到服务器之后再进行白名单的判断

**<font style="color:rgb(77, 77, 77);">先上传，后删除，中间有一个极短的窗口期，文件是在服务器中的，可以进行操作——条件竞争漏洞</font>**

**<font style="color:rgb(77, 77, 77);">在检测出不合法之前访问一次，从而生成小马</font>**

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744891947935-1acb1ff9-72d7-496a-80a3-b764374444e2.png)

先上传含有小马的php文件，用burpsuite抓包

```plain
<?php fputs(fopen('cmd.php','w'),'<?php @eval($_POST["cmd"])?>');?>
```

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744894003133-d645ee4d-cc95-4772-9e0f-3423fc023264.png)

右键发送到攻击模块intruder，清除payload位置

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744894162023-6f347e0b-5571-4446-b56d-e4b7e11bc5eb.png)

设置payload类型为null，无限重复

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744894576330-312e2bac-63f5-4990-911a-b81ba7c9ed30.png)

设置线程为20，间隔为0

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744895692350-9e307bdb-058a-45fb-8a61-8c2e2b665573.png)

访问race.php并抓包，发送到intruder

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744896604489-37ada488-8463-4ecf-acd5-65e8893b8ff5.png)

修改payload类型和线程

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744896626786-2353e6b3-94fe-4e94-9f6d-f0a0942ad620.png)

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744896296332-188696c2-366b-45c7-8661-dd203aca6b59.png)

同时启动上传和读取的攻击模块（卡飞了）,查看记录会发现有一次访问成功的

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744896809037-ef7e21e3-9471-4522-b95f-acf50d8879f0.png)

在系统文件夹中查看发现确实上传成功了

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744896801267-cfa3dac1-a9d0-4236-a58a-0db7e475ce80.png)

访问成功

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744896879023-cde3d018-5301-4191-9514-9ff86ceb0753.png)

蚁剑连接，连接成功

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744896908313-a6fd1b8d-e4a3-4748-b5a0-8699fc8a27bb.png)

## Pass-18
这一关先检查文件后缀再移动、改名，所以上一关的方法不能再用了，因为php文件在移动前就被删掉了

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744897610769-6c5e7fa1-31d0-4f69-9b7b-4ee44af9c036.png)

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744897637748-6e89e616-4bf9-47d3-b228-c2c908e7a3bb.png)

可以利用apache漏洞，将文件后缀改为.php.7z

因为apache不能解析.7z后缀，所以会跳过直接解析.php后缀，从而实现绕过（注意nginx没有这个漏洞）

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744897829239-476266c6-0975-4cba-9a54-0497f24aeb4e.png)

抓包后步骤同上一关进行条件竞争即可

上传爆破

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744897899097-fdf195af-716f-4186-9c70-e87f3925e386.png)

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744897920823-70acfe85-1b05-4877-aa18-a7bd03014d15.png)

读取爆破

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744897995453-ac3bbdbe-27ae-4311-8d67-21fef40aae4a.png)

cmd.php上传成功了

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744898106368-fde2709a-e44c-4999-8ba0-c62297c1b621.png)

蚁剑连接

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744898141354-20b1e2c7-5c2e-4af4-b8d9-3c1fd74f7f2d.png)

## Pass-19
这关先判断再移动，不存在条件竞争，<font style="color:rgb(77, 77, 77);">没有对上传的文件做判断，只对用户输入的文件名做判断</font>使用后缀绕过即可

上传并自定义为.php.文件即可，bp都不用

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744898747658-abf33400-0c47-417f-9465-12cebf63d431.png)

连接成功

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744898704813-de7494a9-21c8-4bb6-b149-8c1885ca4efa.png)

## Pass-20
上传一个cmd.jpg格式文件

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744902958022-6ea7e5bc-9974-41a0-a31b-f05fea81242d.png)

绕过

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744903005460-d53e03d2-eb76-4e51-a400-52b00a90b6e0.png)

抓包后进行以下修改

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744900809458-0c560925-f238-4d6f-a19a-fde8554ef508.png)

save_name[0]=cmd.php 绕过了通过 . 拆成数组的代码，因为$file已经存在值save_name[0]

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744903345328-9de2bfe1-6eff-4cd2-a20e-4b1b419f81e3.png)

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744903448939-dd629b50-4cfe-43fc-ad0b-e9bff063d7e2.png)

绕过以上代码的逻辑如下：

```plain
$file[0]=cmd.php
$file[3]=jpg

end($file)=jpg

一共两个值，读取的是：
$file[2-1]=null

拼接完是：
cmd.php.(null)
```

上传成功

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744900826589-8631bf20-63ea-4fbb-a932-d0e4af487dd7.png)

连接成功

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744900846652-1ce94b53-5de1-48b8-bf27-5f496f3386fb.png)



**终于通关了，完结撒花**

