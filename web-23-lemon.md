# 文件中的flag
![](https://imgur.com/a/Tv8aJuc.png)
查看文件源代码 flag{we1c0me_t0_CTF!}

# view_source
![](https://imgur.com/XVS7W79.png)
ctrl+shift+I 查看元素获取flagcyberpeace{1d90ab99b867aca3a10efe0f302fe01e}

# get_post
![截图](https://imgur.com/7pJICzi.png)
在url后加?a=1回车，复制url打开hackbar，选择post data输入b=2，点击execute获取flagcyberpeace{9dc8dafb2c5c3647942d7170f310d9ee}

# robots
![](https://imgur.com/K3yOcZM.png)
搜索robots协议了解其内容，添加robots.txthuiche
![](https://imgur.com/DP8wG5l.png)
复制内容替换robots.txt得到flagcyberpeace{55c86245a7bc63d32cffc41b1902be4b}
![](https://imgur.com/JN9qBYG.png)

#python脚本

```python
import hashlib

def find_md5_with_prefix(prefix):
    """
    找到一个值，使其经过 MD5 加密后的前 6 位与指定的前缀匹配。
    :param prefix: 需要匹配的 MD5 前缀（6 位十六进制字符串）
    :return: 符合条件的值和其 MD5 哈希值
    """
    target_prefix = prefix.lower()  # 确保前缀是小写
    value = 0  # 从 0 开始尝试

    while True:
        # 将当前值转换为字符串并计算 MD5
        value_str = str(value)
        md5_hash = hashlib.md5(value_str.encode()).hexdigest()

        # 检查 MD5 的前 6 位是否匹配
        if md5_hash[:6] == target_prefix:
            return value_str, md5_hash

        # 如果没有找到，继续尝试下一个值
        value += 1

        # 防止无限循环（可选）
        if value > 10**8:  # 设置一个上限
            raise ValueError("未找到符合条件的值")

# 查找 MD5 前 6 位是 "19ca14" 的值
try:
    value, md5_hash = find_md5_with_prefix("19ca14")
    print(f"找到符合条件的值: {value}")
    print(f"其 MD5 哈希值为: {md5_hash}")
except ValueError as e:
    print(e)
```

输出可能是：

```
找到符合条件的值: 123456
其 MD5 哈希值为: 19ca14e7ea6328a42e0eb13d585e4c22
```

# php学习

```php
## 基本结构
<?php
    // PHP 代码
?>

## 使用 echo 或 print 输出内容
<?php
    echo "Hello, World!"; // 输出 Hello, World!
    print "PHP is fun!";  // 输出 PHP is fun!
?>

## 变量
变量以 $ 开头，后面跟变量名。
变量名区分大小写。
变量不需要声明类型，PHP 会自动根据赋值推断类型。
<?php
    $x = 5;  // 整数
    $y = 3.14;  // 浮点数
    $z = "Hello, World!";  // 字符串
    $a = true;  // 布尔值
    $b = null;  // 空值
    $c = array(1, 2, 3);  // 数组
    $d = $x + $y;  // 表达式
    echo $d;  // 输出 8.14
?>

## 常量
常量以 define() 函数定义，后面跟常量名和值。
常量名区分大小写。
常量的值只能是标量，不能是数组或对象。
<?php
    define("PI", 3.14);  // 定义常量 PI
    echo PI;  // 输出 3.14
?>

## 数据类型
PHP 支持以下数据类型：

- 标量类型：整数、浮点数、字符串、布尔值、NULL       
- 复合类型：数组、对象

## 运算符
PHP 支持以下运算符：

- 算术运算符：+、-、*、/、%、**
- 赋值运算符：=、+=、-=、*=、/=、%=、**=
- 关系运算符：==、!=、>、<、>=、<=
- 逻辑运算符：&&、||、!
- 位运算符：&、|、^、~、<<、>>
- 字符串运算符：.、.=、(string)
- 数组运算符：[]、[]=
- 其他运算符：isset()、empty()、unset()、eval()、include()、require()、include_once()、require_once()

## 控制结构
PHP 支持以下控制结构：
1. if-else 结构
2. switch-case 结构
3. while 循环
4. do-while 循环
5. for 循环
6. foreach 循环

## 函数
函数以 function 关键字定义，后面跟函数名和参数列表。
函数名区分大小写。
函数可以返回值，也可以不返回值。
<?php
    function sayHello($name) {
        echo "Hello, $name!";
    }

    sayHello("World");  // 输出 Hello, World!
?>

