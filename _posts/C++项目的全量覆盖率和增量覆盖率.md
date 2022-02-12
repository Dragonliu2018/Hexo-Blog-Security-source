---
title: C++项目的全量覆盖率和增量覆盖率
date: 2021-09-27 09:44:22
tags: 
- C++
categories:
    - [环境与工具]
    - [开发]
# keywords:
description:
top: #True #顶置
top_img: #（除非特定需要，可以不写）
comments: # 是否显示评论（除非设置false,可以不写）
cover: https://cdn.jsdelivr.net/gh/Dragonliu2018/FigureBed@master/img/ddddd.jpg
toc: true #是否显示toc （除非特定文章设置，可以不写）
toc_number: #是否显示toc数字 （除非特定文章设置，可以不写）
copyright: #是否显示版权 （除非特定文章设置，可以不写）
---

# 0 情景导入

2021.09.26华为实习任务复现，要求实现多次SR合并后的增量覆盖率。

# 关于报错

```sh
###强制回滚到某版本
git reset --hard 36409abd22dcc9f9252152b34fa9e929f7c883ec
```

```sh
###解决：https://stackoverflow.com/questions/3411048/unexpected-operator-in-shell-programming/3411061
./srCover.sh: 17: [: h: unexpected operator 
./srCover.sh: 21: [: h: unexpected operator
```

# 1 思路

强行cherry-pick，可能存在冲突的情况：

1. 不冲突，是准确的
2. 冲突，使用指定版本的代码解决冲突，是不准确的

# 2 cherry-pick

# 3 生成增量覆盖率报告

## 3.1 生成差异文件

v1版本：dir1目录，

v2版本：dir2目录，

执行：

```sh
# diff -r  -N -x ".git" -x "*.gcov" -u <old_src_path> <new_src_path> > diff.txt
diff -r  -N -x ".git" -x "*.gc*" -x "result" -x "hello" -x "hello.info" -u dir1 dir2 > diff.txt
```

## 3.2 生成v2版本的全量覆盖率报告

> * [代码覆盖工具(gcov、lcov)的使用](https://www.cnblogs.com/fnlingnzb-learner/p/6943512.html)

> gcov是Linux下GCC自带的一个C/C++代码覆盖率分析工具,因此只要安装了gcc，就不需要再次安装了。
>
> gcov能够生成代码覆盖信息，但是不够直观，因此需要借助lcov直观展示覆盖率，主要特点有：
>
> * 基于Html输出，并生成一棵完整的HTML树
> * 输出包括概述、覆盖率百分比、图表，能快速浏览覆盖率数据
> * 支持大项目，提供三个级别的视图：目录视图、文件视图、源码视图

1. 安装[lcov](http://ltp.sourceforge.net/coverage/lcov.php)：

   ```sh
   git clone https://github.com/linux-test-project/lcov.git
   # 进入项目目录
   cd lcov
   # 编译
   make install
   ```

2. 以hello.c为例：

   ```c++
   #include<stdio.h>
   
   int main()
   {
           int num = 1;
           if (num == 0) {
                   printf("num is zero\n");
           } else if (num == 1) {
                   printf("num is one\n");
           } else {
                   printf("num is NAN\n");
           }
           printf("hello world\n");
           return 0;
   }
   ```

3. 测试：

   ```sh
   # 编译时候加两个参数：-fprofile-arcs -ftest-coverage, 此时会生成：hello.gcno
   gcc -fprofile-arcs -ftest-coverage hello.c -o hello
   # 此时会生成： hello.gcda, hello.gcno和hello.gcda是gcov引用的关键文件)
   ./hello
   # gcov根据上面的文件生成了：hello.c.gcov,就是代码覆盖信息，但看起来并不直观
   gcov hello.c
   ### 内容如下：
   ➜  dir2 cat hello.c.gcov 
           -:    0:Source:hello.c
           -:    0:Graph:hello.gcno
           -:    0:Data:hello.gcda
           -:    0:Runs:1
           -:    0:Programs:1
           -:    1:#include<stdio.h>
           -:    2:
           1:    3:int main()
           -:    4:{
           1:    5:        int num = 1;
           1:    6:        if (num == 0) {
       #####:    7:                printf("num is zero\n");
           1:    8:        } else if (num == 1) {
           1:    9:                printf("num is one\n");
           -:   10:        } else {
       #####:   11:                printf("num is NAN\n");
           -:   12:        }
           1:   13:        printf("hello world\n");
           1:   14:        return 0;
           -:   15:}
   
   # 借助lcov对hello.c.gcov进行改造，可以看见生成了hello.info
   lcov -d . -t 'Hello test' -o 'hello.info' -b . -c
   # 生成了result文件夹，借助web服务器，我们就可以很直观的看到结果了
   genhtml -o result hello.info
   ```

4. 查看生成的全量覆盖率报告，用浏览器打开result文件夹内的index.html：

   <img src="https://i.loli.net/2021/09/26/aAG4BMTl2mjrIUZ.png" width = "1000" height = "250" alt="图片名称" align=center id=31 />

## 3.3 使用addlcov生成增量代码覆盖率文件

`addlcov --diff <full_cov_file> diff.txt -o <add_cov_file> --strip <depth> --path <new_src_path>`

* <full_cov_file>为之前统计的v2版本上的全量覆盖率；
* <add_cov_file>为输出的增量覆盖率info文件；
* <depth>是要截取的代码深度；
* <new_src_path>是v2版本源码

执行：

```sh
addlcov --diff  diff.txt -o <add_cov_file> --strip <depth> --path <new_src_path>
```

报错

```sh
➜  Test1 ./addlcov --diff hello.info diff.txt -o addhello.info  --strip 5 --path ./dir2
Reading tracefile hello.info
Reading diff diff.txt
Removing /mnt/e/Code/cpp/Test1/dir1/hello.c
1 entry converted, 0 entries left unchanged.
Writing data to addhello.info
Summary coverage rate:
  lines......: no data found
  functions..: no data found
  branches...: no data found
  
### bug addlcov脚本无法匹配
# Try to match diff filename with filename
# if ($filename =~ /^\Q$diff_path$sep$_\E$/) 修改为下面语句，
if (1 == 1)
### 单文件出现上述问题，多文件无问题
```

python中执行shell命令：https://blog.51cto.com/zhou123/1284377
