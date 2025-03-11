<!--flag{we1c0me_t0_CTF!}-->

## **view_source** 

![image-20250309230812899](img/image-20250309230812899.png)

![image-20250309230854553](img/image-20250309230854553.png)



根据提示，我们要打开网页的源代码，根据CSDN的提示，

## 1.右键选择检查 （或 F12 开发者工具）

## 2.view-source:url （或 crtl+u）

## 3.bp抓包发送查看响应

## 4.访问根目录下的index.phps

**点击f12**

![image-20250309234850455](img/image-20250309234850455.png)

![image-20250309235225791](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20250309235225791.png)

![image-20250309235347243](img/image-20250309235347243.png)

## get_post

GET请求通常用于请求数据，而不是提交数据来更改服务器上的资源。当使用GET请求提交数据时，数据会附加在URL后面，形成查询字符串。

示例

例如，要通过GET请求提交一个名为a，值为1的变量，可以在浏览器地址栏中输入以下URL

http://example.com/?a=1
这里，?a=1就是将变量a的值设置为1的查询字符串。

使用工具提交GET请求

如果需要在不直接修改浏览器地址栏的情况下提交GET请求，可以使用各种HTTP客户端工具。例如，可以使用Burp Suite等工具来修改和发送HTTP请求


注意事项

**GET请求中的数据会显示在URL中，因此不适合传输敏感信息。**

**URL长度有限制，过长的查询字符串可能会被截断。**

**浏览器和服务器通常会缓存GET请求的结果，这可能会影响到数据的实时性。**

![image-20250310003621941](img/image-20250310003621941.png)

根据提示用bp抓包

![image-20250310003739597](img/image-20250310003739597.png)

按照POST的形式，并在URL中对应位置填写get参数，包末尾填写post参数

**添加项目：**【**Content-Type: application/x-www-form-urlencoded**】

(powered by CSDN 嘻嘻)

![image-20250310004110658](img/image-20250310004110658.png)

需要注意，添加的那个项目要在请求包中间的位置，而POST变量写在结尾处，如果有多个参数则用&连接，不加问号直接写即可

![image-20250310005240168](img/image-20250310005240168.png)

## Robots

robots是告诉搜索引擎，你可以爬取收录我的什么页面，你不可以爬取和收录我的那些页面。robots很好的控制网站那些页面可以被爬取，那些页面不可以被爬取。

主流的搜索引擎都会遵守robots协议。并且robots协议是爬虫爬取网站第一个需要爬取的文件。爬虫爬取robots文件后，会读取上面的协议，并准守协议爬取网站，收录网站。

robots文件是一个纯文本文件，也就是常见的.txt文件。在这个文件中网站管理者可以声明该网站中**不想被robots访问**的部分，或者指定搜索引擎只收录指定的内容。因此，**robots的优化会直接影响到搜索引擎对网站的收录情况。**

robots文件必须要存放在网站的根目录下。 输入域名/robots.txt 即可访问。

![img](img/v2-c9ebc058762a05be45da6388eee4a31d_1440w.jpg)

例如，图示

user-agent这句代码表示那个搜索引擎准守协议。user-agent后面为搜索机器人名称，如果是“*”号，则泛指所有的搜索引擎机器人；案例中显示“User-agent: *” 表示所有搜索引擎准守，*号表示所有。

