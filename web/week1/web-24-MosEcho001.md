# 1.HackBar和BurpSuite的安装
![](https://github.com/user-attachments/assets/136b2549-1cc1-40cf-9357-d40c1d37711a)
![](https://github.com/user-attachments/assets/2167cce1-e03f-4d4a-9ce2-1ffb89b47ea1)

# 2.攻防世界

（1）view_source
![](https://github.com/user-attachments/assets/576823e6-a56d-4dd9-b0e7-63bd49dd05cc)

## (2)get_post

![](https://github.com/user-attachments/assets/d3cc3131-71d1-45e8-8399-5fbfbfc7fdc9)

## (3)robots


![](https://github.com/user-attachments/assets/cc4ca9a6-4e4e-4dec-9016-09ce8c1d66ff)


## (4)AI Python脚本

```py
import hashlib

def find_md5_with_prefix(prefix):
    """
    找到一个值，使得其 MD5 哈希值的前 6 位与给定的前缀匹配。
    :param prefix: 目标前缀（例如 "19ca14"）
    """
    i = 0  # 从 0 开始尝试
    while True:
        # 将整数转换为字符串
        value = str(i)
        # 计算 MD5 哈希值
        md5_hash = hashlib.md5(value.encode()).hexdigest()
        # 检查哈希值的前 6 位是否匹配目标前缀
        if md5_hash[:6] == prefix:
            print(f"找到符合条件的值: {value}")
            print(f"对应的 MD5 哈希值: {md5_hash}")
            break
        i += 1  # 继续尝试下一个值

if __name__ == "__main__":
    # 设置目标前缀
    target_prefix = "19ca14"
    print(f"正在寻找 MD5 哈希值前 6 位为 {target_prefix} 的值...")
    find_md5_with_prefix(target_prefix)
```

# 3.PHP基础学习

PHP 代码通常嵌入在 HTML 中，使用 <?php 和 ?> 标记包裹。
使用 echo 或 print 输出内容
变量以 $ 开头，后面跟变量名。
变量名区分大小写。

## (2)PHP应用(使用vscode实现)

![](https://github.com/user-attachments/assets/8626e751-aa24-44ae-a66e-1dfdb92bd5ae)
