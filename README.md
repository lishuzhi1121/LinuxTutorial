# 全栈最后一公里 —— Linux从入门到实践

![full_stack-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/full_stack.jpg)

> 傻瓜都能写出计算机能理解的程序，而好的程序员能写出人能读懂的代码。

[TOC]

# 入门篇

## 一、前世今生

![linux_icon-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/linux_icon.png)

### 1、系统来源

在Linux之前有一个叫 *Andrew S. Tanenbaum* 的教授为了给他的学生上课买了一个UNIX操作系统源码，然后参考这个系统但是没有任何抄袭成分的做了一个叫作Minix的操作系统，后来他把这个Minix操作系统的源码全部开放给学校和学生做学习研究使用，再后来经过一番优化之后他把这个系统就直接放到网络上去开源了，当时就一下受到很多人的关注，开源免费的操作系统非常受欢迎，很多人就开始使用，但是用着用着就有人发现这个系统有些bug，于是就向这个教授提了很多修复补丁程序希望教授可以更新修复，但是呢这个教授不这么想，他说：“我写这个代码就只是为了教学，你们能看得懂我的目的就达到了，我不需要任何外来的代码也没有想过把它商业发行。”

那么这个时候另一个家伙出现了，*Linus Torvalds*，当时是芬兰赫尔辛基大学大三的大学生，他以Minix为模板，自己开发了一些软件和并且集中了当时网上呼声较高的一些补丁程序重新写了一个操作系统——Linux操作系统。

![Linus_Torvalds-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/Linus_Torvalds.png)

Linus选择了芬兰的吉祥物企鹅作为该系统的标识，并在1991年的时候就将Linux操作系统发布了，短短20多年的时间，Linux系统就已经在服务器领域占据了举足轻重的地位。

### 2、系统版本

Linux经过20多年的发展已经拥有了非常众多的版本，但是我们需要知道的是这些版本中分为Linux内核版本和Linux发行版本，这两个版本不太一样。

#### Linux内核版本：

