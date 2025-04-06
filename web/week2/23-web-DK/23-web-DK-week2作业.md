# 题目

## HTTP

[https://www.nssctf.cn/problem/3864](https://www.nssctf.cn/problem/3864)

法1：HACKBAR
![image](https://github.com/user-attachments/assets/0cc0a3b1-beb2-477b-903a-00d5477e49a5)

法2：BP抓包修改

代理开始抓包，发送到repeater中，修改头文件使用POST方法，在请求参数中添加CTF=Lit2023

之后我们需要手动构造一下POST包的结构，如图添加两行代码，之后在请求体里添加Challenge=i%27m_c0m1ng![image](https://github.com/user-attachments/assets/2bea4e66-21bb-4933-9056-5d0fa1e2556c)

题目二

[https://www.nssctf.cn/problem/385](https://www.nssctf.cn/problem/385)
一开始提示说使用WLLM浏览器，不知道是什么，问了AI后是user-agent值，该值用于标识发送请求客户端应用程序的相关信息。修改后拿到下一个地址
![image](https://github.com/user-attachments/assets/dc799b6d-084a-40d2-84ec-22c8471f348d)
![image](https://github.com/user-attachments/assets/abf27436-f306-41d5-81a5-d80e4e5cf389)
提示说使用本地访问,修改包，添加下面的代码，功能是伪造IP地址

```
X-Forwarded-For:127.0.0.1
```
![image](https://github.com/user-attachments/assets/8cfffe2d-05e1-4ba3-be85-d4687a0bdda4)

访问新地址拿到FLAG

## 路径扫描，GIT泄露
### webFTP
使用dirsearch扫描
![image](https://github.com/user-attachments/assets/80989e6d-2d9f-4e6e-a043-9a9ed8bd0934)
发现敏感文件phpinfo，进入后即拿到FLAG

## 命令执行

[https://www.nssctf.cn/problem/424](https://www.nssctf.cn/problem/424)

```
?url=phpinfo();    //看一下有没有关键词屏蔽，没想到直接出来
?url=system('ls');    //查看下当前目录的文件，没有发现flag
?url=system('ls /');     //查看下根目录，发现了flllllaaaaaaggggggg文件
?url=system('tac /flllllaaaaaaggggggg');   //直接拿到flag
```

![](https://cdn.nlark.com/yuque/0/2025/png/42695688/1742720192564-1d1ac121-22e8-44fb-a001-6c894cccb780.png)

## PHP散题

[https://www.nssctf.cn/problem/2638](https://www.nssctf.cn/problem/2638)

看了提示之后，发现是MD5绕过，没见过这种题目，立刻上网搜索

[https://blog.csdn.net/weixin_50012220/article/details/119462700](https://blog.csdn.net/weixin_50012220/article/details/119462700)

第一关输入ffifdyop即可，具体原理看网站。

第二关查看源代码，发现需要get传入两个x和Y参数，同时x!=y，但是md5值相同。同样用网站中的方法

```
?x=QNKCDZO&y=240610708
```

第三关后查看源码，是用POST请求传递两个数组dsy和wqh，完成绕过

```
wqh[]=1&dsy[]=2
```

原理解释：提交两个空数组后，会返回报错NULL，在进行==比较时，不仅值相等而且类型也相等，于是达到了绕过的效果

[https://www.nssctf.cn/problem/713](https://www.nssctf.cn/problem/713)

第一关，和上题界面一样，我们输入ffifdyop即可进入。

第二关查看源码，输入两个不相等的值，但MD5加密后值相等(弱类型比较）

```
?a=QNKCDZO&b=240610708    //失败
?a=CbDLytmyGm2xQyaLNhWn & b=770hQgrBOjrcqftrlaZk     //
直接Load然后输入下一关的url地址
http://node4.anna.nssctf.cn:28623/levell14.php    //执行成功
```

有点难绷，感觉是题目崩溃了，但是进入第三关了，是POST强类型比较，和上一题类似,不写payload了

