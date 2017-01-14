title: Linux下tar.xz结尾的文件的解压方法
date: 2013-10-18 18:53:33
tags: 
- Linux
categories: 点滴发现
---

今天尝试编译内核，下载到了一份tar.xz结尾的压缩文件，网上解决方法比较少，不过还是找到了，如下：

$xz -d ***.tar.xz

$tar -xvf  ***.tar

可以看到这个压缩包也是两层压缩，外面是xz压缩方式，里层是tar压缩方式。
