
# 工具安装
- [x] **Hackerbar**：便于发送简单的请求和编码转化等等，强大的功能和简单的使用让它在ctf圈里很出名；
- [x] **BurpSuite**：抓包工具，同时也有爆破的功能；
- [x] **Dirsearch**：路径扫描工具，具体用处试试就知道了
- [x] **浏览器开发者工具**（浏览器自带的，不用下）：没得喷，这个是真神；

# 题目
## **view_source**

直接F12打开控制台，即可查看FLAG
![Pasted image 20250311173812](https://github.com/user-attachments/assets/e8cad127-92e7-4f8f-838e-b7edc3f51fa6)

##  get_post

使用Hackbar，load一下，输入?a=1![Pasted image 20250311180941](https://github.com/user-attachments/assets/4dc50d4a-4b0e-40cd-a29b-261ddcc0202d)

使用POST方法，直接输入b=2![Pasted image 20250313090213](https://github.com/user-attachments/assets/e8c89bc7-8a24-47c4-8347-004a9ab5067b)

## robots协议
[[【CTF-知识】robots协议]]
看完了robots协议，知道这是一个限制爬虫的东西，也就是说要进行目录信息收集，直接使用dirsearch进行一个目录扫描。
发现有一个/robots.txt文件，在网页进行访问,拿到flag位置![Uploading Pasted image 20250313090213.png…]()

## NSSCTF-RCE-LAB
### level0
]![【RCE-LAB】L0](https://github.com/user-attachments/assets/9420ce41-1932-403a-a738-8dfb29bebfc7)

### L1
![【RCE-LAB】L1 1](https://github.com/user-attachments/assets/f90d05cb-892e-4a1e-8cae-c3433f9ea064)
![【RCE-LAB】L1 2](https://github.com/user-attachments/assets/88bef2bf-9e0b-4b1d-9729-29953ec4a99d)


