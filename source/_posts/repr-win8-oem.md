title: Win8出现OEM分区
date: 2013-11-26 18:56:39
tags: 
- win8
- OEM
categories: 精华转载
---
**预装win8的系统**，在恢复系统出厂设置之后，和C盘相邻的硬盘分区会变成**OEM分区**，无法打开此分区也无法删除。目前还不是很清楚具体的情况，以下是临时解决方法，有碰见的可以尝试手动删除此OEM分区，然后重新新建分区。
具体相关信息如下：
![此处输入图片的描述][1]
<!--more-->
 1. 输入`LIST DISK`，显示磁盘列表，如果连有两个磁盘，会显示出 0和1，其中主盘是0，从盘是1
 2. 输入`SELECT DISK 0`，选择第一块硬盘
![此处输入图片的描述][2]
![此处输入图片的描述][3]
![此处输入图片的描述][4]


  [1]: http://ihelloworld.qiniudn.com/@/imgs/oem1.jpg
  [2]: http://ihelloworld.qiniudn.com/@/imgs/oem2.jpg
  [3]: http://ihelloworld.qiniudn.com/@/imgs/oem3.jpg
  [4]: http://ihelloworld.qiniudn.com/@/imgs/oem4.jpg