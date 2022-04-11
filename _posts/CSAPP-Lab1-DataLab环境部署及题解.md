---
title: CSAPP-Lab1-DataLab环境部署及题解
tags:
  - 计组
categories:
  - 基础
toc: true
mathjax: true
top: false
comments: true
copyright: true
date: 2022-04-09 01:26:20
---

# 1 介绍

该实验是《深入理解计算机系统》（英文缩写CSAPP）课程附带实验——Lab1：Data Lab。



# 2 环境部署



# 3 题解

## 3.1 lsbZero.c

**注释**：

```c++
/*
 *   lsbZero - set 0 to the least significant bit of x
 *   Example: lsbZero(0x87654321) = 0x87654320
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 5
 *   Rating: 1
 */
```

**题目**：

给定一个`int`型整数`x`，将其最低一位有效位置0。

* 例如：`lsbZero(0x87654321) = 0x87654320`

**实现**：

```c++
// 法1（标准答案）
/* */
int lsbZero(int x) {
  return (x | 1) + ~0;
}

// 法2（目前最佳答案）
/* 右移一位再左移一位实现把最低一位有效位置0 */
int lsbZero(int x) {
  return x >> 1 << 1;
}
```

## 3.2 byteNot.c

**注释**：

```c++
/*
 *   byteNot.c - bit-inversion to byte n from word x
 *   Bytes numbered from 0 (LSB) to 3 (MSB)
 *   Examples: getByteNot(0x12345678,1) = 0x1234A978
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 6
 *   Rating: 2
 */
```

**题目**：

给定一个`int`型整数`x`，将其第`n`个字节实现取反。

* 例如：`byteNot(0x12345678,1) = 0x1234A978`

**实现**：

```c++
// 法1（标准答案）
/* 将0xFF移动到对应字节，然后取按位异或 */
int nuaa_question2(int x, int n) {
  /* Shift x n*8 positions right */
  int shift = n << 3;
  /* Mask byte */
  int mask = 0xFF << shift;
  return x ^ mask;
}
```

## 3.3 byteXor.c

**注释**：

```c++
/*
 *   byteXor - compare the nth byte of x and y, if it is same, return 0, if not, return 1
 *   example: byteXor(0x12345678, 0x87654321, 1) = 1
 *            byteXor(0x12345678, 0x87344321, 2) = 0
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 20
 *   Rating: 2
 */
```

**题目**：

给定三个`int`整数 `x` 、 `y `和 `n` ，比较 `x` 和 `y` 的第 `n` 字节，如果相同则返回 `0` ，不同则返回 `1`。

* 例如：`byteXor(0x12345678, 0x87654321, 1) = 1`
* 例如：`byteXor(0x12345678, 0x87344321, 2) = 0`

**实现**：

```c++
// 法1（标准答案）
/* 移位取指定字节，异或，两次取逻辑非 */
int byteXor(int x, int y, int n) {
  int shift = n << 3;
  int xs = x >> shift;
  int ys = y >> shift;
  int cmp = (xs & 0xFF) ^ (ys & 0xFF);
  return !!cmp & 1 ;
}

// 法2（目前最佳答案）
/* 法1最后的&1是多余的操作 */
int byteXor(int x, int y, int n) {
  int shift = n << 3;
  int xs = x >> shift;
  int ys = y >> shift;
  int cmp = (xs & 0xFF) ^ (ys & 0xFF);
  return !!cmp;
}
```

## 3.4 logicalAnd

**注释**：

```c++
/*
 *   logicalAnd - x && y
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 20
 *   Rating: 3
 */
```

**题目**：

实现逻辑与`&&`，当两个操作数都是ture的时候，才返回true，否则返回false。

* 例如：`logicalAnd(1, 0) = 0`
* 例如：`logicalAnd(2, 3) = 1`

**实现**：

```c++
// 法1（标准答案）
/* */
int logicalAnd(int x, int y) {
  int z = ~!x & ~!y;
  return ~(z + 1) & 1;
}

// 法2（目前最佳答案）
/* 把x和y分别取NOT，二者相或后再取NOT，即可得到逻辑与 */
int logicalAnd(int x, int y) {
   return !((!x)|(!y));
}

// 法3（目前最佳答案）
/* 把x和y分别转换为逻辑的0和1，再相与 */
int logicalAnd(int x, int y) {
   return (!(!x))&(!(!y));
}
```

## 3.5 logicalOr

**注释**：

```c++
/*
 *   logicalOr - x || y
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 20
 *   Rating: 3
 */
```

**题目**：

实现逻辑或`||`，当两个操作数有一个为ture的时候，就返回true。

* 例如：`logicalOr(0, 0) = 0`
* 例如：`logicalOr(2, 0) = 1`

**实现**：

```c++
// 法1（标准答案）
/* */
int logicalOr(int x, int y) {
  return ~(!x & !y) & 1;
}

// 法2（目前最佳答案）
/* 把x和y分别转换为逻辑的0和1，再相或 */
int logicalOr(int x, int y) {
  return (!(!x))|(!(!y));
}
```

