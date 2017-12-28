---
layout: post
title: Windows 7 下 QT 5.1.1 for Android 开发环境的搭建与配置
date: 2013-11-08 10:00:00 -05:00
categories: Environment
tag: Qt
---

Qt 是诺基亚开发的一个跨平台的 C++ 图形用户界面应用程序框架，对于一些想要开发 Android 的应用但是又不想学习 Java 的开发人员而言，Qt 是一个很好选择。

本次使用的操作系统为 Windows 7 64 位，用的是 32 位的安装包，32 位系统没有验证过。  

---

# 一、下载安装包

首先下载以下安装包，如果提供的链接失效请自行下载：

（1）Android SDK （Windows 32-bit ADT版）：

【直接下载】[http://dl.google.com/android/adt/adt-bundle-windows-x86-20131030.zip](http://dl.google.com/android/adt/adt-bundle-windows-x86-20131030.zip)

（2）Android NDK（Windows 32-bit）：

【直接下载】[http://dl.google.com/android/ndk/android-ndk-r9b-windows-x86.zip](http://dl.google.com/android/ndk/android-ndk-r9b-windows-x86.zip)

（3）Java JDK（Windows 32-bit）：

【手动下载】[http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)

（4）Apache-Ant：

【直接下载】[http://mirrors.cnnic.cn/apache//ant/binaries/apache-ant-1.9.2-bin.zip](http://mirrors.cnnic.cn/apache//ant/binaries/apache-ant-1.9.2-bin.zip)

（5）QT 5.1.1 for Android （Windows 32-bit  离线版）：

【直接下载】[http://mirrors.hustunique.com/qt/official_releases/qt/5.1/5.1.1/qt-windows-opensource-5.1.1-android-x86-win32-offline.exe](http://mirrors.hustunique.com/qt/official_releases/qt/5.1/5.1.1/qt-windows-opensource-5.1.1-android-x86-win32-offline.exe)  

# 二、安装包的解压与安装

接下来解压、安装下载好的各安装包：

（1）Android SDK：【解压】解压到  D:\ADT 目录下  
（2）Android NDK：【解压】解压到 D:\NDK 目录下  
（3）Java JDK（Windows 35-bit）：【安装】安装过程中有两次要选择安装路径，注意请根据自己安装的版本自行修改，后面设置环境变量需要用到，这里我第一次填写：

```
D:\Java\jdk1.7.0_45  
```

第二次填写：

```
D:\Java\jre7  
```

（4）Apache-Ant：【解压】解压到 D:\ANT 目录下  
（5）QT 5.1.1 for Android（Windows 35-bit 离线版）：【安装】安装到 D:\QT 目录下  

# 三、设置环境变量

根据第二步中的相关路径，设置系统环境变量：  

## 1，添加新的环境变量

右键单击 我的电脑 -> 属性 -> 高级系统设置 -> 环境变量，在系统变量中新建以下变量：

（1）变量名：JAVA_HOME，变量值：

```
D:\Java\jdk1.7.0_45  
```

（2）变量名：CLASSPATH，变量值：

```
.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar;  
```

【注意最前面的点号 . 和最后面的分号 ; 不能漏掉】  
（3）变量名：ANDROID_SDK_HOME，变量值：

```
D:\ADT\sdk\  
```

（4）变量名：ANT_HOME，变量值：

```
D:\ANT  
```

## 2，修改系统变量

在系统变量里找到变量 Path ，选择”编辑“，在最后面添加：

```
%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin;%ANDROID_SDK_HOME%;  
```

【注意最后面的分号 ; 不能漏掉】  

<center>
![Windows7-QT-Android-1](/images/Windows7-QT-Android-1.jpeg)
</center>

# 四、Qt Creator 设置

打开Qt Creator，单击 工具 -> 选项，出现选项界面后选择 Android，分别做如下设置：

（1）Android-SDK的路径：

```
D:\ADT\sdk  
```

（2）Android NDK的路径：

```
D:\NDK  
```

（3）ANT的路径： 

```
D:\ANT\bin\ant.bat  
```

（4）JDK location：

```
D:\Java\jdk1.7.0_45  
```

<center>
![Windows7-QT-Android-2](/images/Windows7-QT-Android-2.jpeg)
</center>

# 五、添加虚拟机

单击 启动Android AVD管理器，出现Android Virtual Device Manager界面，单击 New 创建一个Android虚拟设备。

<center>
![Windows7-QT-Android-3](/images/Windows7-QT-Android-3.jpeg)
</center>

# 六、建立测试工程

经过以上这些步骤，开发环境基本配置完成，接下来我们建立一个简单的工程来验证配置是否正确：

（1）重新打开Qt Creator，选择 文件 -> 新建文件或项目，出现项目创建向导，选择 QT Gui 应用：  

<center>
![Windows7-QT-Android-4](/images/Windows7-QT-Android-4.jpeg)
</center>

（2）然后下一步，工程路径任选。  
【但是切记，绝对不要在路径内包含任何空格，这里我使用的是D:\QT-WorkSpace，否则会出现各种意想不到的编译错误！】  
（3）然后下一步，选择 Android for arm：  

<center>
![Windows7-QT-Android-5](/images/Windows7-QT-Android-5.jpeg)
</center>

（4）后面的信息暂时不需要过多关注，直接下一步即可，直至完成项目创建。  

<center>
![Windows7-QT-Android-6](/images/Windows7-QT-Android-6.jpeg)
</center>

（5）项目创建完毕后，右键 项目，选择 构建，若成功则继续下一步，否则请对照上文寻找可能的出错步骤进行相应修改或返回本文开头尝试重新开始配置过程。  

<center>
![Windows7-QT-Android-7](/images/Windows7-QT-Android-7.jpeg)
</center>

（6）项目构建成功后，右键  
项目，选择 运行，Android虚拟设备将会自动打开，启动过程过程较慢，耐心等候。  
（7）若无意外，将会成功运行该空项目生成的apk，因为这里是个空的项目，什么也没写，所以当然什么也没有，效果如图，表明环境配置成功。  

<center>
![Windows7-QT-Android-8](/images/Windows7-QT-Android-8.jpeg)
</center>

---

到这里就应该已经完成了，接下来可以使用 C++ 动手开始 QT for Android 开发了，😝。

---

本文链接：[http://www.eyrefree.org/2013/11/08/2013-11-08-Windows7-QT-Android/](http://www.eyrefree.org/2013/11/08/2013-11-08-Windows7-QT-Android/)

如文中无特殊说明，本站均使用以下协议保护：[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/3.0/cn/)
