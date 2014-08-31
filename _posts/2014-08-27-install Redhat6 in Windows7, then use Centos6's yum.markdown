---
layout: post
title:  Windows7下硬盘安装Redhat6,换用Centos6的yum
date:   2014-08-27 23:00:00
categories: redhat
---
之前一直用的是Fedora，最近因为项目需要用到Linux服务器，就准备尝试一下redhat的破解版。不过5.3的版本报出了硬件不支持，就改用了6.4，因为没有序列号，所以采用了Centos 的 yum。并且遇到了ntfs分区不识别的问题。
#安装
1.使用grub启动
下载grub4dos，解压后将grldr、grldr.mbr、menu.lst放到Win7系统根目录。
CMD运行bcdedit /create /d "Grub4DOS" /application bootsector生成
{03da2fa6-4785-11e3-b4f2-a4daf9fa83fa}，详细命令：
<pre><code>
C:\Users\Administrator>bcdedit /create /d "Grub4DOS" /application bootsector
项 {03da2fa6-4785-11e3-b4f2-a4daf9fa83fa} 成功创建。

C:\Users\Administrator>bcdedit /set {03da2fa6-4785-11e3-b4f2-a4daf9fa83fa} devic
e partition=C:
操作成功完成。

C:\Users\Administrator>bcdedit /set {03da2fa6-4785-11e3-b4f2-a4daf9fa83fa} path
\grldr.mbr
操作成功完成。

C:\Users\Administrator>bcdedit /displayorder {03da2fa6-4785-11e3-b4f2-a4daf9fa83
fa} /add
</code></pre>
grub启动redhat安装命令：
<pre><code>
root (hd0,6)
kernel (hd0,6)/isolinux/vmlinuz
initrd (hd0,6)/isolinux/initrd.img
boot
</code></pre>
出问题时grub进入windows命令：
<pre><code>
方法1：
rootnoverify (hd0,0)
chainloader +1
方法2：
acpi
fallback 1
root (hd0,0)
chainloader /bootmgr
</code></pre>
2.在分区中压缩出约10G空间，格式化为FAT32，将镜像放进去，把isolinux文件夹解压出来，再压缩出/和swap的分区的空间
3.安装分区时选择Custom，准备好的空间分别格式化为ext4挂载/，格式化为swap挂载swap

#yum
centos软件源：[http://centos.ustc.edu.cn/centos](http://centos.ustc.edu.cn/centos "centos")</br>
163软件源：[http://mirrors.163.com/centos](http://mirrors.163.com/centos "163")<br/>
下面采用163的源，64位系统：
1.下载软件包：
<pre><code>
wget http://mirrors.163.com/centos/6/os/x86_64/Packages/python-iniparse-0.3.1-2.1.el6.noarch.rpm
wget http://mirrors.163.com/centos/6/os/x86_64/Packages/yum-metadata-parser-1.1.2-16.el6.x86_64.rpm
wget http://mirrors.163.com/centos/6/os/x86_64/Packages/yum-plugin-fastestmirror-1.1.30-14.el6.noarch.rpm
</code></pre>
2.卸载RedHat yum：
<pre><code>
rpm -qa | grep yum | xargs rpm -e --nodeps
</code></pre>
3.安装centos yum：
<pre><code>
rpm -ivh python-iniparse-0.3.1-2.1.el6.noarch.rpm
rpm -ivh yum-metadata-parser-1.1.2-16.el6.x86_64.rpm
rpm -ivh yum-3.2.29-40.el6.centos.noarch.rpm yum-plugin-fastestmirror-1.1.30-14.el6.noarch.rpm
</code></pre>
4.下载163repo：
<pre><code>
wget http://mirrors.163.com/.help/CentOS6-Base-163.repo
</code></pre>
5.修改repo的$releasever为centos的版本号，即6
6.移动repo到/etc/yum.repo.d
7.yum clean all
8.yum update 开始更新了
</code></pre>
#识别ntfs格式分区
1.NTFS-3G下载：[http://www.tuxera.com/community/ntfs-3g-download/](http://www.tuxera.com/community/ntfs-3g-download/ "NTFS-3G")<br/>
2.用tar -zxvf解压开tgz文件<br />
3.yum install gcc安装gcc，再用./configure检查一下<br />
4.make<br />
5.make install<br />
6.mkdir /mnt/sda1 建立挂载文件夹，用mount -t ntfs-3g /dev/sda1 /mnt/sda1就可以访问了<br />
7.实现开机自动挂载，编辑/etc/fstab添加/dev/sda1 /mnt/sda1 ntfs-3g defaults 0 0