## 3.6 rotateLeft

**注释**：

```c++
/*
 *   rotateLeft - Rotate x to the left by n
 *   Can assume that 0 <= n <= 31
 *   Examples: rotateLeft(0x87654321,4) = 0x76543218
 *   Legal ops: ~ & ^ | + << >> !
 *   Max ops: 25
 *   Rating: 3
 */
```

**题目**：

给定一个`int`型整数`x`，实现循环左移，也就是把`x`的前`n`位移到末尾。

* 例如：`rotateLeft(0x87654321,4) = 0x76543218`

**实现**：

```c++
// 法1（标准答案）
/* */
int rotateLeft(int x, int n) {
    /* Create mask for n = 0 */
    int zmask = (~!n)+1;
    int left = x << n;
    /* Arithmetic shift right by 32-n */
    int right = x >> (33 + ~n);
    /* Mask off upper 1's */
    int lmask = ~0 << n;
    right &= ~lmask;
    return (zmask&x) | (~zmask&(left|right));
}

// 法2
/* 先构造y为高（32-n）位为0的y，再与x右移（32-n）的x相与，相当于储存了x的高n位数，最后再与x左移n位相加即可。 */
int rotateLeft(int x, int n) {
    int y;
    y=~((~0)<<n);
    x=(x<<n)+((x>>(32+(~n+1)))&y);
    return x;
}
```

## 3.7 float_abs.c

**注释**：

```c++
/*
 *   float_abs - Return bit-level equivalent of absolute value of f for
 *   floating point argument f.
 *   Both the argument and result are passed as unsigned int's, but
 *   they are to be interpreted as the bit-level representations of
 *   single-precision floating point values.
 *   When argument is NaN, return argument..
 *   Legal ops: Any integer/unsigned operations incl. ||, &&. also if, while
 *   Max ops: 10
 *   Rating: 2
 */

```

**题目**：

给予⼀个无符号整数表示的浮点数 `uf` (你可以认为 `uf` 具有浮点数的比特级结构) ，函数返回它的绝对值，即 `|uf|` (返回的结果也是⼀个无符号整数表示的浮点数)，如果这个数是 `NaN` ，请返回它本身。

**实现**：

```c++
// 法1（标准答案）
/* */
unsigned float_abs(unsigned uf) {
  unsigned mask = 1 << 31;
  unsigned abs = uf & ~mask;
  if (abs > 0x7F800000)
    return uf;
  return abs;
}

// 法2（目前最佳答案）
/* https://blog.csdn.net/qq_43544682/article/details/102874816 */
unsigned float_abs(unsigned uf) {
    int x=uf&0x7fffffff;
    if(x>0x7f800000)
        return uf;//NaN
    else
        return x;
}
```

## 3.8 float_f2i

**注释**：

```c++
/*
 *   float_f2i - Return bit-level equivalent of expression (int) f
 *   for floating point argument f.
 *   Argument is passed as unsigned int, but
 *   it is to be interpreted as the bit-level representation of a
 *   single-precision floating point value.
 *   Anything out of range (including NaN and infinity) should return
 *   0x80000000u.
 *   Legal ops: Any integer/unsigned operations incl. ||, &&. also if, while
 *   Max ops: 30
 *   Rating: 4
 */

```

**题目**：

给予⼀个无符号整数表示的浮点数 `uf` (你可以认为 `uf` 具有浮点数的比特级结构) ，函数返回它对应的强制类型转换后的整数（具有整数的比特级结构），即实现表达式 `(int)uf` ，如果这个数超过整数表示的范围（包括 `NaN` 和无穷），请返回 `0x80000000u`。

**实现**：

```c++
// 法1（标准答案）
/* */
int float_f2i(unsigned uf) {
  unsigned sign = uf >> 31;
  unsigned exp = (uf >> 23) & 0xFF;
  unsigned frac = uf & 0x7FFFFF;
  /* Create normalized value with leading one inserted,
     and rest of significand in bits 8--30.
  */
  unsigned val = 0x80000000u + (frac << 8);
  if (exp < 127) {
    /* Absolute value is < 1 */
    return 0;
  }
  if (exp > 158)
    /* Overflow */
    return 0x80000000u;
  /* Shift val right */
  val = val >> (158 - exp);
  /* Check if out of range */
  if (sign) {
    /* Negative */
    return val > 0x80000000u ? 0x80000000u : -val;
  } else {
    /* Positive */
    return val > 0x7FFFFFFF ? 0x80000000u : val;
  }
}

// 法2（目前最佳答案）
/*  */

```





# X 参考

* [计算机系统基础 实验一 csapp date lab1](https://blog.csdn.net/qq_43544682/article/details/102874816)
* [CSAPP:Lab1 -DataLab 超详解](https://zhuanlan.zhihu.com/p/339047608)
* [CSAPP: Lab1:Data Lab 实验 （虚拟机安装+Lab环境配置+README及实验引导翻译+如何编译+函数实现）](https://blog.csdn.net/qq_45677541/article/details/123955438)
