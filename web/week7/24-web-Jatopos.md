# SSRF学习笔记
## SSRF前置课程NAT
SSRF:服务器请求伪造：是一种由攻击者形成服务器端发起的安全漏洞。

NAT:网络地址转换：通过将一个外部IP地址和端口映射到更大的内部IP地址集来转换IP地址。

## SSRF漏洞原理
攻击的目标：从外网无法访问的内部系统

形成的原因：大部分是由于服务端提供了从其他服务器应用获取数据的功能，且没有对目标地址做过滤与限制。

攻击方式：借助主机A发起SSRF攻击，通过主机A向主机B发起请求，从而获取B的信息（由于防火墙或主机无法直接访问B）

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1747908408820-c18846b1-4fa6-4865-b3ec-508a23df4323.png)

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748020171886-080af4f5-c3cb-4ffd-a8be-64005573b5d2.png?x-oss-process=image%2Fformat%2Cwebp)

## SSRF信息收集File伪协议
file:///etc/passwd  读取文件passwd

file:///etc/hosts  显示当前操作系统网卡的IP

file:///proc/net/arp  显示arp缓存表（寻找内网其他主机）

利用burpsuite获取所有网段

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1747934433699-aa4ee9e2-44b5-479a-82d4-fd3b0aa544b3.png)

address里面有内容的代表主机是存活的

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1747934713287-5d8cf852-59a7-4e5f-b0ef-25e7fcecbc6f.png)

file:///proc/net/fib_trie  显示当前网段路由信息

## SSRF信息收集Dict伪协议
file://  从文件系统中获取文件内容，如file:///etc/passwd

**dict://  字典服务协议，访问字典资源，如dict://ip:6739/info:**

ftp://  可用于网络端口扫描

sftp://  SSH文件传输协议或安全文件传输协议

Idap://  轻量级目录访问协议

tftp://  简单文件传输协议

gepher://   分布式文档传递服务



通过集束炸弹对两个内容进行爆破 

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1747972165756-1901cc80-6635-4ec2-8e3b-829eb2675015.png)

payload1选number payload2选list（列表中添加端口）

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1747972555317-d106a9b4-7801-49bb-85d3-d57a2f0b8e54.png)

通过length判断端口是否打开



判断端口存活后可以获取网站信息：

dict://172.250.250.6:6379/info

## SSRF信息收集http伪协议
HTT伪协议

作用：常规URL形式，允许通过HTTP1.0的GET方法，以只读访问文件或资源。CTF中通常用于远程包含。

利用burpsuite进行目录爆破

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748021339606-0c03519b-21d3-4602-97d5-83c17d99f487.png)

字典中含有常见目录，比如name、shell等；后缀名也可以更改为zip、txt等

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748022412464-22e42566-8038-40b4-a0c4-a0663913f7e5.png)

phpinfo、name、shell均可以正常打开

比如：http://172.250.250.4/shell.php

## SSRF利用Gopher协议学习
gopher伪协议

利用范围较广：【GET提交】【POST提交】【redis】【Fastcgi】【sql】

1、为何使用gopher伪协议

