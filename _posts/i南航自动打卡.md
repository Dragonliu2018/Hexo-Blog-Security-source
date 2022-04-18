---
title: i南航自动打卡
tags:
  - 待补充
categories:
  - 未分类
toc: true
mathjax: true
top: false
comments: true
copyright: true
date: 2022-04-13 12:17:36
---

# 1 引言

**需求**：每天的i南航打卡比较难受，想自动化处理此事。

**声明**：学习与技术讨论，勿实操！！！

**其他**：目前在使用`2.1 IOS系统`的方法实现自动打卡

# 2 解决

## 2.1  IOS系统

iPhone和iPad可以使用快捷指令，体验感较好。

But最近i南航签到界面增加了字段，目前（2022年4月13日）打卡虽然可以成功，但是健康码是黄色的。快捷指令的作者当前未更新，自己不会改。🥦

(2022年4月14日)打卡恢复正常。

* [博客教程：使用快捷指令自动进行i·南航健康打卡](https://blog.cvvv.me/posts/a67c.html)

* [B站教程：【南航】【i南航打卡】iOS/iPadOS快捷指令i南航自动打卡教程](https://www.bilibili.com/video/BV1CY41137Af)

## 2.2 Linux客户机或服务器

wood的项目：[i南航校外自动打卡小工具](https://github.com/Wood1314/inuaa)

## 2.3 Windows（待完成）

上面wood的项目是通过Linux服务器运行脚本，邮件通知提醒打卡成功。

对脚本稍作修改，迁移到Windows上来，实现定时运行脚本+Windows自带的消息通知。