Linux内核版本就是核心版本，它由 [Linux内核官网](https://www.kernel.org/) 发行，目前最新的内核版本是4.19版本。

![Linux_core-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/Linux_core.png)

> 值得一提的是：对于服务器领域而言，一般不会采用最新版本，线上我们要本着“猥琐发育，不要浪”的原则！

#### Linux发行版本：

内核版本是由官网发布，任何人都可以去免费下载使用的，但是比如基于内核版本之上开发一些工具，开发一个桌面，放点好看的图标，或者做一些裁剪等，那么它就会变成各个厂商的发行版本。

![Linux_versions-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/Linux_versions.png)

在上面众多版本中，比较流行的主要是Redhat和Ubuntu，其中Ubuntu的优势主要在于界面优化的非常漂亮，有的时候都认不出来这是一个Linux系统，但是这些花里胡哨的东西并不适合服务器使用，上面我们说过服务器一定要稳定优先，所以一般不会在服务器上去安装图像界面，所以下面本教程也会以Redhat系列来进行，只不过有一点需要说的是Redhat部分功能是收费的，所以我们会以Redhat的替身CentOS操作系统为例进行讲解。

## 二、Linux系统安装

在开始学习Linux系统之前我们迫切需要知道如何把Linux系统给安装上，但是呢作为一个初学者，如果直接在自己的电脑上就装一个Linux操作系统，那我估计你可能痛苦的坚持不了3天就要重装系统了，所以就引入了下面的虚拟机相关知识。

### 1、虚拟机安装

在Mac上（PS：下述所有内容如无特殊说明都是基于Mac进行）常用的虚拟机就两种 [VMware](https://www.vmware.com/cn.html) 和 [Parallels Desktop](https://www.parallels.com/cn/) ，相同点是这两个都收费，而且价格不菲，不同点是我这里有可以使用的“~~正版VMware~~”，下载链接:https://pan.baidu.com/s/1dtnZ1V_evPFbJCrER2l9mg  密码:6o3p 关于如何在Mac上把VMware安装上就不作过多赘述了，与安装其他软件没有什么区别，一路Next然后完成之后使用天朝独特的Keygen去“~~优化~~（破解）”一下就可以使用了。

![VMware_wel-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/VMware_wel.png)

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

![VMware_chos-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/VMware_chos.png)

然后选择**“Linux”**->**“CentOS 64位”**点击**“继续”**，画面如下：

![VMware_chhd-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/VMware_chhd.png)

选择**“新建虚拟磁盘”**，点击**“继续”**，就可以完成虚拟机的创建了，出现如下画面，点击**“完成”**，选择好虚拟机文件存放路径，即可完成虚拟机的创建。

![VMware_finish-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/VMware_finish.png)

#### 3.2 安装Linux

和Windows或者macOS操作系统一样，安装都需要一个系统镜像来完成安装，我们可以从[CentOS官网](https://www.centos.org/)下载各种版本的系统镜像，本教程使用的系统镜像是[CentOS 6.9 64位](http://vault.centos.org/6.9/isos/x86_64/)

下载好系统镜像之后，打开刚安装好的虚拟机，点击**“设置”**按钮，打开设置面板：

![VMware_st-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/VMware_st.png)

选择**“CD/DVD(IDE)”**：

![VMware_cd-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/VMware_cd.png)

勾选“连接 CD/DVD 驱动器”，然后下拉选择“选择一个光盘或光盘映像...”，选择我们下载好的系统镜像：

![VMware_sel_iso-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/VMware_sel_iso.png)

![VMware_iso-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/VMware_iso.png)

这就跟我们平时安装电脑在光驱里插入系统光盘一样，下面我们就可以启动虚拟机进行Linux系统安装了。

启动虚拟机，虚拟机会自动读取光驱里的系统镜像，从而启动Linux系统的安装程序，如下图：

![CentOS_install_start-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_install_start.png)

默认选择第一项即可，直接回车，会出现这样一个镜像数据完整性检测界面，简单的说就是用来检测你下载的这个系统镜像是不是可以使用，这里我们一般都认为是可用的不再进行检测，所以直接选择**“Skip”**：

![CentOS_install_check-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_install_check.png)

之后就很简单了，一路**“Next”**就行：

![CentOS_installz_wel-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_installz_wel.png)

![CentOS_install_lan-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_install_lan.png)

![CentOS_install_kb-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_install_kb.png)

![CentOS_install_bs-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_install_bs.png)

下面这里需要注意一下，如果曾经虚拟机安装过可能会有这个提示，直接忽略掉就可以了，选择**“是，忽略所有数据”**，然后点击**“下一步”**：

![CentOS_install_dc-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_install_dc.png)

下面这里的主机名默认就好，因为对于Linux来说同一个局域网里并不是靠主机名来进行通信的，所以同一个局域网里可以有多个主机名相同的Linux计算机，这一点和Windows不一样。继续**“下一步”**：

![CentOS_install_host-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_install_host.png)

![CentOS_install_time-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_install_time.png)

![CentOS_install_pwd-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_install_pwd.png)

在上面设置好密码之后就到下面选择安装类型了，其实就是选择分区方式，这里前面我们讲了关于Linux系统分区的相关知识，所以我们选择**“创建自定义布局”**，然后点击**“下一步”**：
![CentOS_install_layout-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_install_layout.png)

下一步之后我们看到了如下的图形化分区界面，选择**“空闲”**的硬盘驱动器，点击**“创建”**来创建我们想要的分区：
![CentOS_install_layout_start-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_install_layout_start.png)

选择**“标准分区”**，点击**“创建”**：

![CentOS_install_layout_c-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_install_layout_c.png)

选择好**“挂载点”**和**“大小”**，点击**“确定”**：

![CentOS_install_layout_hm](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_install_layout_hm.png)

重复上面的创建分区操作，根据之前说的系统分区要求做好分区后点击**“下一步”**，完整的分区创建完成后如下图：

![CentOS_install_full_lay-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_install_full_lay.png)

选择**“格式化”**，点击**“下一步”**：

![CentOS_install_fm-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_install_fm.png)

选择**“将修改写入磁盘”**，点击**“下一步”**：

![CentOS_install_wr-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_install_wr.png)

![CentOS_install_in-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_install_in.png)

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
![CentOS_install_basic-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_install_basic.png)

下面就启动了系统安装程序，静静的等待就好...

![CentOS_install_ing0-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_install_ing0.png)

![CentOS_install_ing1-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_install_ing1.png)

![CentOS_install_ing2-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_install_ing2.png)

经过了上面漫长的等待，出现下面的画面，我们就完成了Linux系统的安装，点击**“重新引导”**：

![CentOS_install_cmp-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_install_cmp.png)

重新引导完成后，CentOS系统启动，启动成功后界面如下，只有一个命令行字符界面，其他什么都没有:

![CentOS_start-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_start.png)

我们输入用户名：root和密码即可登录，登录成功后如下图：

![CentOS_login-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_login.png)

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

![CentOS_files-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_files.png)


![linux-file-system-hierarchy-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/linux-file-system-hierarchy.png)


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

![ISO:OSI七层网络模型-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/ISO:OSI七层网络模型.png)

#### 1.2 TCP/IP四层网络模型

我们说上面的七层网络模型很经典，但是在实际应用中很多时候网络架构并不是这样的，而是采用TCP/IP四层网络模型，那么它们之间到底有怎样的联系呢？我们来看下图：

![IMG_A479D37BC665-1-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/IMG_A479D37BC665-1.jpeg)

#### 1.3 关于IP地址

从上面的网络模型中我们能够知道，IP其实是工作在网络层的互联网协议，它的作用这里不作过多阐述，主要想给大家说的是IP的分类，IP地址分为A~E五大类，但是其中D类IP、E类IP并不开放民用，所以下面是A、B、C三类IP的划分情况：

| 类别 | 最大网络数 | IP地址范围 | 最大主机数 | 私有IP地址范围 |
| --- | --- | --- | --- | --- |
| A | 126 (2^7 - 2) | 1.0.0.0~126.255.255.255 | 2^24 - 2 | 10.0.0.0~255.255.255.255 |
| B | 16384 (2^14) | 128.0.0.0~191.255.255.255 | 2^16 - 2 | 172.16.0.0~172.31.255.255 |
| C | 2097152 (2^21) | 192.0.0.0~223.255.255.255 | 2^8 - 2 | 192.168.0.0~192.168.255.255 |

#### 1.4 关于子网掩码

子网掩码是一种用来指明一个IP地址的哪些位标识的是主机所在的子网，以及哪些位标识的是主机的位掩码。好吧，说人话就是子网掩码是用来确定两个IP是否属于同一个网段的。子网掩码不能单独存在，它必须结合IP地址一起使用。既然这样我们就结合上面三类IP地址来看一下子网掩码，一般情况下，不同类别的IP地址与子网掩码具有如下对应关系：

![IMG_D5346D0871D2-1-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/IMG_D5346D0871D2-1.jpeg)

> A类IP地址与子网掩码的对应关系

![IMG_14FE850B0E9A-1-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/IMG_14FE850B0E9A-1.jpeg)

> B类IP地址与子网掩码的对应关系

![IMG_9F70E8FDFEF3-1-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/IMG_9F70E8FDFEF3-1.jpeg)

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

![IMG_1867-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/IMG_1867.PNG)

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

![CentOS_network_restart-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_network_restart.png)

网络服务重启完成之后我们就可以执行命令：`ifconfig`来查看Linux系统的IP地址，也可以访问互联网了，如下图：

![CentOS_ifconfig-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_ifconfig.png)

OK，到这里为止，Linux网络管理我们就全部讲完了！！

## 五、Linux 用户管理

在完成了网络配置这一小小的难关之后，我们讲是不是就可以开始使用我们的Linux系统了呢？其实理论上是可以了，但生活嘛，总是理想很丰满，现实很骨干啊！不知道大家有没有留意到一个细节，就是我们在安装Linux系统的时候从来没有要求过我们输入用户名，只是让我们输入了一个密码而已，咦，这好像跟我们安装界面话的操作系统，比如：Windows或者macOS 似乎都不太一样哎，这是为什么呢？OK，下面我们就来学习一下Linux的用户管理。

### 1、用户与用户组

现在解释一下为什么之前安装Linux的时候没有让我们输入用户名，这是因为Linux采用无界面化安装的时候她并不会主动为我们分配用户，她会为自己创建一个统一的`root`用户，她认为剩下的事情都应该由`root`用户来操作，同时也给这个`root`用户赋予了极大的权限，后面我们讲到**Linux 权限管理**的时候大家就会体会到这个`root`用户是有多无敌，多寂寞了。也正是因为这一点我们才需要管理好我们Linux的用户。

简单的说，用户就是使用该操作系统的人，而用户组是指具有相同权限的一组用户。我们一开始就说**“Linux一切皆文件！”**，那么用户和用户组对应的文件又在哪里呢？在开始讲这些文件之前，我需要大家先了解一个Linux文件编辑器——***VIM***，这里不会赘述怎么使用这个编辑器，请大家自行参考：[简明 VIM 练级攻略](https://coolshell.cn/articles/5426.html)

#### 1.1 用户组信息文件

当前系统中所有用户组信息都保存在`/etc/group`这个文件中，执行`vim /etc/group`可以看到如下内容：

![CentOS_etc_group-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_etc_group.png)

每一行的内容以冒号分隔，释义如下：

| group | x | 123 | abc,def,xyz |
| --- | --- | --- | --- |
| 组名称 | 组密码占位符 | 组编号 | 组中用户列表 |

> 组密码占位符：一定是x
> 组编号：root用户组的编号一定是0，1~499是系统预留组编号，一般是预留给安装的软件或服务的。
> 组中用户列表：为空并不代表该组中就一定没有用户，因为如果该组中有且仅有一个用户，并且用户名和组名一致，这时候是可以省略的

#### 1.2 用户组的密码信息文件

当前系统中所有用户组的密码信息都保存在`/etc/gshadow`这个文件中，执行`vim /etc/gshadow`可以看到如下内容：

![CentOS_etc_gshadow-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_etc_gshadow.png)

与用户组信息一样，每一行的内容以冒号分隔，释义如下：

| group | 空/* | 空 | abc,def,xyz |
| --- | --- | --- | --- |
| 组名称 | 组密码 | 组管理者 | 组中用户列表 |

> 组密码：为空或为*号的时候都表示当前组没有密码
> 组管理者：一般都为空，表示组内任何用户都可以管理该组

#### 1.3 用户信息文件

当前系统中所有用户信息都保存在`/etc/passwd`这个文件中，执行`vim /etc/passwd`可以看到如下内容：

![CentOS_etc_passwd-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_etc_passwd.png)

同样，每一行的内容以冒号分隔，释义如下：


| user | x | 123 | 456 | xxx | /home/user | /bin/bash |
| --- | --- | --- | --- | --- | --- | --- |
| 用户名 | 密码占位符 | 用户编号 | 用户组编号 | 用户注释信息 | 用户家目录 | shell类型 |

> 用户家目录：创建用户时系统会自动在/home目录下创建一个与用户名同名的目录作为用户的家目录，用户登录系统时自动到该目录下
> shell类型：用来指定用户登录时执行命令的方式，一般是bash

#### 1.4 用户密码信息文件

当前系统中所有用户信息都保存在`/etc/shadow`这个文件中，执行`vim /etc/shadow`可以看到如下内容：

![CentOS_etc_shadow-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_etc_shadow.png)

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

> `passwd username`: 设置/修改用户密码
> 注意：创建用户之后必须设置密码才可以登录系统！！

## 六、Linux 权限管理

这里我们所说的权限管理实际上就是指Linux的文件权限管理，因为Linux一切皆文件！下面我们就来看一下Linux文件权限是怎么管理的。执行`ls -l /etc`可以看到如下内容：

![CentOS_ls_etc-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_ls_etc.png)

图中红色框部分就表示对应文件的权限，详细定义如下：

![file_rights-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/file_rights.png)

> 关于文件类型：除了上面图中展示的三种类型的文件之外还有其他类型的文件，但是，友情提示，如果你不是特别熟悉Linux的文件系统，那么当你看到除以上三种类型文件的时候请你绕着走！
> 关于特殊权限：我就只说一点，如无特殊需求，请无视它，谢谢！

### 1、文件权限的修改

修改文件权限的命令格式为：`chmod [选项] 模式 文件名`

* 选项：选项有很多，但常用的就一个，`-R`表示递归修改
* 模式：分为两种，一种是字母模式`[ugoa][+-=][rwx]`，意思为“为哪一组权限添加、删除或赋予什么样的权限，其中a表示所有组（包括所有者、所属组和其他人）”，例如：`chmod u+x test.sh`表示为`test.sh`这个文件的所有者添加可执行权限；另外一种是数字模式`[mode=421]`，采用rwx对应的数字来表示权限，每一个权限之间是与的关系，所以采用加法运算，每一组权限只用一个数字表示，例如：`chmod 744 test.sh`表示将`test.sh`这个文件的权限修改为其所有者具有读写执行权限，而其所属组和其他人只有读权限。

> 关于模式：可能一眼看上去字母模式更清晰，但说了你可能不信，这种模式使用度几乎为零，原因有二，其一字母模式你需要知道原文件是什么权限，其二字母并不如数字简单好记。

### 2、文件权限的作用

权限是Linux文件系统的核心，本质目的还是为了安全，其实前面介绍完文件权限的时候大家对权限的作用就基本上心里有数了，这里我需要敲黑板的只有一个问题：**“对文件有读写权限就能删除文件吗？？”**

![CentOS_rm_test-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_rm_test.png)

从上图我们就可以看到，不是这样的，就算你对文件有读写执行的一切权利，也不代表你就能删除文件，主要原因跟Linux的文件系统和存储结构有关，简单的解释为你对这个文件的权限只是表示了你对这个文件内的数据所拥有的权限，而对于一切皆文件的Linux来说，这个文件本身实际上是存储在`/home/sands/test`文件夹下的，所以它其实是这个文件夹文件内的数据，要操作它那就得拥有这个文件夹的权限。

![CentOS_ll_test-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_ll_test.png)

> 可以看到我们当前用户对`test`文件夹并不具备写权限，也就解释了为什么我们无法将这个文件夹里面的`test.sh`文件删掉了。

**_注意：我们这里所说的权限`root`用户不适用！！！_**

另外，这里补充一点关于权限对目录的作用：

* r: 可以查询目录下的文件（ls）
* w: 具有修改目录结构的权限，例如：新建、删除、复制、剪切（touch, rm, cp, mv）
* x: 可以进入目录（cd）

> **总结：**对于文件来说，最高权限是执行（x），但对于目录来说，最高权限是写（w）。所以对于目录有效权限只有0（没有任何权限）、5（有读r和执行x权限，可以查看内容）、7（有任何权限），**禁止**对文件夹赋予像1、4、6等这些没有意义的权限。

### 3、文件所属的修改

修改文件的所属者和所属组的命令格式为：`chown [选项] ower:[groupname] filename`，其中`[选项]`有很多，但常用的只有一个`-R`表示递归修改，`groupname`是可选的，例如：

* `chown sands test.sh`表示只将`test.sh`这个文件的所属者改为`sands`用户
* `chown sands:usergroup test.sh`表示将`test.sh`这个文件的所属者改为`sands`用户，所属组改为`usergroup`用户组

### 4、文件的默认权限

说了这么多文件权限的东西，似乎我们还忽略了一点，就是如果我们直接创建一个文件，那么它的权限是什么呢？为什么是这样呢？

![CentOS_ll_664-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_ll_664.png)

从上图可以看到，在非root用户下直接创建一个文件默认权限为664，下面我们就来解释一下这是为什么：

在开始解释之前，我需要大家明白一个原则性问题，之前我们也提到过，对于Linux来说文件的最高权限是x执行，但是Linux认为这个最高权限太危险了，所以文件默认是不能新建为可执行文件的，必须手动赋予执行权限，这样一来文件默认的最大权限就只能是666了。那是不是说新建的文件默认权限就是666了呢？显然不是，这还得受一个叫作`umask`值的家伙的管制：

![CentOS_umask-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_umask.png)

> 第一位0：表示文件特殊权限
> 后面三位002：表示文件默认权限

所以默认文件权限的具体计算方式如下图：

![CentOS_file_default-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_file_default.png)

现在我们知道了Linux文件的默认权限是这么来的，那其实文件夹默认权限也是同样的方式计算来的，但不同的是我们前面说过文件夹的最高权限是w写，所以Linux就认为文件夹的写并不具备什么危险，所以文件夹默认的最大权限就是777，所以计算默认权限的时候也应该用777来进行计算。

关于umask值的修改：

* 临时修改：直接执行`umask 0022`
* 永久修改：修改`/etc/profile`这个环境变量文件

![CentOS_etc_profile_umask-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_etc_profile_umask.png)

## 七、Linux 软件安装管理

软件安装对于任何操作系统来说都是非常重要的一部分，原因很简单，我们安装操作系统就是要做事儿的，而一个空的操作系统能帮我们做的事情微乎其微，所以我们才需要通过安装各种软件来为我们服务。

### 1、挂载

在讲软件安装之前先给大家讲一个概念叫做**挂载**，我们都知道在macOS和Windows中，当我们把光盘放入光驱或者是优盘插入USB接口的时候在我们的电脑里就能查看到对应的里面的内容了，但是很不幸的告诉你，在Linux中并不是这样的。我们在讲Linux系统分区的时候就说过，硬盘每个分区的盘符称为“挂载点”，读取分区的过程称为“挂载”，其实对于其他设备也一样，我们也要通过挂载的方式来读取设备内容。

一般我们选择将光盘挂载到系统的`/media`文件夹下，而将优盘等挂载到系统的`/mnt/xxx`文件夹下，xxx的意思是指在`/mnt`文件夹下再创建一个文件夹来挂载对应设备，挂载命令如下：

* 挂载光盘：`mount /dev/sr0 /media`表示将光盘挂载到`/media`目录下，如下图：

![CentOS_mount_sr0-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_mount_sr0.png)

* 挂载优盘：先执行`fdisk -l`来查看优盘设备文件名，再执行`mount /dev/sdb1 /mnt/usb`来将文件名为sdb1的优盘挂载到mnt文件夹下的usb文件夹里

> 卸载已经挂载好的设备也很简单：`umount /dev/sr0`即可卸载已经挂载的光盘。

### 2、压缩和解压缩

快要吐血了啊，要讲一个东西涉及的实在是太多了，为了安装软件我们应当知道软件包是如何进行压缩和解压缩的？

* `.zip`格式压缩命令为：`zip [选项] 压缩包名 源文件名`，例如：`zip test.zip test.sh`表示将`test.sh`这个文件压缩为`test.zip`，常用选项为`-r`压缩文件夹，例如：`zip -r test.zip test`表示将`test`这个文件夹压缩为`test.zip`
* `.zip`格式解压缩命令为：`unzip 压缩包名`解压缩到当前文件夹
* `.gz`格式压缩命令为：`gzip 源文件名`，例如：`zip cangls`表示将`cangls`这个文件压缩为`cangls.gz`。**_需要注意的是该命令压缩后源文件会消失，并且不适合压缩文件夹_**。
* `.gz`格式解压缩命令为：`gzip -d 压缩包名`

以上这两种格式都可以压缩文件，但是！！！在Linux中并不常用，原因是因为他们的姿势实在是不够风骚，用着很不舒服。

为了解决上面的问题，Linux为我们准备了一个打包命令`tar`，命令格式为：`tar -cvf 打包后的包名 源文件/文件夹`，同样也对应一个解打包命令：`tar -xvf 打包后的包名`

> * -c: 打包
> * -x: 解包
> * -v: 显示过程
> * -f: 指定打包后的文件名

有了这个命令之后我们就可以将一切文件或文件夹先打包再压缩，这样就不会出现文件夹不能压缩的问题了，但是如果真的这样做，总觉得不得劲，难倒就不能一步到位吗？肯定是可以的。

**压缩和解压缩的终极解决方案：**其实`tar`命令是支持直接压缩为`.tar.gz`格式的，命令为`tar -zcvf 压缩包名.tar.gz 源文件/文件夹`与上面相比选项中多了个`z`，即表示将打包后的文件压缩为`.gz`格式的压缩包。同理，`.tar.gz`格式的压缩包对应的解压命令为`tar -zxvf 压缩包名.tar.gz`

> Tips：`.tar.gz`也是Linux软件源码包最常见的压缩格式。

### 3、软件包安装

在Linux中，软件包分为源码包和二进制包（RPM包）两种，其实本来就只有源码包一种，但是由于源码包安装需要经过配置、编译、安装这些繁琐的步骤，安装速度极其慢，而且很容易报错，所以才出现了二进制包，它其实就是由源码包配置编译来的。Linux的安装镜像里就已经包含了所有编译好的RPM包供我们直接安装使用，我们只有将系统镜像挂载进来就可以了。

#### 3.1 RPM包手动安装

RPM包安装命令为：`rpm -ivh 包全名`，选项释义如下：

* -i: install 安装
* -v: verbose 显示详细信息
* -h: hash 显示进度

命令很简单，但是过程一点都不简单，下面我们用一个小示例来看一下RPM包的安装过程：

![CentOS_rpm_httpd_1-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_rpm_httpd_1.png)

![CentOS_rpm_httpd_2-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_rpm_httpd_2.png)

![CentOS_rpm_httpd_3-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_rpm_httpd_3.png)

从上面的安装过程中我们可以看到，RPM包之间还有依赖关系，而且错综复杂，难以捉摸，所以如果按照这种方式去装软件，那估计软件安装好黄花菜都凉了~

#### 3.2 yum在线安装

为了解决上面RPM包安装过程繁琐的问题，Linux将所有软件包放到官方服务器上，并且开发了一个yum安装工具，当进行yum在线安装的时候，它可以自动解决依赖问题。类似于iOS的CocoaPods，Java的Maven，前端的npm一样。

##### yum源 配置文件

yum是Redhat系列的Linux自带的软件安装工具（Debian系列是apt-get），yum源就是指存放所有软件包的服务器地址，它的默认配置文件为：`/etc/yum.repos.d/CentOS-Base.repo`

![CentOS_yum_base-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_yum_base.png)

配置文件中的每一段内容都相似，具体字段说明如下表：

| 字段名称 | 描述 |
| --- | --- |
| [base] | 容器名称，一定要放在[]中 |
| name | 容器说明，内容自己随意 |
| mirrorlist | 镜像站点，与baseurl二选一，即软件包的服务器地址 |
| baseurl | yum源服务器地址，默认是CentOS官方yum源，国内可能速度较慢，建议换成国内的[网易镜像源](http://mirrors.163.com/.help/centos.html)或者[阿里镜像源](https://opsx.alibaba.com/mirror) |
| enabled | 设置此容器是否生效，省略不写或写成enabled=1都是生效，写成enabled=0则不生效 |
| gpgcheck | RPM的数字证书是否生效，1表示生效，0表示不生效 |
| gpgkey | 数字证书的公钥文件保存位置，不要修改。 |

##### 光盘搭建本地yum源

很多时候在有网络的情况下我们使用yum在线源来安装软件就可以了，但是偏偏他就有这么一种情况，没有网络！！那么这个时候我们又想起了我们的系统镜像光盘里其实已经有所有我们需要的软件包了，那么它不就相当于是一个yum源了吗？没错，确实是这样的，所以我们可以使用光盘来做本地yum源，步骤如下：

* 挂载光盘：`mount /dev/sr0 /media`
* 使网络yum源失效：在上面配置文件介绍的时候我们知道，只需要将配置文件里容器的enabled值设置为0就可以了，但是默认配置文件中有好几个容器，每个都设置一遍比较麻烦，所以既然yum源的默认配置文件为`/etc/yum.repos.d/CentOS-Base.repo`，那么如果我将这个文件删掉它就不起作用了吧？这样是可以，但是我们说对于系统默认配置文件我们要本着**_只备份不删除_**的原则，所以我们只需要将这个文件改个名备份一下即可`mv CentOS-Base.repo CentOS-Base.repo.bak`
* 使光盘yum源生效：修改光盘的yum配置文件`/etc/yum.repos.d/CentOS-Media.repo`，如下图：
![CentOS_yum_media](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_yum_media.png)

完成上述步骤后执行`yum list`即可验证本地yum源是否生效。

##### 通过yum安装软件

首先，我们可以通过`yum search 关键字`来查询yum源里是否有需要的软件包。

yum安装命令为：`yum -y install 包名`，其中选项`-y`表示自动回答yes，例如：`yum -y install gcc`

上面我们使用rpm命令手动艰难的安装了httpd（也就是Apache服务器软件）的主包和`httpd-tools`，还有`httpd-manual`文档包和`httpd-devel`开发包没有安装，我们使用yum安装`yum -y install httpd-manual httpd-devel`

> Apache是个急性子，安装好就自动启动了。
> RPM包安装的Apache手动启动的方式有两种：一种是安装好的可执行命令启动：`/usr/sbin/httpd -k start`或者`/usr/sbin/apachectl start`，另外一种是通过Linux服务启动：`service httpd restart`

#### 3.3 源码包安装

源码包安装时一定要安装在指定位置，因为通过源码包安装的软件没有卸载命令，所以如果你想卸载的时候只要把指定位置下的文件全部删掉就可以了，安装位置一般是：`/usr/local/软件名/`

使用源码包安装之前必须保证系统已安装`gcc`：

![CentOS_rpm_gcc-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_rpm_gcc.png)

下载源码包，一般Linux的软件包都有对应的官方网站下载地址，我们下面以Apache软件来讲解，Apache 2.4下载地址为：[http://mirror.bit.edu.cn/apache/httpd/httpd-2.4.37.tar.gz](http://mirror.bit.edu.cn/apache/httpd/httpd-2.4.37.tar.gz)，执行`wget http://mirror.bit.edu.cn/apache/httpd/httpd-2.4.37.tar.gz`即可将软件源码包下载到当前路径下。

![CentOS_wget_1-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_wget_1.png)

![CentOS_wget_2-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_wget_2.png)

> 1.由于Apache 2.4依赖Apache的运行环境下的两个包 [apr](http://mirror.bit.edu.cn/apache//apr/apr-1.6.5.tar.gz) 和 [apt-util](http://apache.website-solution.net//apr/apr-util-1.6.1.tar.gz)
> 2.所以采用同样的wget方式将其下载下来
> 3.另外，Apache编译时还依赖了`pcre-config`，它属于`pcre-devel`，所以我们要使用`yum -y install pcre-devel`命令安装好这个依赖包

全部下载完成后将其一一解压缩到下载的目录下：

![CentOS_tar_1-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_tar_1.png)

解压完成之后，首先我们要安装的是`apr`，所以需要进入到解压后的`apr-1.6.5`目录下，然后依次执行：

* `./configure --prefix=/usr/local/apr`: 软件配置与检查，主要是将软件安装的功能选项和系统环境信息写入Makefile，为后续的make做准备，**这里`--prefix=PATH`一定不能省略**；
* `make`：编译，这个不用多说。清空编译缓存执行`make clean`；
* `make install`：安装；

安装完成之后，在`/usr/local/apr`下就生成了相应的文件。

然后我们需要安装的是`apr-util`，所以需要进入到解压后的`apr-util-1.6.1`目录下，然后依次执行：

* `./configure --prefix=/usr/local/apr-util --with-apr=/usr/local/apr`: `apr-util`编译时要依赖`apr`，所以这里的**`--with-apr=PATH`是用来指定编译`apr-util`时依赖的`apr`使用哪一个**；
* `make`：编译；
* `make install`：安装；

安装完成之后，在`/usr/local/apr-util`下就生成了相应的文件。

最后，我们进入解压好的`httpd-2.4.37`目录下：

![CentOS_tar_2-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_tar_2.png)

依次执行：

* `./configure --prefix=/usr/local/apache2 --with-apr=/usr/local/apr --with-apr-util=/usr/local/apr-util`: 编译时要依赖`apr`和`apr-util`；
* `make`：编译；
* `make install`：安装；

安装完成之后可以看到如下效果：

![CentOS_apache2-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_apache2.png)

执行`/usr/local/apache2/bin/apachectl start`即可开启Apache服务器，通过浏览器访问效果如下（关闭防火墙）：

![CentOS_httpd_work-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/CentOS_httpd_work.png)

## 八、Linux 防火墙（iptables）

### 1、关于防火墙

首先，什么是防火墙？从逻辑上讲，防火墙可以大体分为主机防火墙和网络防火墙。主机防火墙针对于单个主机进行防护，而网络防火墙往往是处于网络入口或边缘，针对网络入口进行防护。网络防火墙和主机防火墙并不冲突，可以理解为网络防火墙主外（集体），主机防火墙主内（个人）。从物理上讲，防火墙又可分为硬件防火墙和软件防火墙。硬件防火墙在硬件级别就实现了部分防火墙功能，另一部分靠软件实现，性能高，成本也高。软件防火墙就是通过一些软件处理逻辑运行于通用硬件平台上的防火墙，性能低，成本也低。

![Cisco_Firepower-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/Cisco_Firepower.png)

### 2、关于iptables

![iptables-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/iptables.png)

**iptables** 其实并不是真正的防火墙，我们可以把它理解成客户端代理，用户通过这个代理将安全设定执行到对应的 **安全框架** 中，这个 **安全框架** 才是真正的防火墙 —— **netfilter** 。由于 **netfilter** 是位于内核空间的，而 **iptables** 是位于用户空间的一个命令，所以我们用它来操作 **netfilter** 。

在CentOS中对应的命令和服务就是 **iptables** ，可以使用 `service iptables start` 来启动 **iptables** 服务，那么对应的就有 `service iptables stop` 停止和 `service iptables restart` 重启 **iptables** 服务。

### 3、iptables规则组成

可以使用 `iptables -L` 命令查看系统当前的iptables规则，如下图：

![iptables-rule-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/iptables-rule.png)

其实这段规则不太看得懂，因为它是来自于一个文件配置好系统识别的，iptables配置文件的位置为： `/etc/sysconfig/iptables`

![iptables-file-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/iptables-file.png)

要想看得懂这个文件，首先我们就要来看一下iptables的组成规则，iptables由 **四张表 + 五条链（hook point）+ 规则** 组成，释义如下：

![iptables-chain-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/iptables-chain.png)

看着这些规则可能还是不知道这个东西到底怎么配置，下面我们来讲一个场景：

我们之前启动了Apache服务器之后需要把iptables防火墙关闭才能正常访问，但是这种操作跟 **“删库跑路”** 都一个性质的，肯定不能被允许，那么怎么办呢？这个时候我们就要来修改我们的iptables规则，实际上外面访问我们的Apache服务器时是访问我们的80端口，但是从默认的规则里我们看到并没有允许80端口的访问，所以我们可以采用命令 `iptables -I INPUT -p tcp --dport 80 -j ACCEPT` 来插入一条iptables规则，让80端口能够被访问。当然，我们也可以直接修改iptables对应的配置文件：

![iptables-http-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/iptables-file-http.png)

> 其中 `-I` 表示在最前面插入，`-A` 表示在最后面追加，iptables读取是按配置顺序的。

**iptables** 规则可以说是非常强大，这里我们只做简单的端口拦截与放行使用介绍，后面用到的时候再做相关补充。

# 实践篇

## 一、FTP服务器（vsftpd）

### 1、关于vsfptd

vsftpd是用于UNIX系统（包括Linux）的GPL许可的FTP服务器。它安全、稳定且速度极快。vsftpd是一个成熟且值得信赖的文件传输解决方案。更多请查看： [vsftpd官网](https://security.appspot.com/vsftpd.html)

### 2、安装并启动FTP服务

使用 `yum` 安装 `vsftpd` ，执行 `yum install -y vsftpd` :

![yum-install-vsftpd-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/yum-install-vsftpd.png)

安装完成后，启动FTP服务，执行 `service vsftpd start` ：

![service-vsftpd-start-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/service-vsftpd-start.png)

成果启动之后，可以看到系统已经监听了21端口 `netstat -ntlp | grep 21` ：

![netstat-grep-21-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/netstat-grep-21.png)

此时，访问 `ftp://192.168.231.128` 就可以浏览机器上的 `/var/ftp` 目录了。

![ftp-anonymous-access-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/ftp-anonymous-access.png)

### 3、配置FTP权限

vsftpd的配置目录为 `/etc/vsftpd` ，包含下列配置文件：

* vsftpd.conf 为主要配置文件
* ftpusers 配置 **禁止** 访问FTP服务的用户列表
* user_list 配置用户访问控制

首先，我们需要阻止匿名访问和切换根目录。匿名访问和切换根目录都会给服务器带来安全风险，我们把这两个功能关闭，编辑 `/etc/vsftpd/vsftpd.conf` ，找到下面两处并修改：

```
# 禁用匿名用户
anonymous_enable=NO
# 禁止切换根目录
chroot_local_user=YES
```

![vsftpd-conf-anonymous-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/vsftpd-conf-anonymous.png)

![vsftpd-conf-chroot-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/vsftpd-conf-chroot.png)

然后，我们来创建一个专门用于访问FTP服务的用户，用户名叫 `ftpuser` ：

```
useradd ftpuser
```

为用户 `ftpuser` 设置密码：

```
echo "ftpuser" | passwd ftpuser --stdin
```

![useradd-passwd-ftpuser-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/useradd-passwd-ftpuser.png)

限制该用户仅能通过 FTP 访问服务器，而不能直接登陆服务器：

```
usermod -s /sbin/nologin ftpuser
```

为用户分配主目录为 `/var/ftp` ，该目录不可上传文件，文件只能上传到 `/var/ftp/pub` 目录下：

```
# 设置用户的主目录
usermod -d /var/ftp ftpuser
# 设置访问权限
chmod a-w /var/ftp && chmod -R 777 /var/ftp/pub
# 创建登陆欢迎文件
echo "Welcome to use my FTP service." > /var/ftp/index.html 
# 重启FTP服务
service vsftpd restart
```

### 4、访问 FTP 服务

FTP服务搭建完成之后我们可以使用浏览器来直接访问FTP服务：

![ftp-user-access-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/ftp-user-access.png)

或者，Mac上我推荐使用 [FileZilla](https://filezilla-project.org/) 这个FTP工具来连接FTP服务：

![filezilla-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/filezilla.png)

> 如果出现无法上传文件的错误，请关闭 [SELinux](https://wiki.centos.org/zh/HowTos/SELinux) 安全：
> 
> 1、临时关闭(不用重启)
> ```
> # 查看SELinux状态
> sestatus -v
> # 关闭
> setenforce 0
> ```
> 
> 2、修改配置文件（需要重启）：
> 
> ![SELINUX-disabled-c](https://raw.githubusercontent.com/lishuzhi1121/LinuxTutorial/master/images/SELINUX-disabled.png)
> 
> 所以常规做法是两个一起操作！

### 二、Web服务器（Tomcat）

