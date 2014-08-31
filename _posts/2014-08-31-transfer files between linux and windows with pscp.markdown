---
layout: post
title:  用pscp在linux和windows之间传输文件
categories: redhat
---
突然需要在redhat和网win7之间传些文件，查了一下发现PuTTY提供了很多途径，
因为之前使用过scp，所以决定选择pscp作为传输工具。</br>
找到PuTTY的下载页面：</br>
[http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html "pscp")</br>
选择下载pscp.exe，放到了win7 E盘的bank文件夹下。</br>
cmd进入到bank下，接下来就和scp一样使用了，命令如下：
<pre><code>
E:\bank>pscp -r E:\BaiduYunDownload\AIX_7.1_TL_7100-00-01 root@192.168.1.6:/mnt/
sda2
root@192.168.1.6's password:
AIX_7.1_Base_Operating_Sy | 3282272 kB | 19083.0 kB/s | ETA: 00:00:00 | 100%
AIX_7.1_Base_Operating_Sy | 2368352 kB | 19254.9 kB/s | ETA: 00:00:00 | 100%
AIX_Enterprise_Edition_V7 | 538613 kB | 19236.2 kB/s | ETA: 00:00:00 | 100%
AIX_Express_Standard_Edit | 1844070 kB | 19011.0 kB/s | ETA: 00:00:00 | 100%
AIX_Profile_Manager.tar.g | 38326 kB | 19163.0 kB/s | ETA: 00:00:00 | 100%
AIX_Toolbox_for_Linux_App | 582963 kB | 18805.3 kB/s | ETA: 00:00:00 | 100%
IBM AIX Version 7.1 Diffe | 2917 kB | 2917.4 kB/s | ETA: 00:00:00 | 100%
</code></pre>
