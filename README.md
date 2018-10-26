# 全栈最后一公里 —— Linux从入门到实践

![full_stack-c](http://pbe07x0ww.bkt.clouddn.com/full_stack.jpg)

> 傻瓜都能写出计算机能理解的程序，而好的程序员能写出人能读懂的代码。

[TOC]

## 一、前世今生

![linux_icon-c](http://pbe07x0ww.bkt.clouddn.com/linux_icon.png)

### 1、系统来源

在Linux之前有一个叫 *Andrew S. Tanenbaum* 的教授为了给他的学生上课买了一个UNIX操作系统源码，然后参考这个系统但是没有任何抄袭成分的做了一个叫作Minix的操作系统，后来他把这个Minix操作系统的源码全部开放给学校和学生做学习研究使用，再后来经过一番优化之后他把这个系统就直接放到网络上去开源了，当时就一下受到很多人的关注，开源免费的操作系统非常受欢迎，很多人就开始使用，但是用着用着就有人发现这个系统有些bug，于是就向这个教授提了很多修复补丁程序希望教授可以更新修复，但是呢这个教授不这么想，他说：“我写这个代码就只是为了教学，你们能看得懂我的目的就达到了，我不需要任何外来的代码也没有想过把它商业发行。”

那么这个时候另一个家伙出现了，*Linus Torvalds*，当时是芬兰赫尔辛基大学大三的大学生，他以Minix为模板，自己开发了一些软件和并且集中了当时网上呼声较高的一些补丁程序重新写了一个操作系统——Linux操作系统。

![Linus Torvalds-c](http://pbe07x0ww.bkt.clouddn.com/Linus Torvalds.png)

Linus选择了芬兰的吉祥物企鹅作为该系统的标识，并在1991年的时候就将Linux操作系统发布了，短短20多年的时间，Linux系统就已经在服务器领域占据了举足轻重的地位。

### 2、系统版本

Linux经过20多年的发展已经拥有了非常众多的版本，但是我们需要知道的是这些版本中分为Linux内核版本和Linux发行版本，这两个版本不太一样。

#### Linux内核版本：

Linux内核版本就是核心版本，它由 [Linux内核官网](https://www.kernel.org/) 发行，目前最新的内核版本是4.19版本。

![Linux_core-c](http://pbe07x0ww.bkt.clouddn.com/Linux_core.png)

> 值得一提的是：对于服务器领域而言，一般不会采用最新版本，线上我们要本着“猥琐发育，不要浪”的原则！

#### Linux发行版本：

内核版本是由官网发布，任何人都可以去免费下载使用的，但是比如基于内核版本之上开发一些工具，开发一个桌面，放点好看的图标，或者做一些裁剪等，那么它就会变成各个厂商的发行版本。

![Linux_versions-c](http://pbe07x0ww.bkt.clouddn.com/Linux_versions.png)

在上面众多版本中，比较流行的主要是Redhat和Ubuntu，其中Ubuntu的优势主要在于界面优化的非常漂亮，有的时候都认不出来这是一个Linux系统，但是这些花里胡哨的东西并不适合服务器使用，上面我们说过服务器一定要稳定优先，所以一般不会在服务器上去安装图像界面，所以下面本教程也会以Redhat系列来进行，只不过有一点需要说的是Redhat部分功能是收费的，所以我们会以Redhat的替身CentOS操作系统为例进行讲解。

## 二、Linux系统安装

在开始学习Linux系统之前我们迫切需要知道如何把Linux系统给安装上，但是呢作为一个初学者，如果直接在自己的电脑上就装一个Linux操作系统，那我估计你可能痛苦的坚持不了3天就要重装系统了，所以就引入了下面的虚拟机相关知识。

### 1、虚拟机安装

在Mac上（PS：下述所有内容如无特殊说明都是基于Mac进行）常用的虚拟机就两种 [VMware](https://www.vmware.com/cn.html) 和 [Parallels Desktop](https://www.parallels.com/cn/) ，相同点是这两个都收费，而且价格不菲，不同点是我这里有可以使用的“~~正版VMware~~”，下载链接:https://pan.baidu.com/s/1dtnZ1V_evPFbJCrER2l9mg  密码:6o3p 关于如何在Mac上把VMware安装上就不作过多赘述了，与安装其他软件没有什么区别，一路Next然后完成之后使用天朝独特的Keygen去“~~优化~~（破解）”一下就可以使用了。

![VMware_wel-c](http://pbe07x0ww.bkt.clouddn.com/VMware_wel.png)

VMware安装完成之后打开应该是上面这样的，这里我们主要介绍三个功能，首先是左边一半重点便捷的**“从光盘或映像安装”**，这个功能没什么好说，给一个iso镜像文件然后就一路向西的安装完成了，其次是**“导入现有虚拟机”**，如果你已经有安装好的虚拟机文件(ps:使用虚拟机安装系统的时候会生成一个文件保存安装的系统数据信息)那么可以直接通过这个功能加载出来，最后是**“创建自定义虚拟机”**，这个功能就相当于你去电脑市场买了一个只有boot系统的电脑，里面并没有操作系统，需要你手动安装。我们下面安装Linux系统的时候也使用该功能。

### 2、系统分区

在进行Linux系统安装之前我们先要搞清楚一个问题，那就是系统分区。下面简单介绍一下系统分区的类型：

* 主分区：由硬盘硬件决定了主分区最多只能有4个
* 扩展分区：扩展分区最多只能有一个，并且主分区加扩展分区最多只能有4个，另外扩展分区不能写入数据，只能包含逻辑分区
* 逻辑分区

我们都知道在Windows中硬盘分区的时候需要指定盘符，例如：系统分区用字母C作为盘符，其他的用D、E、F以此类推，但是在Linux中我们把这个盘符称为**“挂载点”**，把一个硬盘分区让Linux系统读取到的过程称作**“挂载”**。关于Linux系统分区的要求如下：

* 必须分区：
 1. / (根分区) 
 2. swap分区 (交换分区，一般物理机内存4GB以下时建议为内存的2倍，物理机内存超过4GB时建议和物理机内存大小一致即可)
* 推荐分区：
  1. /boot (启动分区，200MB)

那么是不是说一块硬盘分区之后就使用了呢？不是的！做好分区之后还需要做另外一件事情，格式化。

格式化：又称逻辑格式化，它是指根据用户选定的文件系统（如FAT32、NTFS、EXT4等），在磁盘的特定区域写入特定的数据，从而在分区中划出一块用于存放文件分配表、目录表等用于文件管理的磁盘空间的过程；

> 注意：这里有一个误区，很多人认为格式化是用来清空分区里的数据的，其实并不是这样，虽然格式化会清空分区里的数据，但是格式化的根本目的是为了写入文件系统。

### 3、Linux系统安装

安装好虚拟机并且了解了系统分区知识后我们才可以进行科学的Linux系统安装。

#### 3.1 创建虚拟机

在前面我们已经安装好了VMware，启动它，选择**“创建自定义虚拟机”**，点击**“继续”**，之后看到如下画面：

![VMware_chos-c](http://pbe07x0ww.bkt.clouddn.com/VMware_chos.png)

然后选择**“Linux”**->**“CentOS 64位”**点击**“继续”**，画面如下：

![VMware_chhd-c](http://pbe07x0ww.bkt.clouddn.com/VMware_chhd.png)

选择**“新建虚拟磁盘”**，点击**“继续”**，就可以完成虚拟机的创建了，出现如下画面，点击**“完成”**，选择好虚拟机文件存放路径，即可完成虚拟机的创建。

![VMware_finish-c](http://pbe07x0ww.bkt.clouddn.com/VMware_finish.png)

#### 3.2 安装Linux

和Windows或者macOS操作系统一样，安装都需要一个系统镜像来完成安装，我们可以从[CentOS官网](https://www.centos.org/)下载各种版本的系统镜像，本教程使用的系统镜像是[CentOS 6.9 64位](http://vault.centos.org/6.9/isos/x86_64/)

下载好系统镜像之后，打开刚安装好的虚拟机，点击**“设置”**按钮，打开设置面板：

![VMware_st-c](http://pbe07x0ww.bkt.clouddn.com/VMware_st.png)

选择**“CD/DVD(IDE)”**：

![VMware_cd-c](http://pbe07x0ww.bkt.clouddn.com/VMware_cd.png)

勾选“连接 CD/DVD 驱动器”，然后下拉选择“选择一个光盘或光盘映像...”，选择我们下载好的系统镜像：

![VMware_sel_iso-c](http://pbe07x0ww.bkt.clouddn.com/VMware_sel_iso.png)

![VMware_iso-c](http://pbe07x0ww.bkt.clouddn.com/VMware_iso.png)

这就跟我们平时安装电脑在光驱里插入系统光盘一样，下面我们就可以启动虚拟机进行Linux系统安装了。

启动虚拟机，虚拟机会自动读取光驱里的系统镜像，从而启动Linux系统的安装程序，如下图：

![CentOS_install_start-c](http://pbe07x0ww.bkt.clouddn.com/CentOS_install_start.png)

默认选择第一项即可，直接回车，会出现这样一个镜像数据完整性检测界面，简单的说就是用来检测你下载的这个系统镜像是不是可以使用，这里我们一般都认为是可用的不再进行检测，所以直接选择**“Skip”**：

![CentOS_install_check-c](http://pbe07x0ww.bkt.clouddn.com/CentOS_install_check.png)

之后就很简单了，一路**“Next”**就行：

![CentOS_installz_wel-c](http://pbe07x0ww.bkt.clouddn.com/CentOS_installz_wel.png)

![CentOS_install_lan-c](http://pbe07x0ww.bkt.clouddn.com/CentOS_install_lan.png)

![CentOS_install_kb-c](http://pbe07x0ww.bkt.clouddn.com/CentOS_install_kb.png)

![CentOS_install_bs-c](http://pbe07x0ww.bkt.clouddn.com/CentOS_install_bs.png)

下面这里需要注意一下，如果曾经虚拟机安装过可能会有这个提示，直接忽略掉就可以了，选择**“是，忽略所有数据”**，然后点击**“下一步”**：

![CentOS_install_dc-c](http://pbe07x0ww.bkt.clouddn.com/CentOS_install_dc.png)

下面这里的主机名默认就好，因为对于Linux来说同一个局域网里并不是靠主机名来进行通信的，所以同一个局域网里可以有多个主机名相同的Linux计算机，这一点和Windows不一样。继续**“下一步”**：

![CentOS_install_host-c](http://pbe07x0ww.bkt.clouddn.com/CentOS_install_host.png)

![CentOS_install_time-c](http://pbe07x0ww.bkt.clouddn.com/CentOS_install_time.png)

![CentOS_install_pwd-c](http://pbe07x0ww.bkt.clouddn.com/CentOS_install_pwd.png)

在上面设置好密码之后就到下面选择安装类型了，其实就是选择分区方式，这里前面我们讲了关于Linux系统分区的相关知识，所以我们选择**“创建自定义布局”**，然后点击**“下一步”**：
![CentOS_install_layout-c](http://pbe07x0ww.bkt.clouddn.com/CentOS_install_layout.png)

下一步之后我们看到了如下的图形化分区界面，选择**“空闲”**的硬盘驱动器，点击**“创建”**来创建我们想要的分区：
![CentOS_install_layout_start-c](http://pbe07x0ww.bkt.clouddn.com/CentOS_install_layout_start.png)

选择**“标准分区”**，点击**“创建”**：

![CentOS_install_layout_c-c](http://pbe07x0ww.bkt.clouddn.com/CentOS_install_layout_c.png)

选择好**“挂载点”**和**“大小”**，点击**“确定”**：

![CentOS_install_layout_hm](http://pbe07x0ww.bkt.clouddn.com/CentOS_install_layout_hm.png)

重复上面的创建分区操作，根据之前说的系统分区要求做好分区后点击**“下一步”**，完整的分区创建完成后如下图：

![CentOS_install_full_lay-c](http://pbe07x0ww.bkt.clouddn.com/CentOS_install_full_lay.png)

选择**“格式化”**，点击**“下一步”**：

![CentOS_install_fm-c](http://pbe07x0ww.bkt.clouddn.com/CentOS_install_fm.png)

选择**“将修改写入磁盘”**，点击**“下一步”**：

![CentOS_install_wr-c](http://pbe07x0ww.bkt.clouddn.com/CentOS_install_wr.png)

![CentOS_install_in-c](http://pbe07x0ww.bkt.clouddn.com/CentOS_install_in.png)

下面这里选择安装方式需要注意一下，这几种安装方式的说明如下：

* Desktop：基本的桌面系统，包括常用的桌面软件，如文档查看工具、浏览器等
* Minimal Desktop：基本的桌面系统，包含的软件较少
* Minimal：基本的系统，不含有任何可选的软件包
* Basic Server：安装的基本系统的平台支持，不包含桌面
* Database Server：基本系统平台，加上MySQL和PostgreSQL数据库，无桌面
* Web Server：基本系统平台，加上PHP，Web server，还有MySQL和PostgreSQL数据库的客户端，无桌面
* Virtual Host：基本系统加虚拟平台
* Software Development Workstation：包含软件包较多，基本系统，虚拟化平台，桌面环境，开发工具

> 这里需要说明的是服务器上安装的时候一般都是选择Minimal只进行最小安装，系统安装完成后需要什么软件装什么软件；
> 但是由于我们初学者对Linux软件安装并不熟悉，所以我们为了后面的讲解方便，我们这里就选择Basic Server，这样安装完成就包含了一些基本的软件包，使用起来更方便；

选择**“Basic Server”**，点击**“下一步”**：
![CentOS_install_basic-c](http://pbe07x0ww.bkt.clouddn.com/CentOS_install_basic.png)

下面就启动了系统安装程序，静静的等待就好...

![CentOS_install_ing0-c](http://pbe07x0ww.bkt.clouddn.com/CentOS_install_ing0.png)

![CentOS_install_ing1-c](http://pbe07x0ww.bkt.clouddn.com/CentOS_install_ing1.png)

![CentOS_install_ing2-c](http://pbe07x0ww.bkt.clouddn.com/CentOS_install_ing2.png)

经过了上面漫长的等待，出现下面的画面，我们就完成了Linux系统的安装，点击**“重新引导”**：

![CentOS_install_cmp-c](http://pbe07x0ww.bkt.clouddn.com/CentOS_install_cmp.png)

重新引导完成后，CentOS系统启动，启动成功后界面如下，只有一个命令行字符界面，其他什么都没有:

![CentOS_start-c](http://pbe07x0ww.bkt.clouddn.com/CentOS_start.png)

我们输入用户名：root和密码即可登录，登录成功后如下图：

![CentOS_login-c](http://pbe07x0ww.bkt.clouddn.com/CentOS_login.png)

安装完成之后在`/root`目录下有三个安装日志文件，说明如下：

> `/root/install.log`：存储了安装在系统中的软件包及其版本信息
> `/root/install.log.syslog`：存储了安装过程中留下的事件记录
> `/root/anaconda-ks.cfg`：以Kickstart配置文件的格式记录安装过程中设置的选项信息（用作网络批量安装）

OK，到此为止，我们学习Linux的旅程已经踏出了第一步，Linux系统安装完成。

## 三、Linux 序言

在经历了一顿猛如虎的操作之后，我们把操作系统安装好了并且启动了，那么接下来我们即将正式进入Linux基础学习了。

对于Linux学习，其实就一句话 —— **_“一切皆文件！”_**。

OK，我的教程到此~~结束~~（开始）了。。。

好吧，言归正传，在学习Linux之前我们了解了她的前世今生，可是到目前为止我们还不知道她的庐山真面目呢，下面我们就来介绍一下Linux的文件结构：

![CentOS_files-c](http://pbe07x0ww.bkt.clouddn.com/CentOS_files.png)


![linux-file-system-hierarchy-c](http://pbe07x0ww.bkt.clouddn.com/linux-file-system-hierarchy.png)


| 目录 | 描述 |
| :-- | :-- |
| / | 主层次 的根，也是整个文件系统层次结构的根目录 |
| /bin | 存放在单用户模式可用的必要命令二进制文件，所有用户都可用，如 cat、ls、cp等等 |
| /boot | 存放引导加载程序文件，例如kernels、initrd等 |
| /dev | 存放必要的硬件设备文件，例如/dev/sda1表示第一块硬盘第一个分区 |
| /etc | 存放系统管理和应用管理需要的所有配置文件 |
| /home | 用户的主目录，包括保存的文件，个人配置，等等 |
| /media | 可移动的多媒体(如CD-ROMs)的挂载点，例如：光盘 |
| /mnt | 临时挂载的文件系统，例如：优盘，移动硬盘等 |
| /opt | 存放可选的应用程序软件包，例如：数据库等软件安装包 |
| /root | root用户的主目录 |
| /sbin | 存在超级管理员root用户可用的必要命令二进制文件，仅限root权限执行，例如：reboot、poweroff等等 |
| /tmp | 存放临时文件,通常在系统重启后删除 |
| /var | 各式各样的文件，一些随着系统常规操作而持续改变的文件就放在这里，比如日志文件，脱机文件，还有临时的电子邮件文件 |
| /usr | 这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似于Windows下的Program Files目录 |

## 四、Linux 网络管理

在这个网络高速发展的时代，拿到电脑的第一件事儿我们要干嘛呢？？对，没错，插上网线（或连接无线网）进行上网，那么问题来了，插上网线就能上网了吗？NoNoNo，这中间还隔着好几个太平洋呢，不过好在我们有一个用来跨越这些太平洋很重要的硬件，叫做**“网卡”**。下面我们就来讲一下如何让Linux上网。

### 1、网络模型与作用

要想上网首先我们就要了解网络模型，目前公认的网络模型有两种，一种是经典OSI七层网络模型，另外一种是TCP/IP四层网络模型。

#### 1.1 OSI七层网络模型
下图详细描述了OSI七层网络模型架构：

![ISO:OSI七层网络模型-c](http://pbe07x0ww.bkt.clouddn.com/ISO:OSI七层网络模型.png)

#### 1.2 TCP/IP四层网络模型

我们说上面的七层网络模型很经典，但是在实际应用中很多时候网络架构并不是这样的，而是采用TCP/IP四层网络模型，那么它们之间到底有怎样的联系呢？我们来看下图：

![IMG_A479D37BC665-1-c](http://pbe07x0ww.bkt.clouddn.com/IMG_A479D37BC665-1.jpeg)

#### 1.3 关于IP地址

从上面的网络模型中我们能够知道，IP其实是工作在网络层的互联网协议，它的作用这里不作过多阐述，主要想给大家说的是IP的分类，IP地址分为A~E五大类，但是其中D类IP、E类IP并不开放民用，所以下面是A、B、C三类IP的划分情况：

| 类别 | 最大网络数 | IP地址范围 | 最大主机数 | 私有IP地址范围 |
| --- | --- | --- | --- | --- |
| A | 126 (2^7 - 2) | 1.0.0.0~126.255.255.255 | 2^24 - 2 | 10.0.0.0~255.255.255.255 |
| B | 16384 (2^14) | 128.0.0.0~191.255.255.255 | 2^16 - 2 | 172.16.0.0~172.31.255.255 |
| C | 2097152 (2^21) | 192.0.0.0~223.255.255.255 | 2^8 - 2 | 192.168.0.0~192.168.255.255 |

#### 1.4 关于子网掩码

子网掩码是一种用来指明一个IP地址的哪些位标识的是主机所在的子网，以及哪些位标识的是主机的位掩码。好吧，说人话就是子网掩码是用来确定两个IP是否属于同一个网段的。子网掩码不能单独存在，它必须结合IP地址一起使用。既然这样我们就结合上面三类IP地址来看一下子网掩码，一般情况下，不同类别的IP地址与子网掩码具有如下对应关系：

![IMG_D5346D0871D2-1-c](http://pbe07x0ww.bkt.clouddn.com/IMG_D5346D0871D2-1.jpeg)

> A类IP地址与子网掩码的对应关系

![IMG_14FE850B0E9A-1-c](http://pbe07x0ww.bkt.clouddn.com/IMG_14FE850B0E9A-1.jpeg)

> B类IP地址与子网掩码的对应关系

![IMG_9F70E8FDFEF3-1-c](http://pbe07x0ww.bkt.clouddn.com/IMG_9F70E8FDFEF3-1.jpeg)

> C类IP地址与子网掩码的对应关系

#### 1.5 关于端口

端口是计算机对外服务的大门，每开启一个服务都必然有一个对应的端口号来让外部访问，常见端口号如下：


| 端口号 | 对应服务 |
| --- | --- |
| 20、21 | FTP 文件传输服务 |
| 22 | SSH 安全shell协议 |
| 23 | Telnet 远程登录协议（现在默认关闭） |
| 53 | DNS 域名解析服务 |
| 80 | HTTP 超文本传输协议 |
| 25、110 | SMTP 邮件传输协议、POP3 邮局协议3代 |

#### 1.6 关于DNS

DNS，域名解析服务，主要负责把域名解析成IP地址，细节不作过多描述，这里需要说明的是DNS的查询过程，详见下图：

![IMG_1867-c](http://pbe07x0ww.bkt.clouddn.com/IMG_1867.PNG)

#### 1.7 关于网关（路由器）

网关是访问互联网的必须配置项之一，主要作用：

* 网关在所有内网计算机访问的不是本网段的数据报时用于数据转发；
* 网关负责进行内网IP与公网IP之间的相互转换；

一般情况下，网关是路由器，也可以是服务器做路由功能。

> Tips：如果仅仅是内网通信其实是可以不配置DNS和网关的，但是如果需要访问互联网就必须配置网关。

### 2、 Linux网卡与配置

Linux配置IP地址的方法有四种：

* ifconfig命令临时配置IP地址
* setup工具永久配置IP地址
* **_修改网络配置文件_**
* 图形界面配置IP地址

我们这里只讲通过网络配置文件的配置方式，原因很简单，因为Linux一切皆文件！

#### 2.1 网卡信息文件

网卡信息文件在Linux中的路径为：`/etc/sysconfig/network-scripts/ifcfg-eth0`，关于该配置文件的基本说明如下：


| 字段名=对应值 | 描述 |
| --- | --- |
| **_DEVICE=eth0_** | 网卡设备名 |
| **_HWADDR=00:0C:29:71:27:EE_** | 网卡设备MAC地址 |
| **_TYPE=Ethernet_** | 网卡类型为以太网 |
| **_UUID=6c3e1dff-3e00-4933-81aa-317095967170_** | 唯一识别码 |
| **_ONBOOT=yes_** | 是否随网络服务启动，eth0生效 |
| **_NM_CONTROLLED=no_** | 是否可以由Network Manager图形管理工具托管 |
| **_BOOTPROTO=dhcp_** | 是否自动获取IP（none、static、dhcp），如果该项配置为dhcp，则该文件就只需要配置该表中带有下划线加粗的字段，但前提是要确保网络中有dhcp服务器 |
| **_USERCTL=no_** | 不允许非root用户控制此网卡 |
| IPADDR=192.168.1.110 | IP地址 |
| NETMASK=255.255.255.0 | 子网掩码 |
| GATEWAY=192.168.1.1 | 网关 |
| DNS1=192.168.0.1 | DNS（可以配置多个，DNS2、DNS3...） |
| IPV6INIT=no | 不启用IPv6 |

#### 2.2 主机名文件

主机名文件路径为：`/etc/sysconfig/network`，关于该配置文件的基本说明如下：


| 字段名=对应值 | 描述 |
| --- | --- |
| NETWORKING=yes | 网络服务是否工作 |
| HOSTNAME=localhost.localdomain | 主机名 |

> 该配置必须重启后才能生效
> 可以通过hostname命令临时修改

#### 2.3 DNS配置文件

DNS配置文件路径为：`/etc/resolve.conf`，该配置文件说明如下：

| 字段名 对应值 | 描述 |
| --- | --- |
| nameserver 192.168.0.1 | 域名服务器 |

> 域名服务器可以有多个，换行写或者后面使用空格分割
> 当网卡配置文件中的**`BOOTPROTO=dhcp`**时，该文件可以为空，不用配置

### 3、重启网络服务

完成上面的配置后，我们的网卡配置就完成了，下面我们需要重启一下网络服务来让网卡工作，执行命令：`service network restart`，如下图：

![CentOS_network_restart-c](http://pbe07x0ww.bkt.clouddn.com/CentOS_network_restart.png)

网络服务重启完成之后我们就可以执行命令：`ifconfig`来查看Linux系统的IP地址，也可以访问互联网了，如下图：

![CentOS_ifconfig-c](http://pbe07x0ww.bkt.clouddn.com/CentOS_ifconfig.png)

OK，到这里为止，Linux网络管理我们就全部讲完了！！

## 五、Linux 用户管理

在完成了网络配置这一小小的难关之后，我们讲是不是就可以开始使用我们的Linux系统了呢？其实理论上是可以了，但生活嘛，总是理想很丰满，现实很骨干啊！不知道大家有没有留意到一个细节，就是我们在安装Linux系统的时候从来没有要求过我们输入用户名，只是让我们输入了一个密码而已，咦，这好像跟我们安装界面话的操作系统，比如：Windows或者macOS 似乎都不太一样哎，这是为什么呢？OK，下面我们就来学习一下Linux的用户管理。

### 1、用户与用户组

现在解释一下为什么之前安装Linux的时候没有让我们输入用户名，这是因为Linux采用无界面化安装的时候她并不会主动为我们分配用户，她会为自己创建一个统一的`root`用户，她认为剩下的事情都应该由`root`用户来操作，同时也给这个`root`用户赋予了极大的权限，后面我们讲到**Linux 权限管理**的时候大家就会体会到这个`root`用户是有多无敌，多寂寞了。也正是因为这一点我们才需要管理好我们Linux的用户。

简单的说，用户就是使用该操作系统的人，而用户组是指具有相同权限的一组用户。我们一开始就说**“Linux一切皆文件！”**，那么用户和用户组对应的文件又在哪里呢？在开始讲这些文件之前，我需要大家先了解一个Linux文件编辑器——***VIM***，这里不会赘述怎么使用这个编辑器，请大家自行参考：[简明 VIM 练级攻略](https://coolshell.cn/articles/5426.html)

#### 1.1 用户组信息文件

当前系统中所有用户组信息都保存在`/etc/group`这个文件中，执行`vim /etc/group`可以看到如下内容：

![CentOS_etc_group-c](http://pbe07x0ww.bkt.clouddn.com/CentOS_etc_group.png)

每一行的内容以冒号分隔，释义如下：

| group | x | 123 | abc,def,xyz |
| --- | --- | --- | --- |
| 组名称 | 组密码占位符 | 组编号 | 组中用户列表 |

> 组密码占位符：一定是x
> 组编号：root用户组的编号一定是0，1~499是系统预留组编号，一般是预留给安装的软件或服务的。
> 组中用户列表：为空并不代表该组中就一定没有用户，因为如果该组中有且仅有一个用户，并且用户名和组名一致，这时候是可以省略的

#### 1.2 用户组的密码信息文件

当前系统中所有用户组的密码信息都保存在`/etc/gshadow`这个文件中，执行`vim /etc/gshadow`可以看到如下内容：

![CentOS_etc_gshadow-c](http://pbe07x0ww.bkt.clouddn.com/CentOS_etc_gshadow.png)

与用户组信息一样，每一行的内容以冒号分隔，释义如下：

| group | 空/* | 空 | abc,def,xyz |
| --- | --- | --- | --- |
| 组名称 | 组密码 | 组管理者 | 组中用户列表 |

> 组密码：为空或为*号的时候都表示当前组没有密码
> 组管理者：一般都为空，表示组内任何用户都可以管理该组

#### 1.3 用户信息文件

当前系统中所有用户信息都保存在`/etc/passwd`这个文件中，执行`vim /etc/passwd`可以看到如下内容：

![CentOS_etc_passwd-c](http://pbe07x0ww.bkt.clouddn.com/CentOS_etc_passwd.png)

同样，每一行的内容以冒号分隔，释义如下：


| user | x | 123 | 456 | xxx | /home/user | /bin/bash |
| --- | --- | --- | --- | --- | --- | --- |
| 用户名 | 密码占位符 | 用户编号 | 用户组编号 | 用户注释信息 | 用户家目录 | shell类型 |

> 用户家目录：创建用户时系统会自动在/home目录下创建一个与用户名同名的目录作为用户的家目录，用户登录系统时自动到该目录下
> shell类型：用来指定用户登录时执行命令的方式，一般是bash

#### 1.4 用户密码信息文件

当前系统中所有用户信息都保存在`/etc/shadow`这个文件中，执行`vim /etc/shadow`可以看到如下内容：

![CentOS_etc_shadow-c](http://pbe07x0ww.bkt.clouddn.com/CentOS_etc_shadow.png)

这个文件就没什么好说的了，其中第二个字段就表示用户密码，采用的是hash加盐的加密方式。

OK，和用户和用户组相关的文件都介绍完了，这几个文件我们只要了解就行，一般不会去修改它，因为我们操作用户和用户组都有其他方式。

### 2、基本命令

与操作用户组相关的基本命令如下：

* `groups [username]`: 查看用户所属组，不写参数默认查看当前登录用户所属组
* `groupadd groupname`: 添加用户组
* `groupdel groupname`: 删除用户组
* `groupmod -n [new_groupname] [old_groupname]`: 修改用户组名称
* `groupmod -g groupid groupname`: 修改用户组id

与操作用户相关的基本命令如下：

* `useradd username`: 添加用户，默认添加用户时会创建一个同名用户组，添加的用户属于该用户组，同时在/home目录下会创建一个同名目录为该用户的家目录
* `useradd -g groupname username`: 添加用户到指定用户组
* `useradd -d /home/xxx username`: 添加用户时指定家目录
* `userdel username`: 删除用户
* `userdel -r username`: 删除用户同时删除用户的家目录
* `usermod -l new_username old_username`: 修改用户名
* `usermod -d /home/xxx username`: 修改用户家目录
* `usermod -g groupname username`: 修改用户所属组

