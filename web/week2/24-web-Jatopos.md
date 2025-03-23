# nssctf作业
## [LitCTF 2023]Follow me and hack me
### 使用hackbar完成
这道题考察get和post命令，首先在网址中添加/?CTF=Lit2023,实现get的传参，重定向后再通过post将challenge的值传过去，即可得到flag

![86b91e228b8d3ce6a3fe4cdf698bb828](https://github.com/user-attachments/assets/29c34166-d838-4ba9-b494-c71c6e750fad)

### 使用burpsuite完成
先打开burpsuite和火狐浏览器的系统代理，打开拦截请求

![6b7a903755ceb87de0f8969c3f44489c](https://github.com/user-attachments/assets/702a66ed-6011-4371-b336-2bdfb32fb95f)

![752b440368c4e8e389fd777b290396db](https://github.com/user-attachments/assets/cb3338bb-11e3-4caa-99da-673f365b2779)

抓到包后重发送给repeater

![785fe0413bbdaa705ce93f69dc3c54b8](https://github.com/user-attachments/assets/7a59d907-2712-4906-b674-4383c3ed9ef9)

在raw模块中get CTF的值，点击send发包

![cc18cda56445b3e31f939cedc942a45c](https://github.com/user-attachments/assets/31bee76c-b069-4f64-b67a-1b01dc89a698)

通过Content-Type: application/x-www-form-urlencoded指令将get修改为post，在第一行中也进行修改，在最后空一行填入challenge的值（一定要空），点击send发包即可得到flag

![f51871b558e258d4b52016bbea4e69b1](https://github.com/user-attachments/assets/61cc84af-1dfc-468b-973c-3b7d4e93d45c)

适用场景：需要用get post提交两个很长的参数

## [SWPUCTF 2021 新生赛]Do_you_know_http
### 使用burpsuite完成
提示使用WLLM浏览器访问，抓包后在User-Agent:WLLM进行修改

![76f55c543cad4cd4c7e4c2e77f9ca463](https://github.com/user-attachments/assets/7e219749-541a-4e8b-8993-b56d6d6dce7b)

重定向后提示需要在本地读取访问，此时可以伪造ip

![24bd9239199721654cd274a387dd875c](https://github.com/user-attachments/assets/59724035-1746-4b73-a7ec-c73188681525)

用X-Forwarded-For:127.0.0.1替换user agent，重定向后可以得到flag

![7e3431e89ed4e48406ef8a8d9f166108](https://github.com/user-attachments/assets/24100e49-6639-4902-be49-9041fe9ee2fa)

### 使用hackbar完成
在hackbar中通过user agent模块输入wllm

![494e2ebed0569ea5e30e85dbaa76dd73](https://github.com/user-attachments/assets/a61155d8-bcea-4d70-9a05-52790c517c05)

add header后输入X-Forwarded-For:127.0.0.1同样可以得到flag

![7d4031f077024fb476432ec8bbc74b84](https://github.com/user-attachments/assets/55508426-cb0d-42ec-9b30-db0ffa686614)

以下为额外扩展的指令
Host：指定目标服务器的域名或IP地址。
User-Agent：发送请求的用户代理（浏览器、爬虫等）的标识信息。
Accept：指定客户端能够接受的响应内容类型。
Content-Type：在请求中指定发送的实体主体的媒体类型。
Content-Length：指定请求或响应中的实体主体长度（字节）。
Authorization：用于进行身份验证的凭证信息，如基本认证或Bearer令牌。
Cookie：包含客户端发送到服务器的cookie信息。
Set-Cookie：服务器在响应中设置cookie信息。
Cache-Control：控制缓存行为的指令，例如no-cache、max-age等。
Referer：指定当前请求的来源页面的URL。
Location：在重定向响应中指定新的URL。
ETag：指定实体的唯一标识符，用于缓存验证。
X-Forwarded-FOR: 识别通过HTTP代理或负载均衡方式连接到Web服务器的客户端最原始的IP地址的HTTP请求头字段
Server：响应中指定服务器的软件信息

## [LitCTF 2023]就当无事发生
此题考察git泄露，根据题目访问guithub.io，可以找到在guithub.com中的主站

![05db0987642428562f72cabc176f168a](https://github.com/user-attachments/assets/1efb01fb-be4d-4fe0-be16-efa80af2e3e2)

注意到此题发布时间，定位到2023.2找到原项目

![a13df8e579bf5ac8a6133e736a2e8cf9](https://github.com/user-attachments/assets/d1144438-c43a-470a-b0e5-21d112a832ef)

![b2fa30f6126ddd368184f99a3526ac9e](https://github.com/user-attachments/assets/2b17d857-01d8-43d2-973f-f6593ee11e83)

考虑到可能是误上传flag后进行修改，可以在commit里面找到历史版本，发现2023.4.29含有flag相关文件夹

通过检索{}定位到flag

![1711b40e24ae3591d28598af680a0925](https://github.com/user-attachments/assets/01df4715-58e0-4604-8d7e-c2108fdec077)

提交时出现错误，注意到格式为NSSCTF{}，修改后成功

![30ef33d13489d51990a5c4a2c667bce6](https://github.com/user-attachments/assets/f2c11789-d37f-4e3a-bc1b-929fa9cba47b)

## [第五空间 2021]WebFTP
进入后没有任何线索

![QQ_1742747261521](https://github.com/user-attachments/assets/817901f5-3093-4068-a4e6-496941ccbe76)

查看标签后得知考察路径扫描，在dirsearch中输入python dirsearch.py -u http://node4.anna.nssctf.cn:28316/?m=login&a=in

![83ff773194b5ca7e8b94607bfad18709](https://github.com/user-attachments/assets/952b25b6-0411-40c5-b62d-2dcba2434df7)

得到以下目录

```
[17:45:31] 301 -   335B - /.git  ->  http://node4.anna.nssctf.cn:28316/.git/
[17:45:31] 200 -    3KB - /.git/
[17:45:31] 200 -   770B - /.git/branches/
[17:45:31] 200 -    73B - /.git/description
[17:45:31] 200 -   306B - /.git/config
[17:45:31] 200 -    23B - /.git/HEAD
[17:45:31] 200 -    3KB - /.git/hooks/
[17:45:31] 200 -   957B - /.git/info/
[17:45:31] 200 -   173B - /.git/logs/HEAD
[17:45:31] 200 -   240B - /.git/info/exclude
[17:45:31] 200 -    1KB - /.git/logs/
[17:45:31] 301 -   353B - /.git/logs/refs/remotes  ->  http://node4.anna.nssctf.cn:28316/.git/logs/refs/remotes/
[17:45:31] 301 -   351B - /.git/logs/refs/heads  ->  http://node4.anna.nssctf.cn:28316/.git/logs/refs/heads/
[17:45:31] 200 -   173B - /.git/logs/refs/remotes/origin/HEAD
[17:45:31] 200 -   173B - /.git/logs/refs/heads/master
[17:45:31] 301 -   345B - /.git/logs/refs  ->  http://node4.anna.nssctf.cn:28316/.git/logs/refs/
[17:45:31] 301 -   360B - /.git/logs/refs/remotes/origin  ->  http://node4.anna.nssctf.cn:28316/.git/logs/refs/remotes/origin/
[17:45:31] 200 -   114B - /.git/packed-refs
[17:45:31] 200 -    41B - /.git/refs/heads/master
[17:45:31] 200 -    1KB - /.git/refs/
[17:45:31] 200 -    3KB - /.git/objects/
[17:45:31] 301 -   346B - /.git/refs/heads  ->  http://node4.anna.nssctf.cn:28316/.git/refs/heads/
[17:45:31] 200 -   33KB - /.git/index
[17:45:31] 301 -   355B - /.git/refs/remotes/origin  ->  http://node4.anna.nssctf.cn:28316/.git/refs/remotes/origin/
[17:45:31] 301 -   348B - /.git/refs/remotes  ->  http://node4.anna.nssctf.cn:28316/.git/refs/remotes/
[17:45:31] 200 -    32B - /.git/refs/remotes/origin/HEAD
[17:45:31] 301 -   345B - /.git/refs/tags  ->  http://node4.anna.nssctf.cn:28316/.git/refs/tags/
[17:45:32] 403 -   294B - /.php
[17:45:32] 403 -   295B - /.php3
[17:45:43] 403 -   308B - /cgi-bin/awstats.pl
[17:45:43] 403 -   316B - /cgi-bin/a1stats/a1disp.cgi
[17:45:43] 403 -   298B - /cgi-bin/
[17:45:43] 403 -   308B - /cgi-bin/htmlscript
[17:45:43] 403 -   306B - /cgi-bin/awstats/
[17:45:43] 403 -   309B - /cgi-bin/htimage.exe?2,2
[17:45:43] 403 -   310B - /cgi-bin/imagemap.exe?2,2
[17:45:43] 403 -   314B - /cgi-bin/mt/mt-xmlrpc.cgi
[17:45:43] 403 -   315B - /cgi-bin/mt7/mt-xmlrpc.cgi
[17:45:43] 403 -   308B - /cgi-bin/index.html
[17:45:43] 403 -   308B - /cgi-bin/mt7/mt.cgi
[17:45:43] 403 -   311B - /cgi-bin/mt-xmlrpc.cgi
[17:45:43] 403 -   304B - /cgi-bin/mt.cgi
[17:45:43] 403 -   307B - /cgi-bin/login.cgi
[17:45:43] 403 -   303B - /cgi-bin/login
[17:45:43] 403 -   307B - /cgi-bin/mt/mt.cgi
[17:45:43] 403 -   307B - /cgi-bin/login.php
[17:45:43] 403 -   306B - /cgi-bin/test.cgi
[17:45:43] 403 -   305B - /cgi-bin/php.ini
[17:45:43] 403 -   306B - /cgi-bin/test-cgi
[17:45:43] 403 -   306B - /cgi-bin/printenv
[17:45:43] 403 -   309B - /cgi-bin/printenv.pl
[17:45:43] 403 -   309B - /cgi-bin/ViewLog.asp
[17:45:48] 200 -    83B - /index.php
[17:45:48] 200 -    83B - /index.php/login/
[17:45:49] 200 -   10KB - /LICENSE
[17:45:50] 200 -   14KB - /logo
[17:45:53] 200 -   78KB - /phpinfo.php
[17:45:55] 301 -   337B - /Readme  ->  http://node4.anna.nssctf.cn:28316/Readme/
[17:45:55] 200 -    3KB - /README.md
[17:45:56] 403 -   303B - /server-status
[17:45:56] 403 -   304B - /server-status/
[17:45:59] 200 -   33KB - /upload
```

注意到phpinfo.php,访问一下看看，发现有很多内容，通过CTRL+F搜索flag，解决

![d82e1db74f6294d5195689d58dc2e6aa](https://github.com/user-attachments/assets/c7c9b3ca-5900-4061-be57-4fb3cb278da7)

## [SWPUCTF 2021 新生赛]easyrce [LitCTF 2023]Ping
此题考察命令执行，注意到eval这个很危险的函数，可以考虑直接使⽤系统调⽤system()来得到系统信息,利用ls列出目录

?url=system("ls /");

![0319e6ed350a011a2009869933f3424c](https://github.com/user-attachments/assets/3cab2343-680f-4897-a36f-e2904ee400d0)

得到以下界面，看起来像是linux系统的目录，注意到显眼的flag目录，直接调⽤cat函数输出结果

?url=system("cat /flllllaaaaaaggggggg");

![QQ_1742747946600](https://github.com/user-attachments/assets/c3a42ebf-25d4-4de7-b25c-636da6112779)

得到flag

![QQ_1742748102225](https://github.com/user-attachments/assets/af0b7dd2-8619-406a-baa6-a5ee3cc15138)

## [SWPUCTF 2022 新生赛]奇妙的MD5
奇妙的字符串，不知道是什么，检索后了解到为ffifdyop

![ba3d52103784e5c7094102b022e48951](https://github.com/user-attachments/assets/e7ddee13-74d9-41e3-8535-cebfa62ebc47)

原因是

```
经过md5加密后：276f722736c95d99e921722cf9ed621c
再转换为字符串：‘or’6<乱码> 即 ‘or’66�]��!r,��b
用途：
select * from admin where password=’‘or’6<乱码>’
就相当于select * from admin where password=’'or 1 实现sql注入
```

进入后按F12调用查看器，发现有个md5弱类型比较

![9d1f231a40ce78e79ca93c2b4dfa03bd](https://github.com/user-attachments/assets/80c8b562-b061-4442-bfd0-b621b46bacba)

直接构造/?x=aabg7XSs&y=s214587387a

![1100662460ee3b1a473a21ca83d657d0](https://github.com/user-attachments/assets/db0b55d1-f3f7-4c6f-a7cf-83728b77604b)

原理为
以上字符进行 md5 后加密的数值为 0E，开头在 PHP 中会被识别为科学计数法，所有 0E 开头的数据进行弱类型比较皆为 True

以下为一些常用的MD5加密后以0E开头的
```
QNKCDZO

240610708

byGcY

sonZ7y

aabg7XSs

aabC9RqS

s878926199a

s155964671a

s214587387a

s1091221200a
```

进入下一界面后发现是md5强类型比较，由于md5不能加密数组，传入数组会报错，但会继续执行并且返回结果为null，所以可以用数组绕过，payload wqh[]=1&dsy[]=2，（即null === null），得到flag

![6e9fb4a309c4fefd825004546ab837f6](https://github.com/user-attachments/assets/3c534b11-42a1-4d77-8b9b-9baf3bb4a439)

![6a6cef8741896a83f1a0be975ed3cb90](https://github.com/user-attachments/assets/502dc3ce-d7e5-4d16-864c-26e0149b7e09)

## [BJDCTF 2020]easy_md5
此题与上一题完全一样，数组绕过即可

![e6d98c39403a6597cd2a0559f01fcd1d](https://github.com/user-attachments/assets/d4f7f7d0-639e-4104-8da0-0af3296dc478)

![4327bc40a2378e19a3d3ac83849efdae](https://github.com/user-attachments/assets/5da372ac-16d6-406e-89ba-5bab60b278af)

![3b6d36e23336d87b647c32f41790b5c2](https://github.com/user-attachments/assets/3e7a706e-d33a-4040-9ed8-ba34dd7e07d1)
