---
layout: post
title:  redhat环境下，编译安装内核
categories: redhat
---
redhat6自带的内核是2.6.32-358，即使用yum升级后还是为2.6.32-431，无法使用最新的ibus等软件，所以决定手动升级一下内核。</br>
首先下载了3.10.6的内核，linux-3.10.6.tar.gz
[https://www.kernel.org/pub/linux/kernel/v3.x/](https://www.kernel.org/pub/linux/kernel/v3.x/ "kernel")</br>
<pre><code>
tar -zxf linux-3.10.6.tar.gz -C /usr/src/ 
cd /usr/src/linux-3.10.6/
make mrproper //去除内核的依赖关系以及编译后的垃圾信息
make menuconfig //进入菜单方式界面配置内核
</code></pre>
期间发现缺少ncurses-devel，所以yum install ncurses-devel。</br>
之后Linux Lernel Configuration图形界面时，“enable deprecated sysfs features to support old userspace tools”和子项全部选中，不然编译安装之后会找不到原来的挂载点。</br>
配置完后执行命令：
<pre><code>
make;make modules;make modules_install;make install 
//分别是编译内核、编译模块、安装模块、安装内核
</code></pre>
重启就会出现3.10.6的开机选项，第一次进入时会比较慢，之后就正常了。</br>
ref：[http://www.liusuping.com//ubuntu-linux/redhat-linux-kernel-update.html](http://www.liusuping.com//ubuntu-linux/redhat-linux-kernel-update.html "ref")

