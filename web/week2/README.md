# WEB第二周内容

## 题目

这些题目是要求一定写完的，包含了上周的一些知识点

* http（hackerbar和burpsite都尝试用一下）

  [[LitCTF 2023]Follow me and hack me](https://www.nssctf.cn/problem/3864)

  [[SWPUCTF 2021 新生赛]Do_you_know_http](https://www.nssctf.cn/problem/385)

* 路径扫描，git泄露

  [[LitCTF 2023]就当无事发生](https://www.nssctf.cn/problem/3862)

  [[第五空间 2021]WebFTP](https://www.nssctf.cn/problem/344)

* 命令执行
  [[SWPUCTF 2021 新生赛]easyrce](https://www.nssctf.cn/problem/424)
  [[LitCTF 2023]Ping](https://www.nssctf.cn/problem/3873)

* php散题

  [[SWPUCTF 2022 新生赛]奇妙的MD5](https://www.nssctf.cn/problem/2638)

  [[BJDCTF 2020]easy_md5](https://www.nssctf.cn/problem/713)

## 学习要求

* 学ctf比较重要的一点就是学会自己搜索学习。

* 下面列出学习方向：
  1. 强化学习**rce命令执行**，掌握一些过滤绕过方法；
  
     完成在文件夹docs里的题目（给予docker的构建文件，自行使用docker部署到本地做）
  
  2. 学习**文件包含**，这里分享一篇文章
     [文件包含漏洞详解](https://blog.csdn.net/weixin_58849785/article/details/137630067)；
     并完成PHPinclude_labs的`level0`到`level11`，有能力的全部做完。
     这里提供源码位置[PHPinclude-labs](https://github.com/ProbiusOfficial/PHPinclude-labs),可自行构建docker镜像，也可以直接使用我在docker hub发布的已构建的镜像（直接搜chrizsty/include_labs,或者访问镜像地址[chrizsty/include_labs](https://hub.docker.com/r/chrizsty/include_labs)下载）
  
* 学有余力的同学，可以准备开始学习sql注入的基础知识，目前先以mysql数据库为主，同样分享一篇文章
  [SQL注入入门](https://hello-ctf.com/hc-web/sql_injection/)