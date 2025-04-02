1.Hackbar和**<font style="color:rgb(240, 246, 252);background-color:rgb(13, 17, 23);">BurpSuite</font>**的安装

![](https://cdn.nlark.com/yuque/0/2025/png/54389986/1741858622497-a069a1db-9d17-4a22-8dd5-5ac0fb320843.png)

![](https://cdn.nlark.com/yuque/0/2025/png/54389986/1741858763452-0f2f76b2-fd24-4829-bae3-3e935b14b834.png)

2.攻防世界

（1）view_source

![](https://cdn.nlark.com/yuque/0/2025/png/54389986/1741858931174-844b1966-dfac-48a9-a8ef-89033550616d.png)

(2)get_post

![](https://cdn.nlark.com/yuque/0/2025/png/54389986/1741858973516-97874979-ee1a-43ff-89dc-e3af51ce11f6.png)

(3)robots

![](https://cdn.nlark.com/yuque/0/2025/png/54389986/1741859078604-9edbaac2-f7df-4156-89b8-21a6673d4f23.png)

（4）AI Python脚本

```plain
import hashlib

def find_md5_prefix(target_prefix):
    n = 0
    while True:
        candidate = str(n)
        # 计算MD5哈希
        md5_hash = hashlib.md5(candidate.encode('utf-8')).hexdigest()
        # 检查前缀是否匹配
        if md5_hash.startswith(target_prefix):
            print(f"找到符合条件的值: {candidate}")
            print(f"对应的MD5哈希值: {md5_hash}")
            return candidate
        n += 1
        # 可选：每百万次输出进度
        if n % 1000000 == 0:
            print(f"已检查到: {n}")

if __name__ == "__main__":
    target = '19ca14'
    find_md5_prefix(target)
```

3.PHP基础学习

(1)PHP基础语法

![](https://cdn.nlark.com/yuque/0/2025/jpeg/54389986/1741859994401-c26c8f98-79fc-4295-a5ab-52a76fe3b2f4.jpeg?x-oss-process=image/auto-orient,1)

(2)PHP应用（使用PHPstorm实现)

![](https://cdn.nlark.com/yuque/0/2025/png/54389986/1741860051050-c4b119d8-50a2-4304-9ec3-6af875a54a64.png)
（README里的flag{we1c0me_t0_CTF!}

