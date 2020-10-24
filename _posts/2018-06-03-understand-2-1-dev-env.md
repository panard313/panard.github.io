---
layout: article
title: 深入理解Android卷二 第一章 开发环境部署
key: 20180603
tags:
  - 深入理解 二
  - 环境
lang: zh-Hans
---

# 第1章 开发环境部署
本章主要内容：

简单介绍本书内容的架构、编译环境的搭建以及如何利用Eclipse调试SystemServer进程。

## 1.1  系统架构
到目前为止，Android系统的最新版本是4.0.3。而就在本书即将完稿之时，业界有传闻说Android 4.0.4版本已经对大厂商发布。Android系统推出速度之快让很多开发人员都很惊讶。当然，推出的速度如此之快有一些是出于商业上的考虑。不管怎样，Android系统在进化过程中，其大体架构相对保持稳定。图1-1为Android的系统架构图。


![图1-1  Android系统架构](/images/understand2/1-1.png)

相信绝大多数读者对图1-1或类似图表的内容已经司空见惯。此处，我们就不再赘述。

本书作为“深入理解Android”系列的第二卷，从内容上将承接《深入理解Android：卷I》（本书以后简称“卷I”）的内容，但是本书关注的焦点将从Native层Framework转移到Java层Framework。本书涵盖的内容如图1-2所示。



![图 1-2  本书涵盖的内容](/images/understand2/1-2.png)

从图1-2中，读者可发现本书的大部分内容都在讨论Service。确实如此。因为Android Java层Framework的核心就是这些Service。毫不夸张地说，正是这些Service支撑了整个Java层世界的运转。读者可参考第3章的图3-1来了解Framework中Service的概貌。

## 1.2  搭建开发环境
本节将讨论Android 4.0源码下载、Eclipse开发环境，以及如何调试SystemServer进程等相关知识。

### 1.2.1  下载源码
Android 2.3以后，Google官方推荐使用64位的操作系统来编译源码。所以，读者要先安装64位的OS。笔者推荐的操作系统是Ubuntu 10.04 X86-64版。另外，读者不要随意升级Ubuntu，因为高版本Ubuntu中自带的GCC版本过高，会导致编译Android源码时出问题。

提示其实32位的Ubuntu也可以编译Android 2.3以后的Android系统，但弄起来颇为麻烦，而且也没什么技术含量。笔者建议读者遵照官方要求去做即可。

笔者不打算过多讨论Android源码下载的步骤，原因是：这是一个需要读者动手操作的过程，而看着电脑屏幕操作比看书后输入大串的字符串的效率要高很多。

基于上面这个原因，这里笔者向读者提供一个官方说明文档，地址是http://source.android.com/source/downloading.html。该网页中有详细的代码下载步骤，读者只要执行简单的复制/粘贴操作即可动手实验。如图图1-3所示为该网页的截图。



图1-3  Android代码下载网页示意图

注意读者要选择下载Android 4.0.1的代码。虽然最新的Android 4.0.3版本从版本号上看变化不大，但实际代码却有较大变化。

另外，由于网络环境不佳，如果读者发现本章提供的下载地址无法连接，可从笔者博客上的链接去下载源代码和工具，博客地址是：http://blog.csdn.net/innost/article/details/7525205。

 

### 1.2.2  编译源码

#### **1. 部署JDK**
Android 2.3及以后版本的代码编译都需要使用JDK1.6，所以首先要做的是下载JDK1.6。下载网址是http://www.oracle.com/technetwork/java/javasebusiness/downloads/java-archive-downloads-javase6-419409.html。笔者下载的文件是jdk-6u27-linux-x64.bin。把它放到一个目录中，比如将其放到/mnt/hgfs/E目录下，然后在这个目录中执行：

./ jdk-6u27-linux-x64.bin  #执行这个文件

这个命令的作用其实就是解压。解压后的结果在/mnt/hgfs/E/jdk1.6.0_27目录中。有了JDK后，还需要设置~/.bashrc文件。在该文件末尾添加如图1-4所示的几行语句：



图1-4  Java环境部署示意图

重新登录系统后，Java环境就添加到系统中了。

## 2. 编译源码
源码编译的步骤非常简单。我们在卷I也详细介绍编译方法。不过本书要求读者必须先编译整个系统，步骤如下：

·  执行source build/envsetup.sh命令。该命令将导入Android编译环境。

·  输入choosecombo并执行，它是envsetup.sh中定义的一个函数。在执行过程中，分别选择release、generic、eng即可。最终屏幕输出如图1-5所示。  



图1-5  编译设置效果图

·  执行make命令以编译整个系统。编译时间由机器配置决定。笔者的4核4GB机器的编译时间大概在2小时左右。

### 1.2.3  利用Eclipse调试SystemServer
本节将介绍如何利用Eclipse来调试Android Java Framework的核心进程SystemServer。

#### 1.  配置Eclipse
首先要下载Android SDK。下载地址为http://developer.android.com/sdk/index.html。在Linux环境下，该网站截图如图1-6所示。



图1-6 AndroidSDK下载网页示意图

