---
layout: single
category: "Windows"
title:  "Procmon 案例#1"
tags: [Procmon]
toc: true
toc_label: "My Table of Contents"
toc_icon: "gear"
---

### 问题描述
用户反馈，当他的电脑访问访问网络共享文件夹`\\zch49app02\mailattach`，建立文件夹后改名，窗口会hang很长时间（点击窗口任何位置没有反应），而正常情况只需要1~2秒。

### 问题分析
这里涉及到用户电脑，网络通讯，文件共享服务器等。首先排除不相关的因素，通过另外一台主机访问该文件共享服务，建立文件修改文件没有任何问题，初步判断应该是用户电脑自身的问题

### 处理过程
1、使用Procmon记录一些操作，然后让用户修改文件共享服务器上的文件名（原来文件夹 ‘new folder’修改为 ‘new folder2’），等待漫长时间后修改成功（问题重现），保存LOG，做进一步的分析。

2、打开LOG文件，如前描述描述，我们是把'new folder'修改成'new folder2'，所以先过滤路径包含'new folder‘的内容（如下图片）
![](/images/procmon-case1-01.png)  
可以看到和关键字相关的有svchost.exe、Exlorer.EXE，svchost.exe一般是服进程，所以不考虑；而显示文件框一般和explorer相关，所以我们进一步过滤包含 explorer.exe的进程。

3、过滤 explorer.exe后，拖动Procmon日志框，因为窗口Hang，则explorer.exe一般会有时间跳跃地方；直到发现如下图片，时间中断（接近1分钟的中断）
![](/images/procmon-case1-02.png)  

4、explorer除了处理这次修改文件名的操作外，还会处理其它操作（基于Windows多进程原理），开启线程显示，然后搜索 new folder2发现如下结果
![](/images/procmon-case1-03.png)  
发现setrenameinfo操作，且TID 10924。可以初步判断 TID 10924是这次操作（修改文件名）的线程。

5、取消先前的所有过滤，设置只过滤TID 10924的Log，拖到刚才时间中断点
![](/images/procmon-case1-04.png)
6、查看19:50最后一个记录，查看其Stack调用，发现bit9的痕迹（Parity.sys）  
![](/images/procmon-case1-05.png)  
备注：公司在前期大面积部署了Bit9（安全软件），而Parity.sys是Bit9的一个驱动，此驱动会在Windows启动以服务方式加载系统，而无法用普通删除方式来解决（Windows删除会提示正在使用无法删除），需要通过Winpe，来删除硬盘Windows服务方式来解决。这里也可以点击Parity.sys，然后查看属性，会发现是Bit9进程而不是Windows系统自身的进程。删除Bit9后，恢复正常。

### 最后
通过分析，了解到是部署安全软件引起的问题。不过这个问题也很有迷惑性，因为是大面积部署Bit9，绝大部分用户都安装Bit9，但是只有极个别用户有碰见类似问题;所以只能考虑这个Bit9软件自身Bug，某种环境下会出现异常，只能是具体问题具体分析。另外Windows自身的程序，动态连接并不会引起使用异常，这就是直接跳过图片中flmgr.sys、ntoskrnl.exe进程检查的原因。

特别注意，修改系统文件肯能会引起系统崩溃，所以任何操作请三思。简易在试验机上操作确定没问题再到用户机上操作。
