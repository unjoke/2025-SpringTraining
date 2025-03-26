学习了一些linux的指令,最基本的有ls, cat , nc.

nc，是用来连接服务器的，后面输入地址就可以开始做题

ls，可以显示目录的内容，这个目录下面有什么东西

cat,可以直接查看文件的内容，可以cat flag,得到答案

第一道题是test nc ,先启动靶机和虚拟机，先要连上服务器

连上服务器后，试一下ls指令

可以看到出现了flag,用cat指令得到flag
![屏幕截图 2025-03-22 194058](https://github.com/user-attachments/assets/3be0312c-b562-40ac-a4c0-c96ed21aa5ae)

注意提交flag时要都要提交，提交flag{...}

第一题主要是熟悉linux中基本指令怎么运用，以及拿到一道题时应该干什么，基本操作是什么。

第二题：

首先按基本操作，连上服务机，出来了nss 和 问候的话 
![屏幕截图 2025-03-22 194928](https://github.com/user-attachments/assets/8309069b-573a-4738-b354-ae601ec8f754)

然后用ls指令 
![屏幕截图 2025-03-25 192431](https://github.com/user-attachments/assets/b74240b4-909b-46f5-8940-37d81966273f)

发现并没有flag，一个一个试，试到pwn的时候发现是乱码，看视频上说可以下载一下附件，用ida分析一下，这道题没有附件。试一试上面的目录，cd进入目录 
![屏幕截图 2025-03-25 195515](https://github.com/user-attachments/assets/9060a089-0a95-4bd5-a5d4-77b8e02d54bc)

cd bin的话出来了三个指令，应该不行

cd dev的时候这好像也是指令，感觉跟bin的差不多 
![屏幕截图 2025-03-25 195646](https://github.com/user-attachments/assets/5dd92661-bbc4-486a-9019-9257f4ea353c)

cd lib 的时候出来了一堆东西看着好像是系统里的东西，也有一点指令。 

![屏幕截图 2025-03-25 192431](https://github.com/user-attachments/assets/ce923c03-5e34-470a-8754-1fb692fa3a42)

cd nss的时候出来了ctf，感觉比前面的靠谱一点，cat 直接查看一下，cat ctf,说ctf是一个目录，再打开 下ctf，看看里面有什么。里面只有一个flag,看来试对了。 
![屏幕截图 2025-03-25 195901](https://github.com/user-attachments/assets/ee07d066-12b9-43db-91fc-84fcd00bc4c0)

cat flag,得到flag。这样答案就得到了


视频题目：nc签到

打开题目，连上服务器。发现了乱码，想把附件用ida打开，但打开的时候没有这个选项，它有那个直接的print就用vs打开

打开后分析一下想表达的意思，可以发现ls,cat,cd,还有这个doller ifs都不能用

知道那个doller ifs是空格后要想办法看怎么运行这些指令。

看了对应视频对应文章后知道了可以通过在指令中间添加\（无效字符）来绕过指令的读取，让指令运行，先用ls试一下，得到了对应的结果。发现出现了flag
![屏幕截图 2025-03-26 6](https://github.com/user-attachments/assets/171664bf-b462-400e-b8ae-20bb101e7655)


正常的cat指令是禁用的，加\行不行呢。可以看到加两个\是没有结果的，
![屏幕截图 2025-03-26 114655](https://github.com/user-attachments/assets/c4315e0c-157b-4006-bd76-4eb81142929e)

用空格替换一下，文章中说这个dollarIFSdollar9可以替换空格，试一下，得到了答案
![屏幕截图 2025-03-26 114346](https://github.com/user-attachments/assets/46dc50a4-5efb-48ee-998d-09194cdd1d26)







