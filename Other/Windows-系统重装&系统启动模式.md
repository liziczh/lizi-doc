---
title: Windows | 系统重装&系统启动模式
id: windows-reos
comments: true
date: 2018-04-10 12:09:34
tags:
- Windows
categories: 电脑维修
toc: true
reward: true
copyright: true
---

<!--# Windows | 系统重装-->

使用微软官方工具制作系统启动U盘，安装微软原版 Windows 系统，了解Windows系统的启动模式。

<!-- more -->

## Windows 系统重装

### 一、准备工作

#### Win7 下载

1.下载系统镜像：搜索[mdsn，我告诉你](https://msdn.itellyou.cn/)，复制系统的【ed2k链接】，粘贴至迅雷下载系统ISO文件。

2.制作启动U盘：下载微软官方工具 [Windows USB/DVD Download Tool](https://www.microsoft.com/en-us/download/details.aspx?id=56485)，格式化U盘，制作启动盘。

3.下载网卡驱动：登录【本机机型官网】，寻求【服务支持】，下载【网卡驱动程序】。

#### Win10 下载

1.下载微软官方工具 [MediaCreationTool.exe](https://www.microsoft.com/zh-cn/software-download/windows10?OCID=WIP_r_Win10_Body_AddPC) ，选择【为另一台电脑创建安装介质】，制作启动U盘。

### 二、重装系统

1.BIOS设置：开机进入【BIOS设置程序】，设置【USB】为【第一启动项】，保存退出。

  > ？：机型不同，进入BIOS方式不同，BIOS设置也不同，根据机型自行百度；

2.磁盘分区：进入系统安装界面，安装方式选择【自定义安装】，根据需求自行分区。

> ？：如果是win10/8换win7，需要将GPT分区转换为MBR分区；
> ？：如果是win7升win10/8，需要将MBR分区转换为GPT分区；
>
> ```shell
> # 在windows系统安装界面 Shift+F10 调出控制台。
> diskpart      # 进入DiskPart工具
> list disk     # 列出磁盘信息
> select disk n # 选择需要操作的磁盘，n为磁盘编号。
> clean         # 清理磁盘所有数据
> convert mbr   # 转换为MBR分区形式
> convert gpt   # 转换为GPT分区形式
> ```

3.根据需求设置系统时间日期地区等等信息，等待系统安装......

4.安装驱动：安装网卡驱动，连接网络，安装其他驱动。

> - Win7系统：安装已下载的网卡驱动。连接网络，可以前往【本机机型官网】下载驱动选择，也可以下载驱动软件【驱动大师】【驱动精灵】等一键安装。
> - Win10系统：自动安装驱动。

5.BIOS设置：开机进入【BIOS设置程序】，设置【HardDisk】为【第一启动项】，保存退出。

6.系统激活：在[淘宝](https://s.taobao.com)10块钱买个激活码，激活系统。



## 了解Windows系统启动模式

### 一、Windows 系统启动方式

- **Legacy BIOS 启动**：在开机时需进行自检，启动过程较复杂。并且BIOS无法识别GPT分区，所以在Legacy BIOS模式下，采用GPT方式分区的磁盘无法安装操作系统，只能用于数据存储。
- **UEFI 启动**：直接从预启动的操作环境加载操作系统，简化开机过程有效提高启动速度。并且可以同时识别MBR和GPT，在UEFI模式中MBR和GPT都支持安装操作系统，但微软规定，在UEFI模式只能使用GPT硬盘安装Windows系统。 

### 二、磁盘分区方式

- **MBR分区**：主引导记录磁盘分区格式。采用MBR分区，主分区数目不能超过4个，而且由于MBR分区方式用4个字节存储分区的总扇区数，按每扇区512字节计算，无法支持超过2TB容量的磁盘。
- **GPT分区**：GUID全局唯一标识磁盘分区表。GPT的先进之处在于GPT分区表头可自定义分区数量的最大值，即GPT分区表的大小不是固定的。GPT采用64位二进制数表示逻辑块地址，所以分区个数和分区大小几乎没有限制，但Windows系统最多只允许划分128个分区。

> Legacy BIOS+MBR支持安装所有的Windows系统。
> UEFI+GPT支持Win7-64位及Win7之后的操作系统。
