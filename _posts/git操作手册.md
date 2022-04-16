---
title: git操作手册
tags:
categories:
  - 环境与工具
toc: true
mathjax: true
top: false
comments: true
copyright: true
date: 2022-04-13 10:17:22
---

# 1 命令



# 2 举例

对方更新过代码，然后自己也需要上传新增文件，操作：

```sh
# 拉下更新代码
git pull
git pull origin master:master
# 详细的。。。
git pull <远程主机名> <远程分支名>:<本地分支名>
# 新增文件
···
# 将所有新增文件添加到暂存区
git add .
# 将暂存区内容添加到本地仓库中
git commit -m "注释"
# 从将本地的分支版本上传到远程并合并
git push
git push origin master:master
# 详细的。。。
git push <远程主机名> <本地分支名>:<远程分支名>
git push <远程主机名> <本地分支名> # 如果本地分支名与远程分支名相同，则可以省略冒号
```

# X 参考

* [菜鸟教程——git](https://www.runoob.com/git/git-tutorial.html)
