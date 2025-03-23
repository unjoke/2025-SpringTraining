# 24-web-Younbrian
## 题目
### http
#### [LitCTF 2023]Follow me and hack me
用hackbar
![屏幕截图 2025-03-17 213600](https://github.com/user-attachments/assets/49a9e73d-460d-4c8c-b6df-0be215823f23)
用burpsuite抓包抓了几次都不行，翻了一下wp才知晓hackbar与burpsuite抓包的差异所在
![屏幕截图 2025-03-17 220032](https://github.com/user-attachments/assets/bb7eec7d-8a29-4c13-9063-e62088d73347)
于是我在用hackbar的同时，抓了一下包，对比差异
![屏幕截图 2025-03-17 220353](https://github.com/user-attachments/assets/e77049fb-8238-4e4f-9bd5-0a14563a9e1e)
正如wp中所说，自己抓包时未成功就是没有构造content-type
于是把content-type复制，重新抓包时粘贴，得到flag

#### [SWPUCTF 2021 新生赛]Do_you_know_http
![屏幕截图 2025-03-17 222204](https://github.com/user-attachments/assets/2e451f07-46de-4658-a9fa-0c9bd1e3bac0)
一开始提示用'WLLM'browser,browser有浏览器的意思，但是刚U-A开始属实是没有头绪，甚至产生过去找找是不是真的有一个叫WLLM的浏览器的想法
但是突然看见题目中的http，浏览器与http挂钩只能是U-A头了，改了一下推进到下一步
![屏幕截图 2025-03-17 222310](https://github.com/user-attachments/assets/d493914e-dfc2-4048-9bf8-243f2162e149)
提示我只能用local访问
于是去搜索了一下http请求包中什么能够代表自己的ip

1.X-Forwarded-For（XFF）是一个 HTTP 扩展头部，用于标识通过代理或负载均衡器连接到 web 服务器的客户端的原始 IP 地址。虽然 HTTP/1.1 没有定义这个头部，但它已经成为事实上的标准，并被广泛应用于各种 HTTP 代理和负载均衡服务中。

2.X-Forwarded-For 的工作原理
当客户端直接连接到服务器时，其 IP 地址会被发送给服务器并记录在服务器的访问日志中。但如果客户端通过代理服务器进行连接，服务器只能看到最后一个代理服务器的 IP 地址。X-Forwarded-For 的出现就是为了向服务器提供更有用的客户端 IP 地址。

3.X-Forwarded-For 的格式
X-Forwarded-For: client, proxy1, proxy2
其中，列表中的第一个 IP 地址是原始客户端的 IP 地址，后面的 IP 地址则依次是每个代理服务器的 IP 地址。

除了XFF还有一个X-Real-IP
一些代理服务器和web应用程序使用此字段记录客户端的的真实IP
于是我在请求包中构造了一个XFF，最后得到flag
![屏幕截图 2025-03-18 101536](https://github.com/user-attachments/assets/c8713cad-441d-4e0b-96de-9a0ec083d2c1)
用hackbar
![屏幕截图 2025-03-18 102154](https://github.com/user-attachments/assets/2fa158c1-2e2c-4a7a-a465-f9bafbff91fc)

### 路径扫描，git泄露
#### [LitCTF 2023]就当无事发生
看着题目发布时间去翻了一下2023文件夹3，4，5月的更改记录
![屏幕截图 2025-03-18 000737](https://github.com/user-attachments/assets/d7d7174b-3ca5-421d-af48-0d4a8de03064)
#### [第五空间 2021]WebFTP
![屏幕截图 2025-03-18 171632](https://github.com/user-attachments/assets/c353548f-eeda-4f03-a5b4-608c8db487bc)
扫描出路径来无从下手（经验还是太少了QWQ），看了一下wp，才知道要访问哪个文件
![屏幕截图 2025-03-18 111906](https://github.com/user-attachments/assets/1a740683-8c32-48bc-bb0f-ab139e3268a4)

### 命令执行
#### [SWPUCTF 2021 新生赛]easyrce
![屏幕截图 2025-03-18 112503](https://github.com/user-attachments/assets/a4036992-fdfe-4e3f-bac3-cbb854981ea5)
![屏幕截图 2025-03-18 112533](https://github.com/user-attachments/assets/ea1c233b-895d-4229-8ff2-f7b8063294f7)
#### [LitCTF 2023]Ping
抓包
![屏幕截图 2025-03-18 113255](https://github.com/user-attachments/assets/5d2ebd3a-0c15-4483-a07d-0021daa33755)
![屏幕截图 2025-03-18 172447](https://github.com/user-attachments/assets/45c4662e-a22c-42f9-a589-c831b740550d)
看到一个javascript函数，应该是起过滤作用
问了一下AI那个正则
![屏幕截图 2025-03-18 114801](https://github.com/user-attachments/assets/b30e24f5-3a45-47d4-8bb3-6cd703e08b19)
然后把浏览器的javascript禁了，还是不行

又去搜了一下rce绕过过滤的一些方法(写在后面)，输入127.0.0.1|ls /得到目录列表

![屏幕截图 2025-03-18 124613](https://github.com/user-attachments/assets/58155404-4781-4767-b220-215c8db97de9)
![屏幕截图 2025-03-18 170701](https://github.com/user-attachments/assets/52db6c41-591c-446c-944e-3b4854a6989e)


### php散题
#### [SWPUCTF 2022 新生赛]奇妙的MD5
奇妙の字符串
![屏幕截图 2025-03-18 191620](https://github.com/user-attachments/assets/66a30884-2a31-48ec-8783-e16d4b13d18b)
![屏幕截图 2025-03-18 191939](https://github.com/user-attachments/assets/3f4f4572-3b47-42aa-b97a-9f0836b8d869)

![屏幕截图 2025-03-18 192229](https://github.com/user-attachments/assets/4d1c0226-4aef-4e20-a01a-67544261f011)
![屏幕截图 2025-03-18 204827](https://github.com/user-attachments/assets/bbdabe34-836a-4c35-a094-8db7478b2bbe)


#### [BJDCTF 2020]easy_md5
![屏幕截图 2025-03-18 205313](https://github.com/user-attachments/assets/48cede0b-068a-4963-a9be-8ee40e23b47f)

![屏幕截图 2025-03-18 205823](https://github.com/user-attachments/assets/95637f25-04ed-4262-916e-4edc6c3f8b0a)

![屏幕截图 2025-03-18 210011](https://github.com/user-attachments/assets/ea7cb8fc-5c94-41b8-8082-619fa7be6a20)
![屏幕截图 2025-03-18 210023](https://github.com/user-attachments/assets/4d0be5a7-17d6-49aa-af68-a481b9953741)


### 学习
#### 一些简单的rce绕过
1.空格绕过
cat /flag
$IFS等价空格

2.管道符

![屏幕截图 2025-03-18 175033](https://github.com/user-attachments/assets/b544268f-76ba-4b01-b7a1-4dd37a382acf)

3.反斜杠\绕过
如cat、ls被过滤，使用\绕过

c\at /flag

l\s /
4.取反绕过
![屏幕截图 2025-03-18 175606](https://github.com/user-attachments/assets/79df8de8-768e-43f9-bdf4-b8dd0dd0ed64)
![屏幕截图 2025-03-18 175642](https://github.com/user-attachments/assets/c869aa3d-7714-4725-aff7-a143c866d36a)

#### 文件包含
原理
在程序员开发过程中，通常可能会把可重复使用的函数写到单个文件中，在使用某些函数时，直接调用此文件，无需再编写，开发人员一般希望代码更灵活，所以将被包含的文件设置为变量，来进行动态调用，但是正是这种灵活性通过动态变量的方式引入需要包含的文件时，用户对这个变量可控且服务端没有做合理校验或被绕过

常见的文件包含函数
1.include()
只有代码执行导函数所在行才将文件包含进来，发生错误时给一个警告，然后向下继续

2.include_once()
功能和include()相同，区别在于程序调用同一文件时，只调用一次

3.require()
require()与include()的区别在于require()执行如果发生错误，函数会输出错误信息，并终止脚本的运行

4.require_once()
与include_once类似

5.highlight_file(),show_source()
函数对文件进行语法高亮显示，通常能看到源代码

6.readfile(),file_get_contents()
函数读取一个文件，并写入到输出缓冲

7.fopen()
打开一个文件或者url

文件包含分类
本地包含
即包含目标服务器本身的文件

远程文件包含
包含第三方服务器的文件
条件：php.ini中的配置选项allow_url_fopen和allow_url_include为on的话，则包含的文件可以是第三方服务器中的文件
如下图包含的文件为kali虚拟机上面的文件
注意：利用远程文件包含时，在远程服务器上被包含的文件一定不能是php文件否则达不到攻击目标服务器的效果
例：我们得知A服务器可以远程文件包含，此时只需在本地创建一个一句话木马的txt文件（导入非php文件仍按照php语法解析），然后打开apache,接着给A服务器传文件名参数时把本地一句话木马文件路径上传，然后访问，就能通过一句话木马执行命令控制A服务器，如果上传的是php文件，那么一句话木马最后作用于自己电脑上而不是目标服务器
