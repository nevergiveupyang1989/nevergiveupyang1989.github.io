---
layout: post
category : 计算机技术
title: "从管理开机启动项看windows注册表"
tags : [开机启动, windows, 计算机技术]
---
{% include JB/setup %}

最近一直在折腾winy7系统，从管理开机启动项到管理鼠标右键菜单（后续的文章会讲到这方面的内容）等等。

    “开机启动项”包括了开机启动程序和开机启动服务。

## 设置开机启动

### 查看和禁用开机启动程序

win7查看和管理开机启动项的方式非常简单，打开系统开始菜单，搜索`msconfig`，打开它，就会看到下图：

__步骤一__

![image](https://f.cloud.github.com/assets/4392234/1482575/253abb4c-46e5-11e3-9261-d3b2ae0cf1c6.png)

__系统设置图__

![image](https://f.cloud.github.com/assets/4392234/1482572/fe333cea-46e4-11e3-9359-67875cd4aedd.png)

在系统设置的启动选项卡里面就可以禁用启动项目了。

### 如何添加开机启动程序

那怎么添加一个程序到开机启动呢？我们继续细看启动选项卡里面的位置栏：

![image](https://f.cloud.github.com/assets/4392234/1482583/731f5566-46e5-11e3-82bd-ead5d8c60037.png)

位置是什么？这一坨坨的是什么？

这里的位置是指该启动设置在__注册表__的位置。我们可以联想得到，有三种方式可以设置开机启动程序，一个是在注册表`HKLM`中，一个是在注册表`HKCU`中，另一个就是在C盘内的startup文件夹新建程序的快捷方式。

## 注册表

### 注册表是什么？

注册表（Registry）是微软公司从Windows95系统开始，引入用于代替原先Win32系统里.ini文件，管理配置系统运行参数的一个全新的核心数据库。它与老的win32系统里的ini文件相比，具有方便管理，安全性较高、适于网络操作等特点。

注册表整合集成了全部系统和应用程序的初始化信息。它存储下面这些内容：
（1）软、硬件的有关配置和状态信息，应用程序和资源管理器外壳的初始条件、首选项和卸载数据；
（2）计算机的整个系统的设置和各种许可，文件扩展名与应用程序的关联，硬件的描述、状态和属性；
（3）计算机性能纪录和底层的系统状态信息，以及各类其他数据。

### 编辑注册表的工具---Regedit.exe

Regedit可对注册表进行添加、修改主键、键值，备份注册表，局部导入导出注册表等操作。
启动方法：开始菜单→运行，所在对话框中输入regedit并点确定。

![image](https://f.cloud.github.com/assets/4392234/1482728/f10316cc-46e8-11e3-9176-2a617072fee9.png)

![image](https://f.cloud.github.com/assets/4392234/1482731/fc090914-46e8-11e3-83ae-23af0dce4c27.png)

![image](https://f.cloud.github.com/assets/4392234/1482743/5d9d9ad2-46e9-11e3-8f58-c8fb3d183f9c.png)

### 注册表的结构

在Windows中，注册表由两个文件组成：System.dat和User.dat，保存在windows所在的文件夹中。它们是由二进制数据组成。

System.dat包含系统硬件和软件的设置。
User.dat保存着与用户有关的信息，例如资源管理器的设置，颜色方案以及网络口令等等。

注册表编辑器与资源管理器的界面相似。左边窗格中，由“计算机”开始，以下是六个分支（WINNT只有前面5个），每个分之名都以HKEY开头，称为主键(KEY)，展开后可以看到主键还包含次级主键(SubKEY)。当单击某一主键或次主键时，右边窗格中显示的是所选主键包含的一个或多个键值(Value)。键值由键值名称(Value Name)和数据(Value Data)组成。主键中可以包含多级的次级主键，注册表中的信息就是按照多级的层次结构组织的。

### 注册表中各分支的功能
 
HKEY-CLASSES-ROOT
文件扩展名与应用的关联及OLE信息（Object Linking and mbedding--对象连接与嵌入）
HKEY-CURRENT-USER
当前登录用户控制面板选项和桌面等的设置,以及映射的网络驱动器
HKEY-LOCAL-MACHINE
计算机硬件与应用程序信息
HKEY-USERS
所有登录用户的信息
HKEY-CURRENT-CONFIG
计算机硬件配置信息
HKEY-DYN-DATA
即插即用和系统性能的动态信息

 ### 注册表中的键值项数据
 
注册表通过键和子键来管理各种信息。但是注册表中的所有信息都是以各种形式的键值项数据保存的。在注册表编辑器右窗格中显示的都是键值项数据。这些键值项数据可以分为三种类型：

1. 字符串值

    字符串值一般用来表示文件的描述和硬件的标识。通常由字母和数字组成，也可以是汉字，最大长度不能超过255个字符。

2. 二进制值

    二进制值是没有长度限制的，可以是任意字节长。在注册表编辑器中，二进制以十六进制的方式表示。

3. DWORD值

     DWORD值是一个32位(4个字节)的数值。在注册表编辑器中也是以十六进制的方式表示。

## 最初的问题

回到我们最开始的那个问题，添加启动程序就非常简单了，只需要在注册表`HKEY_CURRENT_USER＼SOFTWARE＼Microsoft＼Windows＼CurrentVersion＼Run`出新建一个字符串值即可

![image](https://f.cloud.github.com/assets/4392234/1483003/27bc3774-46ef-11e3-8563-808ed5c91138.png)

## 参考

[注册表详解](http://blog.renren.com/share/257058797/2975498634)
[注册表基础大全（基础篇）](http://www.360doc.com/content/06/1022/21/10506_237389.shtml)
[玩转注册表：从入门到精通](http://www.360doc.com/content/10/0129/12/285549_14656823.shtml)