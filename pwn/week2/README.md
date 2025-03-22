1. 学习markdown基本语法 [Markdown 基本语法 | Markdown 教程](https://markdown.com.cn/basic-syntax/)，之后的笔记全用md文件记录，并选择一个md软件记录在上面，这边推荐几个软件：
  - Typora: https://github.com/amazingcoderpro/TyproaCrack/tree/main?tab=readme-ov-file
  - 语雀
  - 飞书
  - Obsidian
  上面一个软件选择一个即可，也可以自己找喜欢的。
2. 环境搭好后可以正式开始pwn的学习，首先要知道一些基本的linux命令的使用 [Linux 命令大全 | 菜鸟教程](https://www.runoob.com/linux/linux-command-manual.html)
  ```python
  nc IP PROT # 连接服务器
  cat flag # 输出flag文件
  ls # 显示当前文件夹内的内容
  cd ./your_file_name # 进入文件夹
  ```
  完成下列题目
  - https://buuoj.cn/challenges#test_your_nc
  - https://www.nssctf.cn/problem/2633
3. 现在开始pwn学习的第一种漏洞类型，栈溢出 [栈溢出原理 - CTF Wiki](https://ctf-wiki.org/pwn/linux/user-mode/stackoverflow/x86/stackoverflow-basic/)
  pwntools使用：https://cloud.tencent.com/developer/article/2384837
  **完成本周任务文件夹中的题目**
4. 完成上述任务后用md编写每周学习笔记并pr到对应的仓库中
5. 学有余力可以去了解学习ret2text以及gdb的使用
6. 注册nssctf和buuctf或其他ctf学习平台,自己做些题
