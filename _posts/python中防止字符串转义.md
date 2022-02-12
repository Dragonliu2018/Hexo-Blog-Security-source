---
title: python中防止字符串转义
tags:
  - python
categories:
  - 开发
toc: true
mathjax: true
top: false
comments: true
copyright: true
date: 2022-02-09 15:31:55
---

# 1 问题引入

最近在做xml解析、可视化时遇到一个问题：将字符串（文件路径）中的转义字符不进行转义`123\tabc`，正常print会将`\t`看作转义字符tab，需求是使字符串按原样输出。下面是不同场景下的解决方法：

# 2 字符串常量

如果是字符串常量，则在字符串前加`r` 即可：

```python
print("123\tabc")
# 123	abc
print(r"123\tabc")  # test_str = r"123\tabc"
# 123\tabc
```

# 3 字符串变量

如果字符串是变量，使用 `repr()` 方法：

```python
str1 = "123\tabc"
str2 = repr(str1)

print(str1)  # 123	abc
print(str2)  # '123\tabc'
print(str2.strip("\'"))  # 123\tabc
```

<font color=red>**注意**</font>：字符串使用 `repr` 函数后输出是会在字符串前后自动加上单引号（见print第二行），print第三行是去掉了字符串首尾的单引号。关于`strip`函数，详见[博客](dragonliu.tk/2022/02/09/python去除字符串首尾的空格/)。

# 4 文件读取

```python
file1 = open("test.txt", "r")  # 123\tabc

print(file1.read())  # 123\tabc
```

文本文件读取测试发现字符串未发生转义。

拓展阅读：[关于python中open函数读取和写入windows记事本的换行问题研究](https://blog.csdn.net/ahalearner/article/details/119777972)

# X 参考

* [python中防止字符串转义](https://www.cnblogs.com/hellofengying/p/10183057.html)
* [python如何实现对变量的禁止转义操作](https://blog.csdn.net/weixin_41813169/article/details/105702294)
