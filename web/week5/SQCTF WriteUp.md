# Web
## ezGame  
进入后发现是2048小游戏



第一次拼尽全力无法战胜



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744118507823-90e1f50f-8016-459c-8da9-67356c2037a9.png)



第二次上下左右瞎玩过了



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744118704320-14f04774-561b-4c66-97a1-53ed661cefb7.png)



拿到flag：sqctf{dee2d2546a754531990724506d16ed4b}



## eeaassyy  
进入后按F12调用开发者工具，发现被禁用了



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744118791640-082be548-7935-4522-aa91-f81b8f343f4d.png)



再按一下F12即可，从源代码中拿到flag：sqctf{c67c58ab01744d25b4c62ec0dd17c715}



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744118862467-7d1c076c-de50-4b40-ab20-67c59f5fb1dd.png)



## My Blog  
进入后发现页面下方有github的链接可以点，进去看一下



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744118965594-2cbcf41a-7e10-436d-9f68-d79f73f08e4b.png)



发现是一个pdf，在里面找到用户名和密码，需要找到登录界面



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744119008364-f1f8bd27-f3d4-4c21-9078-7047501f3779.png)



打开robots.txt看一下，找到登录界面文件



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744119047892-b3d92a22-173c-4e3b-9a50-da21cd62302e.png)



输入用户名和密码即可拿到flag： sqctf{24d52e10a7564c689a482a42093298e0}  



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744119104440-15bd69d0-36d5-45ac-ac6e-dbb1709cf85e.png)



## 逃  
这道题考察反序列化



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744119162326-0b1d1db4-e672-46bf-8a74-460744234f1b.png)



<font style="color:rgb(64, 64, 64);">要构造一个反序列化的payload，使得test类的pswd属性值为escaping</font>

<font style="color:rgb(64, 64, 64);"></font>

<font style="color:rgb(64, 64, 64);">将payload参数设置为以下字符串，通过get方式提交</font>

<font style="color:rgb(64, 64, 64);">O:4:"test":2:{s:4:"user";s:4:"test";s:4:"pswd";s:8:"escaping";}</font>

<font style="color:rgb(64, 64, 64);"></font>

<font style="color:rgb(64, 64, 64);">拿到flag： sqctf{8a354281631747db8cf58f8fcb0d900a}  </font>

<font style="color:rgb(64, 64, 64);"></font>

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744119320420-e2fea17c-66ef-4072-82f8-fc94358ad9b2.png)



## 商师一日游  
进入后直接访问第一关



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744119387389-90bc9334-d637-4e19-998c-161e899e06ad.png)

<font style="color:rgb(64, 64, 64);"></font>

<font style="color:rgb(64, 64, 64);">进入第一关后先查看源代码，直接发现碎片1和第二关的名称</font>

<font style="color:rgb(64, 64, 64);"></font>

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744119426851-354f8eff-e853-4fb1-9f14-61baf08a5168.png)



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744119446673-0d2523d9-23b6-48b3-b9ce-a6df7e15b43a.png)

<font style="color:rgb(64, 64, 64);"></font>

<font style="color:rgb(64, 64, 64);">进入第二关后，首先尝试get和post方式但是失败了，注意到cookie翻译过来是曲奇饼，于是尝试去修改cookie</font>

<font style="color:rgb(64, 64, 64);"></font>

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744119515695-df657cf0-f400-4b97-b03c-1d4fe9df7cc7.png)

<font style="color:rgb(64, 64, 64);"></font>

<font style="color:rgb(64, 64, 64);">利用burpsuite抓包后，将weak修改为strong，发送，即可得到碎片2和第三关</font>

<font style="color:rgb(64, 64, 64);"></font>

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744119619704-fff6693c-1f8b-4815-8b30-1beb0d1a3d3b.png)



第四关提示在开发者工具中找，发现网络中含有碎片3和第四关



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744119728125-c5bbb920-7dc3-4dfc-882f-1e2200f2b8e2.png)



第四关提示访问robots.txt



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744119783668-e1d5f516-f799-4121-93ca-225599619c1f.png)



找到碎片4和第五关



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744119809939-42703544-86bf-44fd-a917-3b5037238213.png)



第六关考察preg_match正则匹配式



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744119851340-de30d6d4-9009-43cd-9a39-b83d3c731342.png)



<font style="color:rgb(64, 64, 64);">要让代码输出 xxxxxxxxxxx 而非 hacker，需确保 第二个正则不匹配，但 第一个正则匹配。</font>