笔者下载的是Linux系统上的SDK。解压后的位置在/thunderst/disk/anroid/android-sdk-linux_x86下。

然后要为Eclipse安装ADT插件（Android Development Tools），步骤如下。

-  单击Eclipse菜单栏Help->Install New Software，输入Android ADT下载地址：https://dl-ssl.google.com/android/eclipse/，然后安装其中的所有组件，并重启Eclipse。

- 单击Eclipse菜单栏Preferences->Android一栏，在右边的SDK Location中输入刚才解压SDK后得到的目录，即笔者设置的/thunderst/disk/anroid/android-sdk-linux_x86，最终结果如图1-7所示。



图1-7  SDK安装示意图

·  单击Eclipse菜单栏Window->Android SDK Manager，弹出一个对话框，如图1-8所示。



图1-8  Android SDK Manager对话框

在图1-8中选择下载其中的Tools和对应版本的SDK文档、镜像文件、开发包等。有条件的读者可以将Android 4.0.3和4.0对应的内容及Tools全部下载过来。

#### 2.  使用Coffee Bytes Java插件
Coffee BytesJava是Eclipse的一个插件，用于对代码进行折叠，其功能比Eclipse自带的代码折叠功能强大多了。对于大段代码的研究，该插件的作用功不可没。此插件的安装和配置步骤如下：

-  单击Eclipse菜单栏Help->Install New Software，在弹出的对话框中输入http://eclipse.realjenius.com/update-site，选择安装这个插件即可。

-  单击Eclipse菜单栏Window->Preference，在左上角输入Folding进行搜索，结果如图1-9所示。



图1-9  Coffee Bytes Java 插件配置

在图1-9所示的对话框中，要先打开Enablefolding选项，然后从Select foldingto use框中选择CoffeeBytes Java Folding。图1-9右下部分的勾选框用来配置此插件的代码折叠功能。读者不妨按照图1-9所示来配置它。使用该插件后的示意图如图1-10所示。

如果不关心else if分支，就可以把这段代码折叠起来



图1-10  Coffee Bytes Java插件的使用示例

从图1-10中可看到，使用该插件后，基本上代码中所有分支都可以折叠起来，该功能将帮助开发人员集中精力关注自己所关心的分支。

#### 3.  导入Android源码
注意，这一步必须编译完整个Android源码才可以实施，步骤如下：

-  复制Android源码目录/development/ide/eclipse/.classpath到Android源码根目录。

-  打开Andriod源码根目录下的.classpath文件。该文件是供Eclipse使用的，其中保存的是源码目录中各个模块的路径。由于我们只关心Framework相关的模块，因此可以把一些不是Framework的目录从该文件中注释掉。同时，去掉不必要的模块也可加快Android源码导入速度。图1-11所示为该文件的部分内容。



图1-11.classpath文件部分文件内容

另外，一些不必要的模块会导致后续在Eclipse中Android源码编译失败。笔者共享了一个.classpath文件，读者可从http://download.csdn.net/detail/innost/4247578下载并直接使用。

单击Eclipse菜单栏New->Java Project，弹出如图1-12所示的对话框。设置Location为Android4.0源码所在路径。



图1-12  导入Android源码示意图

由于Android 4.0源码文件较多，导入过程会持续较长一段时间，大概10几分钟左右。

注意导入源码前一定要取消Eclipse的自动编译选项（通过菜单栏Project->BuildProject Automatically设置）。另外，源码导入完毕后，读者千万不要清理（clean）这个工程。清理会删除之前源码编译所生成的文件，导致后续又得重新编译Android系统了。

#### 4.  创建并运行模拟器
单击Eclipse菜单栏Window->AVD Manager，创建模拟器，如图1-13所示。



图1-13  模拟器创建示意图

模拟器创建完毕后即可启动它。

#### 5.  调试SystemServer
调试SystemServer的步骤如下：

-  首先编译Android源码工程。编译过程中会有很多警告。如果有错误，大部分原因是.classpath文件将不需要的模块包含了进来。读者可根据Eclipse的提示做相应处理。笔者配置的几台机器基本都是一次配置就成功了。

-  在Android源码工程上单击右键，依次单击Debug As->Debug Configurations，弹出如图1-14所示的对话框，然后从左边找到RemoteJava Application一栏。



图1-14  Debug配置框示意图

-  单击图1-14中黑框中的新建按钮，然后按图1-15中的黑框中的内容来设置该对话框。



图1-15  Remote Java Application配置示意图

由图1-15所示，需要选择Remote调试端口号为8600，Host类型为localhost。8600是SystemServer进程的调试端口号。Eclipse一旦连接到该端口，即可通过JDWP协议来调试SystemServer。

-  配置完毕后，单击图1-15右下角的Debug按钮，即可启动SystemServer的调试。

图1-16所示为笔者调试startActivity流程的示意图。



图1-16  SystemServer调试效果图

## 1.3  本章小结
本章对Android系统和源码搭建，以及如何利用Eclipse调试SystemServer等做了相关介绍，相信读者现在已经迫不及待了吧？马上开始我们的源码征程！
