http:

1.[[LitCTF 2023]Follow me and hack me](https://www.nssctf.cn/problem/3864)

![](https://cdn.nlark.com/yuque/0/2025/png/54389986/1742699723611-60a604d3-f014-42b9-9109-a99772daf293.png)

基本的get post请求

2.[[SWPUCTF 2021 新生赛]Do_you_know_http](https://www.nssctf.cn/problem/385)

![](https://cdn.nlark.com/yuque/0/2025/png/54389986/1742699880243-c7e07a69-0831-4ea4-a80d-fb8f20d1496f.png)

题目让使用WLLM浏览器，利用BP抓包，修改User-Agent,之后它让本地查看，更改IP地址，访问它提供的地址即可。

3.[[LitCTF 2023]就当无事发生](https://www.nssctf.cn/problem/3862)

![](https://cdn.nlark.com/yuque/0/2025/png/54389986/1742700098874-d8d2def9-5ea5-4c30-877c-5e5b92525d5f.png)

访问它给的网站，进入github，查找项目文件，文件太多了(B站上有这人账号，看他视频定位了文件时间)

5.[[SWPUCTF 2021 新生赛]easyrce](https://www.nssctf.cn/problem/424)<font style="color:rgb(240, 246, 252);background-color:rgb(13, 17, 23);"> </font>[[LitCTF 2023]Ping](https://www.nssctf.cn/problem/3873)

![](https://cdn.nlark.com/yuque/0/2025/png/54389986/1742700344644-2f5c0268-4891-447a-a7c8-14dd04bc7930.png)

看题如果传入一个参数url，eval下会将它作为一行执行命令，后面根据需要添加命令。用ls列出全部文件，找到flag文件。

![](https://cdn.nlark.com/yuque/0/2025/png/54389986/1742700630920-33085365-2b0c-425b-9e5b-b5c502d86f32.png)

查看flag文件，即可。

6.奇妙的md5

![](https://cdn.nlark.com/yuque/0/2025/png/54389986/1742702496963-e696ff31-8b12-4ef4-9415-635715c6d61b.png)

叫提交一串神奇的md5字符串（我也不知道啥玩意儿神奇的字符串，于是直接利用Internet）

![](https://cdn.nlark.com/yuque/0/2025/png/54389986/1742702606095-843bfbb4-d896-46e8-80b6-3b20b54a982c.png)

查看源代码，发现它传入两个值不相等的参数，且在md5下相等，md5无法处理数组，如果传参为数组元素，则都会返回NULL，实现绕过。

![](https://cdn.nlark.com/yuque/0/2025/png/54389986/1742702799877-2c7181cd-d7fa-4820-b02e-de9174ac68af.png)

依然是数组绕过，即可得到flag。

7.easy_md5

![](https://cdn.nlark.com/yuque/0/2025/png/54389986/1742702855080-2a74b1e8-5986-41b3-a9ec-7f6cead874f8.png)

![](https://cdn.nlark.com/yuque/0/2025/png/54389986/1742702863760-df539274-05c7-43c4-9fd9-9868084c960a.png)

和上道题一模一样，依然是弱比较、数组绕过。

