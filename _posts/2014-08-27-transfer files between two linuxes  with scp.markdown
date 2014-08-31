---
layout: post
title:  用scp在两台linux之间传输文件
date:   2014-08-27 23:56:00
categories: redhat
---
两台电脑都没有安装ftp客户端，所以使用了一下scp。
不过遇到了两台电脑ping不通的问题。
经过检查发现是因为无线网和虚拟机造成了多网卡，导致网段不同。
所以手动设置为同一网段，并且关闭无线网。
设置实例：
<pre><code>
inet（ip地址） 192.168.1.9
netmask（子网掩码） 255.255.255.0
broadcast（默认网关） 192.168.1.255
</code></pre>
从远程复制到本地:
<pre><code>
sudo scp -r root@192.168.1.6:/mnt/sda2/swift/ /mnt/sda6/swift/
-r 参数表示递归复制（即复制该目录下面的文件和目录），复制单文件可以省略
-P 如果需要设置端口
</code></pre>
