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

#### [BJDCTF 2020]easy_md5


### 学习
#### rce绕过

#### 文件包含