2、利用gopher伪协议发起get/**post**提交

基本格式:URL:gopher://<host>:<port>/<gopher-path>

web也需要加端口号80

gopher协议默认端口为70

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748106919960-ac38049d-4ff8-4e7c-a534-53f0f7801b11.png)

GET:

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748107601882-8c64121f-cfe6-4ff7-8763-d5e4179bcd13.png)

gopher://172.250.250.4:80/_GET%20/name.php%3fname=benben%20HTTP/1.1%0d%0AHost%20172.250.250.4%0d%0A

或者从 _ 后开始，进行两次URL编码

POST:

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748108685447-6999374b-5b01-441e-a70a-d57a3924d05d.png)

## SSRF之环回地址绕过（绕过127进制）
当127.0.0.1被限制时，http://127.0.0.1/flag.php无法访问

需要将127.0.0.1变形

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748189201718-4bbec833-babe-4c50-b9f6-a6a494f0a588.png)

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748189283191-78a090dd-cc92-4a7f-8cd4-f59e4b8cc608.png)

## SSRF之302重定向绕过
内网ip被限制的时候 http://127.0.0.1/flag.php 无法访问

搭建一个公网服务器

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748191035131-e4b13a9c-1b73-4336-a806-b312266a8553.png)

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748196779916-1431de51-69fc-43cd-94de-6bdf734c3bc9.png)

访问公网ip即可

如http://222.183.24.10:7777/index.php

## SSRF之DNS重绑定绕过
![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748191903391-60fd0cdc-320b-4d33-ade2-a8184ebb34e9.png)

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748192389880-4c5c64b3-0664-4ea2-92e2-8969d0da1e7c.png)

第一次DNS解析ip为合法ip：绕过host合法性检查

第二次DNS解析ip设为内网ip：SSRF访问内网

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748192146106-a7483af3-a2bd-4f35-80db-3e813e1906b6.png)

借助以下网站进行DNS

[https://lock.cmpxchg8b.com/rebinder.html](https://lock.cmpxchg8b.com/rebinder.html)

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748192262697-62b32416-8f81-4dd3-b078-a4bdc867ab93.png)

## 使用SSRF进行命令执行
通过访问shell.php进行命令执行

## 绕过方法补充
1.@   http://abc.com@127.0.0.1（ http://www.baidu.com@10.10.10.10 与 http?/10.10.10.10 请求是相同的  ）

2.添加端口号   http://127.0.0.1:8080

3.短地址   https://0x9.me/cuGfD       推荐：http://tool.chinaz.com/tools/dwz.aspx、https://dwz.cn/

4.可以指向任意ip的域名 xip.io                             原理是DNS解析。xip.io可以指向任意域名，即127.0.0.1.xip.io，可解析为127.0.0.1

5.ip地址转换成进制来访问 192.168.0.1=3232235521（十进制） 

6.非HTTP协议

7.DNS Rebinding

8.利用[::]绕过                 http://[::]:80/ >>> http://127.0.0.1

9.句号绕过                  127。0。0。1 >>> 127.0.0.1

10.利用302跳转绕过     使用https://tinyurl.com生成302跳转地址



**限制为http://www.xxx.com 域名**

采用http基本身份认证的方式绕过。即@  
`http://www.xxx.com@www.xxc.com`

**限制请求IP不为内网地址**

当不允许ip为内网地址时  
（1）采取短网址绕过  
（2）采取特殊域名  
（3）采取进制转换

**限制请求只为http协议**

（1）采取302跳转  
（2）采取短地址



如果要求host为null，可以构造?url=http:/@<font style="color:#d19a66;">127.0.0.1</font>/flag



# SSRF习题
##  [NSSRound#28 Team]ez_ssrf  
本题限制了127.0.0.1的访问

直接提交?url=[http://0.0.0.0/flag](http://0.0.0.0/flag)即可

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748194191744-95aaf1de-9ade-4437-b64a-9cb3dc8df8b4.png)

<font style="color:rgb(64, 64, 64);">在大多数系统中，</font>`**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">0.0.0.0</font>**`<font style="color:rgb(64, 64, 64);"> 会被视为指向本地回环地址（</font>`**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">127.0.0.1</font>**`<font style="color:rgb(64, 64, 64);">）</font>

## <font style="color:rgb(64, 64, 64);"> [NISACTF 2022]easyssrf  </font>
<font style="color:rgb(64, 64, 64);">尝试直接访问，给了提示文件</font>

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748197205492-80656832-1d62-4342-b73d-aa82e893ec9e.png)

<font style="color:rgb(64, 64, 64);">得到一个php文件</font>

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748197220190-e33d5b7d-1b06-41e2-94c5-84afb7b44fc3.png)

访问后得到源代码

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748197236982-206c92ce-2712-448a-9da7-bbe48eb08f85.png)

直接?file=/flag即可

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748197281757-1eb7b340-268a-40a8-a30c-84f7ce0ed332.png)