> <font style="color:rgb(64, 64, 64);">第一个正则 /^php$/im：</font>
>
> <font style="color:rgb(64, 64, 64);">i：不区分大小写。</font>
>
> <font style="color:rgb(64, 64, 64);">m：多行模式（^ 和 $ 匹配每行的开头和结尾）。</font>
>
> <font style="color:rgb(64, 64, 64);">要求字符串中至少有一行完全等于 php（不区分大小写）。</font>
>
> <font style="color:rgb(64, 64, 64);"></font>
>
> <font style="color:rgb(64, 64, 64);">第二个正则 /^php$/i：</font>
>
> <font style="color:rgb(64, 64, 64);">i：不区分大小写。</font>
>
> <font style="color:rgb(64, 64, 64);">无多行模式，要求整个字符串完全等于 php（不区分大小写）。</font>
>

<font style="color:rgb(64, 64, 64);"></font>

<font style="color:rgb(64, 64, 64);">构造hhh=php%0A123即可（%0A相当于换行），拿到碎片5和第六关</font>

<font style="color:rgb(64, 64, 64);"></font>

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744120122733-a3ec5ace-6788-43a6-aab6-ee18b8bbadcc.png)



第六关要点击按钮但是被锁定了，看一下源代码



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744120171326-a9244a97-fdfe-44b6-aa89-75f0e1ccbeba.png)



只需要post一下auth=Hidden Levels即可



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744120206401-fc8891cc-afc4-4c47-8c16-17aea9e99c6c.png)



拿到碎片6和第七关



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744120247692-5bef132b-b6ed-430e-bc4a-11446cfcf0a6.png)



最后一关是很简单的命令执行，先调用目录



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744120330879-0d8b360e-ddc4-4061-9100-6b32b1644dd4.png)



直接cat  Tourist_fragment7，拿到碎片7



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744120360608-2948a14e-81dd-47ea-9881-7c83e4ecb5c1.png)



拼起来可以得到sqctf{0017fa6acec345289e978f134dc963e9} 



## Upload_Level1  
先将一句话木马写在php文件中，发现题目要求只能交图片，直接将文件名改为png上传



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744122232588-5d154154-1f0a-4721-a50a-6eda2ccd4e03.png)



直接用burpsuite抓包，并修改文件格式为php



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744122987061-0e6fed98-a9ee-4ea5-9d60-e52131b3ffaf.png)



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744122955154-1fbae653-37c1-47d5-b052-9ae9083e2ecb.png)



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744123150177-85f0b329-fc6b-498c-b153-bf59aff77b99.png)



用中国蚁剑连接



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744123177007-24b0db6c-a2f3-431a-afc5-e8ddeb99f2b6.png)



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744123205659-bb77bf2e-188c-42cf-b503-692c5fd3dd8f.png)



直接在根目录下找到flag



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744123224216-66699a6f-bd51-4a3d-86d6-e13062266fc3.png)



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744123238239-96aa6864-043d-40c3-b0cd-373980f3b23a.png)



拿到flag：

sqctf{34b5041187524ec18458816047f35dd7}



## 小小查询系统   
这题考察sql注入，先判断闭合方式为单引号



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744306093665-486a4f18-eab6-4f52-be5f-ab059376bdd1.png)



判断为3列



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744306132732-342efb82-fc10-4b28-a678-c177ec00f15a.png)



查询回显位置，拿到库名，感觉应该在ctf库里面



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744306074781-d8665aa6-1105-4e83-a260-ad6712c69833.png)



拿到表名



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744306308888-a7a672a6-adfa-467d-b588-fc8954616f64.png)



拿到列名



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744306342716-4a75d5de-de07-403d-97cf-372be967c931.png)



查看value，拿到flag：

<font style="color:black;">SQCTF{66a47fc2-cf6e-7648-b523-d29363b5580c}</font>



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744306409060-60321ee7-68b5-47e4-920f-45069d14d263.png)

## Upload_Level2  
类似于Upload_Level1，将一句话木马写在php文件中，将文件名改为png上传



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744194045745-6c597fc6-5b57-4326-96b3-687556f88045.png)



用burpsuite抓包改文件格式



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744194092363-d981fc6b-73b0-432b-bc67-bdb0be9e50aa.png)



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744194032105-9750c67a-8763-4ca2-8109-5ecceeec5f86.png)



还是用中国蚁剑连接，但是在目录下没有找到flag



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744195604322-80ba5752-b683-4acc-94a0-6eccb06030ec.png)



选择用命令执行，调用system()函数，先查目录，找到flag



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744194268269-a05b2fde-765c-4d1f-b1bf-893117d9996d.png)



直接cat /flag即可



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744194310821-18c80894-fdc5-492a-8a72-b6b7be6b23b0.png)



