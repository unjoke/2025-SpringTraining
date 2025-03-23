# 刷题
## <font style="color:rgb(0, 0, 0);">[LitCTF 2023]Follow me and hack me</font>
![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742282498420-96590239-b045-408b-94e1-f30adcf2e256.png)

简单的GET和POST还有改cookie

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742282990412-38aa37e4-e338-4d9c-b9b5-a8703e4e04f7.png)

有彩蛋！扫你目录

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742282780502-3d941721-d657-464d-b99d-11df5fb37cff.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742283017838-7311adca-b8a6-4935-82b7-a7480c3d84c4.png)

彩蛋（虽然我没GET到什么含义）：

```php
<?php
    // 第三个彩蛋！(看过头号玩家么？)
    // _R3ady_Pl4yer_000ne_ (3/?)
?>
```

burp写法

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742283918229-a5306ad4-2c02-4283-850c-ce3f6581f40c.png)

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742284080149-6866d247-9a7f-41e4-b2a3-c8cc77730bed.png)Yakit写法：

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742283260367-8b69135b-5354-4665-8473-45a5939ebebd.png)

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742283486300-60309929-46c1-482f-8b81-a00569de4c62.png)

## <font style="color:rgb(0, 0, 0);">[SWPUCTF 2021 新生赛]Do_you_know_http</font>
![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742284200057-073d9253-2fe5-4293-82f3-a62ce892bca4.png)

在这里也可以改包

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742284383447-6e941b82-d5eb-4ccc-a6fe-3ae5b7693289.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742284934428-6c10edb7-4858-4f23-bb05-3576c8478288.png)访问这个文件就行了，secretttt.php![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742284951707-67c7f19a-78bf-42d1-afa0-8d629b92a025.png)

yakit解法：

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742285112040-a49a3945-8e80-4cc0-8710-426f9f969146.png)

重定向到了a.php，访问a.php![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742285167397-7c773ed6-3b49-4c04-a29b-ca52426ded7c.png)

可以了，访问secretttt.php

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742285196623-51cbea51-2b12-4a09-854e-38c646ac9fa9.png)

行了

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742285239582-976121e2-bd6e-4767-a7cd-39e31b148e34.png)

Burp解法

改User-Agent: WLLM

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742285314781-31f0dc41-6358-407c-9874-fb3e803e15f7.png)

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742285354435-734529d2-935c-4781-86a3-68687cf16dec.png)访问a.php，继续本地访问

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742285419128-e2199ca9-7d00-49c7-9a5b-401ab70d3796.png)

访问secretttt.php

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742285450910-917580a0-aaa3-4690-bd64-7ff13973b325.png)

## <font style="color:rgb(0, 0, 0);">[LitCTF 2023]就当无事发生</font>
考的是git泄露，一般来说是要扫网站目录的，但是这道题直接把网站给出来了，还是GitHub上的，那就不需要了

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742287084460-601bf63c-e156-4d3d-8069-c96183440fc7.png)

访问一下[https://ProbiusOfficial.github.io](https://ProbiusOfficial.github.io)

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742286974685-5bbbb367-6207-49f6-949d-b966a44c599a.png)

找他GitHub

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742287161937-e0f33ae2-844a-43ee-abf4-c4bda15f3226.png)

看提交记录![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742287185214-7a3ad195-faab-407b-84f9-9e8010ecf9aa.png)

看看版本及时间，方便找flag

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742286943655-8a53fcd9-7269-45c4-a8ab-c5dd61e7102d.png)

找23年45月份的，Flag在此

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742287110602-372c97e2-d986-4618-a508-d352ffbddc7d.png)

## <font style="color:rgb(0, 0, 0);">[第五空间 2021]WebFTP</font>
看标签是git泄露，所以目录扫描一下

py dirsearch.py -u http://node7.anna.nssctf.cn:24629/

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742288819588-8f766bb5-307c-4c44-87fd-151b60277364.png)

访问README.md

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742288803245-af96f404-c8ad-470a-ab0a-7286af04e003.png)

找到用户名和密码

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742289023634-338561df-1969-4cff-9772-f2d12cb91626.png)

找flag

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742289040913-10fc6f66-e818-42f1-9b82-d7f4b9987cb2.png)

之后查看phpinfo.php找flag就行

Ctrl+f全局搜索flag。搜NSSCTF也行![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742289296273-04b21aea-7767-4f0b-b0f2-615f3104e128.png)

在 dirsearch中又看到phpinfo.php，访问一下还是刚才的页面，刚才的步骤麻烦了![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742289311932-1c161b7f-40cd-4b79-b899-b927b45e1f21.png)

## <font style="color:rgb(0, 0, 0);">[SWPUCTF 2021 新生赛]easyrce</font>
![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742288140022-aa885f2c-e4f1-4f6e-afdb-1bc34da008c4.png)

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742288175231-323aba4a-053e-479f-84ec-368e953de2f3.png)

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742288205112-2beefb72-998c-4f39-af71-b17b04fc3df9.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742288222987-568b72b9-70c5-41f0-bd7d-83c6308a80f2.png)

## <font style="color:rgb(0, 0, 0);">[LitCTF 2023]Ping</font>
ping命令通常用来作为网络可用性的检查。ping命令可以对一个网络地址发送测试数据包，看该网络地址是否有响应并统计响应时间，以此测试网络。

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742290804173-04bd1f3f-4f23-4e3a-b7dd-88b1dccae9d5.png)

分号隔开，看看文件

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742290824878-08cc78c7-5604-4925-b1f8-1c731941baea.png)

ls看看根目录

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742290966166-5d38530c-5850-4ad5-b3c0-417d0776c774.png)

cat /flag

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742291038267-9c39b73d-ba51-4dbc-97c6-c8c951b40b70.png)

## <font style="color:rgb(0, 0, 0);">[SWPUCTF 2022 新生赛]奇妙的MD5</font>
![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742291212989-4e8ce82c-c34b-4339-8913-721ba6d62303.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742291242327-4ac612c4-e4e4-419b-96f6-570ef6c2b82d.png)

php弱比较，数组绕过

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742291286547-5c625893-deab-45c1-b615-56f608350ddb.png)

还是数组绕过

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742291580536-3f01cd27-3c48-4fc0-b450-6068e04b56b4.png)

## <font style="color:rgb(0, 0, 0);">[BJDCTF 2020]easy_md5</font>
相应标头有hint

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742291866525-87cbd11d-1c5e-40f0-8b3f-b7d33beb58ce.png)

提交一下

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742291911604-d637a1c5-7d2e-4578-85c3-872cd9afe77a.png)

出现了这个

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742292057505-580c0f56-aaf5-4431-a952-3527b3e14ce9.png)

看源代码

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742292180717-2f2c7364-c817-46cf-88d3-cdc7f2ea1def.png)

还是弱比较,数组绕过,或者也可以直接访问levell14.php也行,

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742292264323-d250e12a-d3d1-4cb2-b323-c5acefea4725.png)

出现了这个页面

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742292273640-cead8cf4-9d22-48a4-9714-83305fccf1d2.png)

数组绕过

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1742292547508-1e598cc6-6766-4e5e-a0b1-2c176f001b52.png)