[Disallow](https://zhida.zhihu.com/search?content_id=164257492&content_type=Article&match_order=1&q=Disallow&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NDE3NTgyOTgsInEiOiJEaXNhbGxvdyIsInpoaWRhX3NvdXJjZSI6ImVudGl0eSIsImNvbnRlbnRfaWQiOjE2NDI1NzQ5MiwiY29udGVudF90eXBlIjoiQXJ0aWNsZSIsIm1hdGNoX29yZGVyIjoxLCJ6ZF90b2tlbiI6bnVsbH0.P3MBivIAJfiKUKirYECPX60gDuIvB-m-7uJq1xudhdU&zhida_source=entity)是禁止爬取的意思。Disallow后面是不允许访问文件目录（你可以理解为路径中包含改字符、都不会爬取）。案例中显示“Disallow: /?s*” 表示路径中带有“/?s”的路径都不能爬取。 *代表匹配所有。 这里需要主机。 Disallow空格一个，/必须为开头。

如果“Disallow: /” 因为所有路径都包含/ ，所以这表示禁止爬取网站所有内容。

**如果没有被禁止到的路径，默认为可以被爬取。**

## 关于robots的注意事项

1、不要禁止爬虫爬取网站的所有，因为从经验来看，如果屏蔽一次，解封后好一段时间爬虫都不会来你网站，收录成为问题。

2、代码后需要【冒号+空格+斜杆】 ，比如“Disallow: /*?* ”

3、当网站为静态路径时，需要屏蔽掉所有动态链接。网站中存在一种链接被收录即可，避免一个页面2个链接。代码如下“Disallow: /*?* ”表示禁止所有带 ?号的网址被爬取。通常动态网址带有“?”“=”等。

4、根据自己网站情况定，屏蔽不需要收录的网址。



因此根据提示，我们访问该网站的robots文件按格式修改网址

![image-20250310135421447](img/image-20250310135421447.png)

![image-20250310135516488](img/image-20250310135516488.png)

再根据提示查看php,得到答案

![image-20250310135600173](img/image-20250310135600173.png)

## AI脚本

```
import hashlib

def find_md5_with_prefix(prefix):
    target_prefix = prefix
    i = 0
    while True:
        # 将整数转换为字符串并进行MD5加密
        value = str(i)
        md5_hash = hashlib.md5(value.encode()).hexdigest()
        
        # 检查MD5哈希值的前6位是否匹配目标前缀
        if md5_hash[:6] == target_prefix:
            print(f"找到满足条件的值: {value}")
            print(f"对应的MD5哈希值: {md5_hash}")
            break
        
        i += 1

if __name__ == "__main__":
    find_md5_with_prefix("19ca14")
```

加密:将明文数据通过一系列算法变成密文数据(目的就是为了数据的安全) 加密算法:md系列 sha系列 base系列 hmac系列

要让AI输出这段利用了md5的代码，要导入了Python的`hashlib`库，这个库提供了很多加密算法，当然也包括MD5。

接着AI定义了一个函数，名为`find_md5_with_prefix`，它接受一个参数`prefix`，这个参数是你想要匹配的MD5哈希值的前6位。DEF即DEFINE

将传入的`prefix`赋值给`target_prefix`，并初始化一个变量`i`，从0开始。进入循环。

将整数`i`转换为字符串`value`，然后对这个字符串进行MD5加密，生成一个32位的十六进制哈希值`md5_hash`。检查生成的MD5哈希值的前6位是否与`target_prefix`匹配。如果匹配，就执行下面的代码。如果找到满足条件的值，就打印这个值和对应的MD5哈希值，并使用`break`退出循环。没有找到满足条件的值，就将`i`加1，继续下一次循环。

至于最后，![image-20250310225757299](img/image-20250310225757299.png)

这是Python程序的入口点。当直接运行这个脚本时，会调用`find_md5_with_prefix`函数，并传入参数`"19ca14"`，即寻找一个数字，使得其MD5哈希值的前6位是`"19ca14"`。大致就是暴力破解的一个过程。

![image-20250310235517904](img/image-20250310235517904.png)

![image-20250310235610622](img/image-20250310235610622.png)

然后金币不够，做不下去了......   学生认证审核中

## 基本的 PHP 语法

PHP 文件的默认文件扩展名是 **.php**。

PHP 文件通常包含 HTML 标签和一些 PHP 脚本代码。

PHP 中的每个代码行都必须以分号结束。分号是一种分隔符，用于把指令集区分开来。

通过 PHP，有两种在浏览器输出文本的基础指令：**echo** 和 **print**。

注释方式和C语言一致

<!DOCTYPE html>
<html>
<body>

<h1>我的第一个 PHP 页面</h1>

```
<!DOCTYPE html>
<html>
<body>

<h1>我的第一个 PHP 页面</h1>

<？php
echo “Hello World！”;
？>

</body>
</html>

```

以这个代码为例

![image-20250311122546951](img/image-20250311122546951.png)

DOCTYPE是document type的简写。它是一种标记语言的文档类型声明，即告诉浏览器当前 HTML 是用什么版本编写的。

<html> 标签告知浏览器这是一个 HTML 文档。

<html> 标签是 HTML 文档中最外层的元素。

`<body>` 标签定义文档的主体。

`<h1>` 到 `<h6>` 标签用于定义 HTML 标题。

`<h1>` 定义最重要的标题。`<h6>` 定义最不重要的标题。

echo（） 函数输出一个或多个字符串。

**注释：**echo（） 函数实际不是一个函数，所以不必对它使用括号。echo（） 函数比 [print（）](https://www.runoob.com/php/func-string-print.html) 速度稍快。

*php先更新到这里，鼠鼠水平不高，配置phpstorm的时候遇到了很多很多问题，因此pr的速度慢了亿些，大佬们不要嫌弃鼠鼠*，鼠鼠后续会加快学习PHP的速度。

![84e903110202374ddae2efd6b5565ad](img/84e903110202374ddae2efd6b5565ad.jpg)