拿到flag： sqctf{440b9144f834468cbbeb74e2e455ef75}  

## RceMe
先用ls确定flag位置在根目录下

?com=ls /

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744168386418-f5da340b-6327-409f-86dd-6dd6a9d1bb86.png)



直接通过nl /* 显示根目录下文件内容

?com=nl /*

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744168298622-0eef401a-f7f1-4990-8fbe-2b25baeff601.png)



得到flag为： sqctf{c5d6a122a16d4de8aad1e80538f9f311}  



## Ping  
使用 | 管道符绕过，无论 ping 是否成功，后面指令都会执行。



查看目录?ip=127.0.0.1 | ls /



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744169015963-a74fe0a9-cad3-44ea-b380-f58df5bde1de.png)



直接执行?ip=127.0.0.1 | cat /flag



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744168894100-e0a56b5b-392e-4af1-94cb-3a161690a55a.png)



得到flag： sqctf{83f468c9017640589b4cbc123bd97f57}  



## baby rce  
shal()是弱类型比较，用数组绕过可以满足 null == null，可以通过第一关



?param1[]=a&param2[]=b



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744172296938-a2530e07-0a29-4f40-85a4-709c3e90a49e.png)



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744172745946-c2ea41ad-490f-434f-ade7-a929048776ff.png)



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744172973405-2118e377-d202-4fd7-a94a-0f04a50563c3.png)



第二关对`<font style="color:#0000bb;">call_user_func</font>`不是很了解<font style="color:#0000bb;">，</font>通过deepseek可以得到以下payload



payload=TYctf::getKey



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744173327451-1f7921e9-a46a-4e18-a3f5-7336db7db18e.png)



得到flag： sqctf{37156e9ce3c54d788908e1692dbfce70}  



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744173356305-b0e83081-1852-4eb6-b250-3d75cfa2dfe3.png)



## Input a number  
直接构造浮点数即可



$num == 114514 ，114514.1作为浮点数，与整数比结果为false，条件不成立



intval($num, 0) == 114514

intval("114514.1", 0) 会截取整数部分 114514，条件成立，输出 flag



?sqctf=114514.1



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744175233247-ca842cae-196b-4d1e-a5d1-0e0bd7810a75.png)



拿到flag：sqctf{b0e34ba9805c4443b2d918e1795598f6}

## 嘿嘿嘿
通过 unserialize($_POST['data']) 反序列化用户输入，生成对象 $data



is_array($data->file) || md5($data->file) === md5("flag.php")：确保 $data->file 不是数组且其 MD5 不等于 flag.php 的 MD5。



strpos($data->file, 'php://') !== false：禁止使用 php:// 协议。



$data->content === "GET_FLAG"：若满足，直接输出 flag.php 的内容



让 $data->content = "GET_FLAG"，同时 $data->file 不触发前两个检查。将 $data->file 设置为一个对象（如 xxx 类），其 __toString() 返回一个与 flag.php MD5 不同的值（如 test.txt）



构造data=O:3:"hhh":2:{s:4:"file";O:3:"xxx":1:{s:4:"data";s:8:"test.txt";}s:7:"content";s:8:"GET_FLAG";}



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744219952389-7e804a28-5ae8-4a3e-bccc-56472c9f8158.png)



查看源代码



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744219963324-0066e61b-7fcb-4b71-a0c5-faf45c43fe2c.png)



得到flag：sqctf{afa30555f21f43d7b167b69cf8782852}

## 哎呀大大大黑塔
唉，崩，复制提交bv号



?SQNU=BV1tXckehEd



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744212081789-37e48df2-4f57-4d0c-854f-ca9fb19c2136.png)



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744213593684-bb05689b-d12a-4880-afd6-50eadae4311d.png)



直接构造Secret对象，key属性设置为"SQCTF"，并将其序列化，反序列化会触发<font style="color:rgb(64, 64, 64);">__destruct()方法，执行include函数，输出flag</font>



?data=O:6:"Secret":1:{s:3:"key";s:5:"SQCTF";}



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744212742185-7a2a2a4f-e577-4f09-957d-8c67238c4fa2.png)



拿到flag：sqctf{7278c2a4b0a2432bbe7c70fd9e92168e}



##  Are you from SQNU?  
点击hit the question setter，得到?tyctf=



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744466027678-7eb6abe1-80e1-4fa0-807b-47ff54ae9ebe.png)



改为post方式提交



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744466047352-780ec716-5461-4bc8-b83f-73cae99192ed.png)



再提交hhh=abc



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744466061361-72c6e3ac-34ab-4bb6-9118-0f3fe5705789.png)



添加referer，修改来源



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744466096356-f448aeb6-7716-4779-9970-6e7ab458bd61.png)



修改user agent



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744466119544-e01bbd57-dfdf-4621-a38e-4d0fe09e10c7.png)



改为本地访问



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744466140271-97d901c9-2320-41f2-858f-dc8983f1530a.png)



修改cookie



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744466186407-bc7ac6b4-442e-4fd4-a97f-e789fca12495.png)



拿到flag：sqctf{1dab7746f116495b853c9d671827dac6}



## ggoodd  
<font style="color:rgb(64, 64, 64);">JSON（JavaScript Object Notation）是一种</font>**<font style="color:rgb(64, 64, 64);">轻量级的数据交换格式</font>**<font style="color:rgb(64, 64, 64);">，广泛用于前后端通信或配置文件。它的结构类似于编程中的字典（键值对）</font>

<font style="color:rgb(64, 64, 64);"></font>

<font style="color:rgb(64, 64, 64);">POST参数id的值为abc，且GET参数json解析后的数组中x的值为cba</font>



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744475634624-8c75d8b4-e355-46e7-b715-649bc90242c9.png)



拿到flag：`  
`sqctf{f167cca208d5416fa58806d6c83b9f46}  



## 开发人员的失误
将题干问deepseek后提示用dirsearch扫描目录



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744476422860-7d5b901a-5ad4-4f92-a00c-c6408291cbcf.png)



访问backup.sql文件，发现自动下载了



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744476468961-cba8ee3e-a52c-4ba1-a108-a276eee507aa.png)



在该文件里可以找到flag：sqctf{2b20ae77713742459efec5c4be50f21d}



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744476500284-66a5887e-a541-44ab-96e8-36e125bcdcc3.png)



## 伪装
喂给deepseek



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744479447756-bf1f17b0-3d82-47a2-883e-eff9c065f160.png)



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744479457693-cf691e07-07d3-4592-bb44-fbbc171cdfb0.png)



利用`**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">flask-unsign</font>**`<font style="color:rgb(64, 64, 64);">工具</font>生成<font style="color:rgb(64, 64, 64);">伪造的cookie值</font>



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744479466963-e8f248c2-dd57-49fc-9acd-324e819bb8d1.png)



伪造session后直接访问/admin即可拿到flag：

 sqctf{6be1328c188f4db6af0858017ec3a263}  



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744479507667-9061ee22-a50d-4aeb-a330-bd9defb2233c.png)



## Ez_calculate
喂给deepseek



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744480463953-1945db15-1515-45db-ab42-115554e7fb48.png)



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744480485657-5b3ede32-3b51-46d4-957c-f15cd53f9d8c.png)



生成以下自动计算并提交的脚本



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744480508646-a486d0c1-623a-4d97-ab3b-4868177307f3.png)



```plain
import requests
from bs4 import BeautifulSoup

# 目标URL
url = "http://challenge.qsnctf.com:31369"  # 替换为题目实际URL

# 创建会话保持连接
session = requests.Session()

# Step 1: 获取页面并提取数学表达式
response = session.get(url)
soup = BeautifulSoup(response.text, 'html.parser')
challenge_div = soup.find('div', class_='challenge')

if not challenge_div:
    print("错误：未找到数学表达式!")
    exit()

expression = challenge_div.text.strip().replace(' ', '')  # 清理空格
print("提取的表达式:", expression)

# Step 2: 计算表达式结果
try:
    # 安全提示：实际CTF中eval可接受，生产环境需谨慎！
    result = eval(expression)
    print("计算结果:", result)
except:
    print("计算错误，表达式可能有误!")
    exit()

# Step 3: 提交答案
data = {'value': result}
response = session.post(url, data=data)

# Step 4: 输出响应（含flag）
print("响应内容:")
print(response.text)

```



得到响应结果



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744480529813-56bd312a-43bf-467d-89ce-2402b0bdb9af.png)



继续喂给deepseek，生成并添加自动点击链接的脚本



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744480563010-e9306145-494c-46df-88ed-6a5fa8fa905c.png)



得到响应内容，flag为：sqctf{af2df04b7a654de3b4e26822fbb4d39b}



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744480626831-f75cfbc0-2683-47eb-b0be-c859573f4ed0.png)

# Misc
## Welcome_Sign_in  
签到题，扫码即得



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744123829016-3586fb50-1bac-4f0d-8da5-3bae8809a641.png)



![](https://cdn.nlark.com/yuque/0/2025/jpeg/54428404/1744123838583-1c66b566-c472-43b2-bcb2-e277be28ff99.jpeg)





## love.host
该图片是隐藏的压缩包，直接将图片解压即可



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744304158770-ab20da40-fdac-46ed-a35b-5ce7da30866e.png)



即可得到flag，但是需要改一下大小写



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744304206067-134b4e88-e1f7-4dfb-ba1e-606d48f42652.png)



##  ez_music1  
将音频文件导入Audacity，听后发现后半段有杂音



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744307512462-dcece54a-8ab6-4d01-b4a1-60d6ca46e14e.png)



切换到频谱图就可以直接看到flag：

SQCTF{Rush_B}



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744307739767-d5b980ff-a8bd-4508-9643-0dfc26e2036b.png)



## 王者荣耀真是太好玩了  
啊这啊这农，看到就蚌埠住了



先启动王者荣耀，找到图中超下饭的萌新



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744489268199-1ad80e64-d09e-4f42-8bd3-d76c228a4c74.png)



![](https://cdn.nlark.com/yuque/0/2025/jpeg/54428404/1744489243195-b18d1513-df1e-4b27-b889-188125256ffd.jpeg)



发现头像是一个地点，结合百度地图的提示，去搜一下



![](https://cdn.nlark.com/yuque/0/2025/jpeg/54428404/1744489284183-89fa34db-8a62-4d2f-946c-0ddbe0a4666f.jpeg)



在评论里找到



![](https://cdn.nlark.com/yuque/0/2025/jpeg/54428404/1744489332947-bb876079-d14a-4157-bdfd-9ff330cea06b.jpeg)



url解码即可得到flag：sqctf{d29_qaXVza_GlndXlpZ_Xhpbm5p_ZGVoYWh_haGE%3d}



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744489349575-281a9c42-eebd-4e01-89ab-d545ca74a927.png)



## 可否许我再少年
填问卷即可拿到：SQCTF{this_is_real_end}



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744519336678-d3f7c498-f277-41d2-8bd2-fe8fcb5a61cb.png)



# Crypto
感谢以下两篇文章

[CTF——Crypto密码学题型总结_ctf密码题型总结-CSDN博客](https://blog.csdn.net/hahaha233330/article/details/109294276)

[CTF中的常见密码——古典密码学、现代密码等等_ctf密码-CSDN博客](https://blog.csdn.net/wanfengchuifu/article/details/135910625)

## 春风得意马蹄疾  
通过检索得知是社会主义核心价值观编码，在utools里下载工具



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744298716956-2acdb6f3-7a8d-4edb-a22c-0b880b82f54b.png)



原码



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744299280307-2b1181e0-cef0-432e-a946-21b7712703fa.png)



第一轮解码



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744299289080-78bcaf13-ba05-4c91-b736-40bfecd82d2e.png)



第二轮解码



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744299296286-9fd2a09b-7357-4971-8047-c919ecc38dd7.png)



第三轮解码拿到flag



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744299302231-fb1d5166-7c31-401e-948c-afaa976e98c0.png)



flag：SQCTF{E2jacnicamcm_cnanamw_kwkma}



## 简单RSA
RSA解码，已知e n c先分解n得到p和q



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744301384079-f8d8155d-0f72-46d4-8a08-a23f5adb9901.png)



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744301333675-b1694929-72bf-4c4a-86ab-7a70c3498952.png)



python脚本如下



```plain
import gmpy2
from Crypto.Util.number import long_to_bytes
 
 
q = 85729314844316224669788680650977264735589729061816788627612566392188298017717541385878388569465166835406950222982743897376939980435155664145111997305895651382483557180799129871344729666249390412399389403988459762024929767702864073925613168913279047262718022068944038280618279450911055132404010863611867388261
p = 85729314844316224669788680650977264735589729061816788627612566392188298017717541385878388569465166835406950222982743897376939980435155664145111997305895651382483557180799129871344729666249390412399389403988459762024929767702864073925613168913279047262718022068944038280618279450911055132404010863614460682753
 
e = 65537
c = 3514741378432598036735573845050830323348005144476193092687936757918568216312321624978086999079287619464038817665467748860146219342413630364856274551175367026504110956407511224659095481178589587424024682256076598582558926372354316897644421756280217349588811321954271963531507455604340199167652015645135632177429144241732132275792156772401511326430069756948298403519842679923368990952555264034164975975945747016304948179325381238465171723427043140473565038827474908821764094888942553863124323750256556241722284055414264534546088842593349401380142164927188943519698141315554347020239856047842258840826831077835604327616
# n = 73069886771625642807435783661014062604264768481735145873508846925735521695159
n = q*p
# print(n)
d = gmpy2.invert(e, (p - 1) * (q - 1))
print("d=",d)
m = pow(c, d, n)
print(m)
print(long_to_bytes(m))
```



运行结果如下，拿到flag



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744301273347-4bafc00c-2dc1-4e2c-a671-6b9635a2a815.png)



flag：SQCTF{be7e48547356cdf16649fd29e0ff9e1f}



## 别阴阳我了行吗？  
检索得知是阴阳怪气编码，直接解码即可



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744305728549-6310f637-69e5-4a27-ba6e-388172942a94.png)



拿到flag：SQCTF{xm！tql！xm！}



## 玩的挺变态啊清茶哥  
检索得知为猪圈密码，直接解码



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744308491995-0a3dca7f-1240-4413-be0b-d9fabb757c2b.png)



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744309006816-94919609-4c9b-4e57-8429-02caf34f2c7e.png)



##  你的天赋是什么  
一眼摩斯密码



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744477758863-f04775eb-3ef0-49e0-852f-10a776c1c2bb.png)



直接解码即可，再进行url解码拿到flag： 

SQCTF{YOU-HAVE-TALENT}  



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744477804800-f7cb0f59-85e3-4923-8dd0-ff5b37f92d5f.png)



##  丢三落四的小I  
这题是RSA中dp泄露的题目



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744482358074-ff0cc589-ae13-4513-9e0b-bd1c48ca77b9.png)



以下为python脚本



```plain
import gmpy2
from Crypto.Util.number import long_to_bytes
 
e = 65537
n = 15124759435262214519214613181859115868729356369274819299240157375966724674496904855757710168853212365134058977781083245051947523020090726851248565503324715984500225724227315777864292625995636236219359256979887906731659848125792269869019299002807101443623257106289957747665586226912446158316961637444556237354422346621287535139897525295200592525427472329815100310702255593134984040293233780616515067333512830391860868933632383433431739823740865023004008736555299772442805617275890761325372253913686933294732259451820332316315205537055439515569011020072762809613676347686279082728000419370190242778504490370698336750029

dp = 1489209342944820124277807386023133257342259912189247976569642906341314682381245025918040456151960704964362424182449567071683886673550031774367531511627163525245627333820636131483140111126703748875380337657189727259902108519674360217456431712478937900720899137512461928967490562092139439552174099755422092113
 
c = 4689152436960029165116898717604398652474344043493441445967744982389466335259787751381227392896954851765729985316050465252764336561481633355946302884245320441956409091576747510870991924820104833541438795794034004988760446988557417649875106251230110075290880741654335743932601800868983384563972124570013568709773861592975182534005364811768321753047156781579887144279837859232399305581891089040687565462656879173423137388006332763262703723086583056877677285692440970845974310740659178040501642559021104100335838038633269766591727907750043159766170187942739834524072423767132738563238283795671395912593557918090529376173
 
for i in range(1, e):
    if (dp * e - 1) % i == 0:
        if n % (((dp * e - 1) // i) + 1) == 0:
            p = ((dp * e - 1) // i) + 1
            q = n // (((dp * e - 1) // i) + 1)
            d = gmpy2.invert(e, (p-1)*(q-1))
            m = pow(c, d, n)
 
print(m)
print(long_to_bytes(m))

```



拿到flag：SQCTF{7b909221-c8ff-f391-0c86-d3a9ca8491d1}



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744482375445-fc68104e-063d-4d95-b7ce-1912f51d5690.png)





## 密室逃脱的终极挑战  
第一关是凯撒密码



I am the key to the next



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744483561729-ecae067d-3be1-48c5-9c93-b86de9d76ac7.png)



第二关是栅栏密码



The secret message is hidden in the flag



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744483584823-ab08e66d-58ac-4983-9f08-2ae827d768e8.png)



第三关是摩斯电码



HALLO MORSE CODE GUTR



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744484644155-10c890a8-6004-458f-ae11-3cc6fb2354f2.png)



第四关是base64，直接可以拿到flag



SQCTF{F4BBAC33-8D80-A886-5238-EA35B38B353A}



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744484912426-14c8f5c6-6ea1-4c5a-a05e-6519143661c5.png)



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744484860842-3072a5e2-5df6-45c1-81df-171421940fdd.png)



```plain
# Rotor cipher decoder
# parameter input
rotor = [
    "QWXZRJYVKSLPDTMACFNOGIEBHU", "BXZPMTQOIRVHKLSAFUDGJYCEWN", 
    "LKJHGFDSAQZWXECRVBYTNUIMOP", "POIUYTREWQASDFGHJKLMNBVCXZ", 
    "ZXCVBNMASDFGHJKLPOIUYTREWQ", "MNHBGVCFXDRZESWAQPLOKMIJUY", 
    "YUJIKMOLPQAWSZEXRDCFVGBHNM", "EDCRFVTGBYHNUJMIKOLPQAZWSX", 
    "RFVGYBHNUJMIKOLPQAZWSXEDCT", "TGBYHNUJMIKOLPQAZWSXEDCRFV", 
    "WSXEDCRFVTGBYHNUJMIKOLPQAZ", "AZQWSXEDCRFVTGBYHNUJMIKOLP", 
    "VFRCDXESZWAQPLOKMIJNUHGBTG", "IKOLPQAZWSXEDCRFVTGBYHNUJM" 
]
 
cipher = "UNEHJPBIUOMAVZ"
 
key = [4, 2, 11, 8, 9, 12, 3, 6, 10, 14, 1, 5, 7, 13]
 
tmp_list=[]
 
for i in range(0, len(rotor)):
    tmp=""
    k = key[i] - 1
    for j in range(0, len(rotor[k])):
        if cipher[i] == rotor[k][j]:
            if j == 0:
                tmp=rotor[k]
                break
            else:
                tmp=rotor[k][j:] + rotor[k][0:j]
                break
    tmp_list.append(tmp)
# print(tmp_list)
 
message_list = []
for i in range(0, len(tmp_list[i])):
	tmp = ""
	for j in range(0, len(tmp_list)):
		tmp += tmp_list[j][i]
	message_list.append(tmp)
 
print(message_list)

```



## 《1789年的密文》
这题看格式是 托马斯.杰斐逊 转轮密码



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744487138867-059039e8-f525-43c7-89f6-ff9f39f5cc22.png)



python脚本如下：



```plain
import re
text = ""
code = [  # 密码本
"QWXZRJYVKSLPDTMACFNOGIEBHU",
"BXZPMTQOIRVHKLSAFUDGJYCEWN",
"LKJHGFDSAQZWXECRVBYTNUIMOP",
"POIUYTREWQASDFGHJKLMNBVCXZ",
"ZXCVBNMASDFGHJKLPOIUYTREWQ",
"MNHBGVCFXDRZESWAQPLOKMIJUY",
"YUJIKMOLPQAWSZEXRDCFVGBHNM",
"EDCRFVTGBYHNUJMIKOLPQAZWSX",
"RFVGYBHNUJMIKOLPQAZWSXEDCT",
"TGBYHNUJMIKOLPQAZWSXEDCRFV",
"WSXEDCRFVTGBYHNUJMIKOLPQAZ",
"AZQWSXEDCRFVTGBYHNUJMIKOLP",
"VFRCDXESZWAQPLOKMIJNUHGBTG",
"IKOLPQAZWSXEDCRFVTGBYHNUJM",
]
print(code)
codetext = "UNEHJPBIUOMAVZ"  # 密文
codenum = "4,2,11,8,9,12,3,6,10,14,1,5,7,13"  # 密钥
codenum = codenum.split(",")  # 把这些数字都放到一个列表里面去，以逗号隔开
#print(codenum)
a = 0
print("最终解码本：")
for i in codenum:
    index = code[int(i)-1].index(codetext[a])
    # code[int(i)-1]代表取出密钥对应的密码表中的字符串（对应重新排列），以解码本第一行为例，即取出密码本中的第12行字符串BRUHGUFGTJNUBAFDEGTEF
    # index(codetext[a])取出当前对应密文在取出的字符串中的下标（即先找到循环的下标），以解码本第一行为例，即密文中第一个字符A在上面一行取出的字符串中的下标，即13，从0开始
 
    a = a + 1  # 密文下标加一
    code[int(i)-1] = code[int(i)-1][index:] + code[int(i)-1][:index]  # 拼接得到最终的一行解码
    # code[int(i)-1][index:]是取出从密文对应的字符开始到最后的字符串，以解码本第一行为例，即取出AFDEGTEF
    # code[int(i)-1][:index]是取出从头到密文对应的字符的字符串，不包括密文对应的字符，以解码本第一行为例，即取出BRUHGUFGTJNUB
    print(code[int(i)-1])  # 此时输出的字符串是按解码本的顺序从第一行开始，但在code中存储的顺序是对应密钥内容的顺序
 
#完成了变形了
 
print("输出解码本每一列")
for i in range(len(code[0])):  # len(code[0])即一行的长度
      str=""
      print("第{}列的是:".format(i),end="")
      for j in codenum:
          str += code[int(j)-1][i]  # 因为变形后的解码本中的字符串是存储在密码本中，因此要按照密钥内容的顺序进行取出
      print(str.lower())

```



运行结果如下：



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744487197472-81164d89-bb97-4c60-908e-4b60829221a2.png)



找到最像flag的：SQCTF{maketysecgreat}



##  字母的轮舞与维吉尼亚的交响曲  
看题目名得知是维吉尼亚密码



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744487828167-8880fe94-7372-4527-bb9f-e83c1cea37a9.png)



文件夹里有一个压缩包需要密码，通过网站破解一下，得到密码123456



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744487849097-d882f5be-bd81-4e75-9bf5-a26f43714fc2.png)



得到密钥是flag



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744487963073-881deaa6-29f5-48f2-8aa9-a12c01cf97d1.png)



解码后得到以下文本，其中<font style="color:rgb(17, 17, 17);">VTFWI{brx_duh_zlq!}很像flag</font>



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744488114380-0934cc2c-1e6c-40d8-88ec-3e5af8ad75fc.png)



题目中提示字母的轮舞，可能是凯撒密码，解码后得到flag：SQCTF{you_are_win!}



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744488242878-9695e12f-ccde-463c-a2ed-351e9fe361b3.png)



##  ezCRT  
根据题目提示知道是中国剩余定理解rsa



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744520844372-b2ba81d4-3348-493c-9340-21b4a1a82fe1.png)



参考文章：[BUUCTF RSA4——中国剩余定理-CSDN博客](https://blog.csdn.net/shshss64/article/details/127178428)



python脚本：

```plain
from Crypto.Util.number import long_to_bytes
import gmpy2
 
n1 = int('64461804435635694137780580883118542458520881333933248063286193178334411181758377012632600557019239684067421606269023383862049857550780830156513420820443580638506617741673175086647389161551833417527588094693084581758440289107240400738205844622196685129086909714662542181360063597475940496590936680150076590681')
n2 = int('82768789263909988537493084725526319850211158112420157512492827240222158241002610490646583583091495111448413291338835784006756008201212610248425150436824240621547620572212344588627328430747049461146136035734611452915034170904765831638240799554640849909134152967494793539689224548564534973311777387005920878063')
n3 = int('62107516550209183407698382807475681623862830395922060833332922340752315402552281961072427749999457737344017533524380473311833617485959469046445929625955655230750858204360677947120339189429659414555499604814322940573452873813507553588603977672509236539848025701635308206374413195614345288662257135378383463093')
c1 = int('36267594227441244281312954686325715871875404435399039074741857061024358177876627893305437762333495044347666207430322392503053852558456027453124214782206724238951893678824112331246153437506819845173663625582632466682383580089960799423682343826068770924526488621412822617259665379521455218674231901913722061165')
c2 = int('58105410211168858609707092876511568173640581816063761351545759586783802705542032125833354590550711377984529089994947048147499585647292048511175211483648376727998630887222885452118374649632155848228993361372903492029928954631998537219237912475667973649377775950834299314740179575844464625807524391212456813023')
c3 = int('23948847023225161143620077929515892579240630411168735502944208192562325057681298085309091829312434095887230099608144726600918783450914411367305316475869605715020490101138282409809732960150785462082666279677485259918003470544763830384394786746843510460147027017747048708688901880287245378978587825576371865614')

M1=n2*n3*gmpy2.invert(n2*n3,n1)
M2=n1*n3*gmpy2.invert(n1*n3,n2)
M3=n1*n2*gmpy2.invert(n1*n2,n3)
 
num=c1*M1+c2*M2+c3*M3
me=num%(n1*n2*n3)
print(me)
 
m=gmpy2.iroot(me,3)[0]
print(long_to_bytes(m)) 

```



运行结果：

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744520921533-ba6fae18-9647-4a58-b9e5-f1c37f8cd16f.png)



得到flag：SQCTF{CRT_Unl0cks_RSA_Eff1c13ncy}

# Pwn
## 浅红欺醉粉，肯信有江梅  
nc即NetworkConnection，是一种网络连接的工具，可以用来在两台计算机之间建立网络连接。它是一种简单的网络连接工具，可以帮助用户在不同的计算机之间传输文件和数据。



安装完后直接在ubuntu里运行nc challenge.qsnctf.com 31405，然后cat flag即可



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744304666222-d7e3207e-3755-4049-8abb-1ffa0c5e4d0c.png)



flag为：sqctf{000e2f8dea144560a19af9e6a54df797}



## 领取你的小猫娘  
直接在ubuntu里nc该网站



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744522670003-935a5baa-dcd4-4298-841c-0a9cb876cca9.png)



它说cat girl喜欢吃字母，多输几个字母她就放弃了，然后cat /flag



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744522682036-51dbb699-b428-405a-a7d2-848bfd301c0b.png)



得到flag：sqctf{2152d0491f13473cbdcff2dc59300fa6}

