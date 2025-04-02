## 攻防世界基础题单：view_source
进入场景==>F12==>菜单中的“元素”==>查看网页前端代码即可获得flag
![屏幕截图 2025-03-11 201320](https://github.com/user-attachments/assets/3b4bb7ee-33c3-4b87-9744-eca484ea3faf)

## 攻防世界基础题单：get_post
### 1. get传参
   直接在地址栏后加?a=1,回车
### 2. post传参

   在hackbar中
   ![屏幕截图 2025-03-11 200918](https://github.com/user-attachments/assets/a1881b63-6443-406a-945a-9a949a867f62)


## 本次作业flag 
![34196413227125(1)](https://github.com/user-attachments/assets/5e5e4630-6c7d-4474-9d21-be66c4004c33)

flag{we1c0me_t0_CTF!}
## python脚本
```
import hashlib
def find_md5_with_prefix(prefix):
    target_prefix = prefix
    i = 0
    while True:
        # 将整数转换为字符串并进行MD5加密
        original_value = str(i)
        md5_hash = hashlib.md5(original_value.encode()).hexdigest()

        # 检查MD5哈希的前6位是否匹配目标前缀
        if md5_hash[:6] == target_prefix:
            print(f"找到匹配的值: {original_value}")
            print(f"MD5哈希: {md5_hash}")
            break

        i += 1
if __name__ == "__main__":
    find_md5_with_prefix("19ca14")
```

## php一些基础语法学习（寒假学了一点点没做笔记，敲了个登录就当作业吧QwQ）
```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>登录</title>
		<link rel="stylesheet" type="text/css" href="./login.css">
		<?php
			$connect=mysqli_connect("localhost","root","root");//打开一个数据库连接
			$select=mysqli_select_db($connect,"data");//设置或更改连接的默认数据库，后面接数据库名
			//var_dump($select);//查看连接是否成功
			mysqli_query($connect,"SET NAMES 'utf8'");//设置字符集为utf8*/
		?>
	</head>
	<body class="container1">
		<form action="<?php echo htmlspecialchars($_SERVER["PHP_SELF"]);?>" method="post" autocomplete="on">
			<table class="container2">
				<tr>
					<td><h1>登录</h1></td>
				</tr>
				<tr>
					<td><input type="text" placeholder="用户名" class="username" name="username"></td>	
				</tr>
				<tr>
					<td><input type="password" placeholder="密码" class="passwd" name="password"></td>	
				</tr>
				<tr>
					<td><input type="submit" class="container3" value="Login" name="login"></input></td>
				</tr>
				<tr>
					<td>没有账号去<a href="register.php">注册</a></td>
				</tr>
				<tr>
				 <td>
					<div class="check">
						<?php
							if(isset($_POST["login"]))
							{
								$username=$_POST["username"];
								$password=$_POST["password"];
								$check="select password from users where username='$username'";//因为设置了密码不能为空，所以如果查出来有值说明注册过，没有值说明没注册过
								$result=mysqli_query($connect,$check);
								$arr=mysqli_fetch_row($result);
								if($arr!=NULL&&$arr[0]==$password)
								{
									echo "<script>alert(\"登陆成功\");</script>";
								}else
								{
									echo "<script>alert(\"密码错误\");</script>";
								}
							}
						?>
					</div>
				 </td>	
				</tr>
			</table>
		</form>
	</body>
</html>
```

## RCE_writeup
### easyrce
![203444756791729(1)](https://github.com/user-attachments/assets/2e596578-09c2-40ee-8404-64b7aacaa53a)

可以看到源码中要以get方式传一个url参数，然后将其放入了eval（）函数中，此函数能将括号中的字符串视为命令运行
常见的系统命令执行函数还有
passthru()

exec()

shell_exec()

popen()

proc_open()

pcntl_exec()
考虑上传系统命令查看根目录
![556926102365644(1)](https://github.com/user-attachments/assets/434ae80c-ccf2-4edd-aee4-7dea8e3f2a50)


查看flag文件
![158316197493369(1)](https://github.com/user-attachments/assets/cbf1a5c5-1fbb-45b2-a15c-a0beae1ec11f)


### jicao
![213308334322052(1)](https://github.com/user-attachments/assets/f0c8b393-41c6-4e6d-ac09-d509f00cc781)

由源码可知要求以post传参上传一个id=wllmNB，以get传参上传一个一个json数组，且键为'x'的值为'wllm'![570255906128593(1)](https://github.com/user-attachments/assets/c71e250f-fd0b-41cb-a627-9a06bbef3a31)

### caidao
![266505649215211(1)](https://github.com/user-attachments/assets/c1174847-d79c-4da1-95ac-8e5058b773db)

要以post传参一个wllm,用hackbar传一个wllm=systen("ls /");![屏幕截图 2025-03-11 190946](https://github.com/user-attachments/assets/b8ccf856-4efb-4953-aa51-feb79e22e8ae)

然后查看flag文件
![109075908279927](https://github.com/user-attachments/assets/fd7838ee-430a-41ad-87ae-6e3b15861c28)

### babyrce
![175781854130549(1)](https://github.com/user-attachments/assets/d268f2cd-79d9-477f-b27e-d46e085ab1c2)

根据源码提示，用burp抓包后（hackbar也行）cookie中admin=1
![207644774099732(1)](https://github.com/user-attachments/assets/80148cc4-9747-4c48-98f1-212082f9f308)

得到进一步的代码
php基础薄弱，不知道preg_match的作用
![301343709447759(1)](https://github.com/user-attachments/assets/94e2356e-1940-40a0-be62-332c9896a05c)


搜索后得知preg_match在php中用于执行正则表达式，参数有 pattern--正则表达式模式,subject--要搜索的字符串, matches--如果提供，匹配结果将储存在此数组中,flags--用于控制匹配行为的标志，offset--从字符串指定位置开始搜索
那么代码中的preg_match就应该是检查ip中是否存在空格
由于查看根目录中的命令含有空格，所以看了下别人的wp得知${IFS}可以代替空格
![504115924039699(1)](https://github.com/user-attachments/assets/ebb919fd-87c5-46d1-9bc4-d68206f250b6)
![屏幕截图 2025-03-11 200239](https://github.com/user-attachments/assets/9f55b255-6685-45c6-96f7-643c0809654c)




