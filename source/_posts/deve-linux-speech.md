title: Linux演讲
toc: true
date: 2015-06-02 12:39:22
tags: Linux
categories: 设计开发
---

## 前言
教授Linux的丁宋涛老师，安排大家轮流讲课，这不，6月4号就到我了，赶紧备课！
我们组要讲的内容有：
Linux第9题：
- 利用dumpe2fs把linux一个ext2(ext3也可)分区的信息dump出来，解释dump出来的信息。
- 结合课程的知识和ext2的目录结构，解析文件打开的过程。

课本第11章：
- 进程间通信。

本来想把这两部分内容都做一下，后来发现，内容实在太多，搞不定，还是大家均摊吧！题目我来，课本部分交给小伙伴们！PPT是我已经从百度文库Down好了，同志们，剩下的看你们的了！

## Linux第9题
### 第一小问
利用dumpe2fs把linux一个ext2(ext3也可)分区的信息dump出来，解释dump出来的信息。
<!--more-->
#### 命令
`mount`或者`df -lhT`，查看有哪些文件系统。
![mount](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/linux/mount.jpg)
![df](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/linux/df.jpg)
`dumpe2fs /dev/sda1 > dump.txt`，把分区信息写入dump.txt。
由于小编在使用虚拟机安装Ubuntu的时候，选择了让系统自动分区，所以，没有文件系统使用ext2或者ext3。最终导出的，是一个ext4系统的分区信息。不再更改分区了，分区命令也忘得差不多了。。。

#### dump.txt
```
Filesystem volume name:   <none>
Last mounted on:          /
Filesystem UUID:          56f9c6d7-9968-4c57-b2e5-c63d82738033
Filesystem magic number:  0xEF53
Filesystem revision #:    1 (dynamic)
Filesystem features:      has_journal ext_attr resize_inode dir_index filetype needs_recovery extent flex_bg sparse_super large_file huge_file uninit_bg dir_nlink extra_isize
Filesystem flags:         signed_directory_hash 
Default mount options:    user_xattr acl
Filesystem state:         clean
Errors behavior:          Continue
Filesystem OS type:       Linux
Inode count:              458752
Block count:              1834752
Reserved block count:     91737
Free blocks:              738375
Free inodes:              287057
First block:              0
Block size:               4096
Fragment size:            4096
Reserved GDT blocks:      447
Blocks per group:         32768
Fragments per group:      32768
Inodes per group:         8192
Inode blocks per group:   512
Flex block group size:    16
Filesystem created:       Tue Jun  2 13:05:17 2015
Last mount time:          Tue Jun  2 13:47:14 2015
Last write time:          Tue Jun  2 13:47:14 2015
Mount count:              3
Maximum mount count:      -1
Last checked:             Tue Jun  2 13:05:17 2015
Check interval:           0 (<none>)
Lifetime writes:          5791 MB
Reserved blocks uid:      0 (user root)
Reserved blocks gid:      0 (group root)
First inode:              11
Inode size:	          256
Required extra isize:     28
Desired extra isize:      28
Journal inode:            8
First orphan inode:       7969
Default directory hash:   half_md4
Directory Hash Seed:      04a70c20-f9ac-430b-9b6b-4179feaa85c6
Journal backup:           inode blocks
Journal features:         journal_incompat_revoke
Journal size:             128M
Journal length:           32768
Journal sequence:         0x00000a4f
Journal start:            1


Group 0: (Blocks 0-32767) [ITABLE_ZEROED]
  Checksum 0xdf8b, unused inodes 0
  Primary superblock at 0, Group descriptors at 1-1
  Reserved GDT blocks at 2-448
  Block bitmap at 449 (+449), Inode bitmap at 465 (+465)
  Inode table at 481-992 (+481)
  19210 free blocks, 56 free inodes, 1061 directories
  Free blocks: 9640-9649, 9652-9661, 10382, 10388, 10499, 10669, 10755, 10786, 10892-10893, 10899, 13400, 13572-13573, 13579, 13591-32767
  Free inodes: 7970-7975, 7977-8025, 8032
Group 1: (Blocks 32768-65535) [ITABLE_ZEROED]
  Checksum 0xd97d, unused inodes 0
  Backup superblock at 32768, Group descriptors at 32769-32769
  Reserved GDT blocks at 32770-33216
  Block bitmap at 450 (bg #0 + 450), Inode bitmap at 466 (bg #0 + 466)
  Inode table at 993-1504 (bg #0 + 993)
  0 free blocks, 15 free inodes, 961 directories
  Free blocks: 
  Free inodes: 9270, 9272, 9317, 11214, 11246, 15869, 15878, 15883, 15931, 15937, 16048, 16218, 16304, 16349-16350
Group 2: (Blocks 65536-98303) [ITABLE_ZEROED]
  Checksum 0x2380, unused inodes 0
  Block bitmap at 451 (bg #0 + 451), Inode bitmap at 467 (bg #0 + 467)
  Inode table at 1505-2016 (bg #0 + 1505)
  0 free blocks, 33 free inodes, 58 directories
  Free blocks: 
  Free inodes: 16416-16418, 16436-16441, 16627-16630, 16647-16651, 17142-17146, 17871-17872, 18189-18190, 18887, 18906, 18908, 19092-19094
......
......
......
Group 55: (Blocks 1802240-1834751) [INODE_UNINIT, ITABLE_ZEROED]
  Checksum 0x182b, unused inodes 8192
  Block bitmap at 1572871 (bg #48 + 7), Inode bitmap at 1572887 (bg #48 + 23)
  Inode table at 1576480-1576991 (bg #48 + 3616)
  32512 free blocks, 8192 free inodes, 0 directories, 8192 unused inodes
  Free blocks: 1802240-1834751
  Free inodes: 450561-458752
```

#### ext2文件系统分区信息
```
Filesystem volume name:   /                                 文件系统标签
Last mounted on:          <not available>
Filesystem UUID:          a5d248ed-850b-4392-929d-b86e2b4d65b8
Filesystem magic number:  0xEF53
Filesystem revision #:    1 (dynamic)
Filesystem features:      has_journal ext_attr resize_inode dir_index filetype needs_recovery sparse_super large_file        ext2文件系统可以包含的几种功能
Default mount options:    user_xattr acl
Filesystem state:         clean
Errors behavior.:          Continue
Filesystem OS type:       Linux
Inode count:              2097152                            文件系统中inode节点的数目
Block count:              2096474                            文件系统中块的数目
Reserved block count:     104823
Free blocks:              1910125
Free inodes:              2091098
First block:              0
Block size:               4096                                       块大小
Fragment size:            4096
Reserved GDT blocks:      511
Blocks per group:         32768
Fragments per group:      32768
Inodes per group:         32768
Inode blocks per group:   1024
Filesystem created:       Sat Nov 20 00:27:31 2010
Last mount time:          Sat Nov 20 23:53:31 2010
Last write time:          Sat Nov 20 23:53:31 2010
Mount count:              10
Maximum mount count:      -1
Last checked:             Sat Nov 20 00:27:31 2010
Check interval:           0 (<none>)
Reserved blocks uid:      0 (user root)
Reserved blocks gid:      0 (group root)
First inode:              11
Inode size:               128
Journal inode:            8
Default directory hash:   tea
Directory Hash Seed:      3302e23c-fc75-4a98-b9d1-9660c335988c
Journal backup:           inode blocks
Journal size:             128M

Group 0: (Blocks 0-32767)
  Primary superblock at 0, Group descriptors at 1-1
  Reserved GDT blocks at 2-512
  Block bitmap at 513 (+513), Inode bitmap at 514 (+514)
  Inode table at 515-1538 (+515)
  0 free blocks, 32757 free inodes, 2 directories
  Free blocks: 
  Free inodes: 12-32768
Group 1: (Blocks 32768-65535)
  Backup superblock at 32768, Group descriptors at 32769-32769
  Reserved GDT blocks at 32770-33280
  Block bitmap at 33281 (+513), Inode bitmap at 33282 (+514)
  Inode table at 33283-34306 (+515)
  26899 free blocks, 32592 free inodes, 44 directories
  Free blocks: 35887-55295, 58045, 58047-65535
  Free inodes: 32944, 32946-65536
Group 2: (Blocks 65536-98303)
  Block bitmap at 65536 (+0), Inode bitmap at 65537 (+1)
  Inode table at 65538-66561 (+2)
  31742 free blocks, 32768 free inodes, 0 directories
  Free blocks: 66562-98303
  Free inodes: 65537-98304
......
......
......
Group 63: (Blocks 2064384-2096473)
  Block bitmap at 2064384 (+0), Inode bitmap at 2064385 (+1)
  Inode table at 2064386-2065409 (+2)
  22114 free blocks, 32187 free inodes, 113 directories
  Free blocks: 2065411-2086911, 2095861-2096473
  Free inodes: 2064966-2097152
```

#### dump.txt详解
```
Filesystem volume name:   <none>	文件系统 volume 名称

Last mounted on:          /			上一次挂载文件系统的挂载点路径

Filesystem UUID:          56f9c6d7-9968-4c57-b2e5-c63d82738033	由乱数产生的识别码，可以用来识别文件系统。

Filesystem magic number:  0xEF53	用来识别此文档系统为 Ext2/Ext3/Ext4 的签名，位置在文档系统的 0x0438 - 0x0439 (Superblock 的 0x38-0x39)，必定是 0xEF53。

Filesystem revision #:    1 (dynamic)	文件系统版本编号

Filesystem features:      has_journal ext_attr resize_inode dir_index filetype needs_recovery extent flex_bg sparse_super large_file huge_file uninit_bg dir_nlink extra_isize
开启了的文件系统的功能
has_journal - 有日志 (journal)，亦代表此文件系统必为 Ext3 或 Ext4
ext_attr - 支持 extended attribute
resize_inode - resize2fs 可以改变文件系统大小
dir_index - 支持目录索引，可以加快在大目录中搜索文件。
filetype - 目录项目为否记录文件类型
needs_recovery - e2fsck 检查 Ext3/Ext4 文件系统时用来决定是否需要完成日志记录中未完成的工作，快速自动修复文件系统
extent - 支持 Ext4 extent 功能，可以加快文件系统效能和减少 external fragmentation
flex_bg
sparse_super - 只有少数 superblock 备份，而不是每个区块组都有 superblock 备份，节省空间。
large_file - 支持大于 2G 的档案
huge_file
uninit_bg
dir_nlink
extra_isize

Filesystem flags:         signed_directory_hash 	文件系统旗号

Default mount options:    user_xattr acl	默认挂载选项

Filesystem state:         clean		文件系统状态，可以为 clean(文件系统已成功地被卸载)、not-clean(表示文件系统挂载成读写模式后，仍未被卸载)或 erroneous (文件系统被发现有问题)

Errors behavior:          Continue		文件系统发生问题时的处理方案，可以为 continue (继续正常运作) 、remount-ro (重新挂载成只读模式) 或 panic (即时当掉系统)。可以使用 tune2fs -e 改变。

Filesystem OS type:       Linux		建立文件系统的作业系统，可以为 Linux/Hurd/MASIX/FreeBSD/Lites

Inode count:              458752	文件系统的总inode 数目，亦是整个文件系统所可能拥有文件数目的上限

Block count:              1834752	文件系统的总区块数目

Reserved block count:     91737		保留给系统管理员工作之用的区块数目

Free blocks:              738375	未使用区块数目

Free inodes:              287057	未使用 inode 数目

First block:              0			Superblock 或第一个区块组开始的区块编数。此值在 1 KiB 区块大小的文件系统为 1，大于1 KiB 区块大小的文件系统为 0。(Superblock/第一个区块组一般都在文件系统 0x0400 (1024) 开始)

Block size:               4096		区块大小，可以为 1024, 2048 或 4096 字节 (Compaq Alpha 系统可以使用 8192 字节的区块)

Fragment size:            4096		Fragment大小，实际上Ext2/Ext3/Ext4并不支持Fragment，所以此值一般和区块大小一样

Reserved GDT blocks:      447	保留GDT区块数目，保留作在线改变文件系统大小的区块数目。若此值为0，只可以先卸载才可脱机改变文件系统大小

Blocks per group:         32768		每个区块组的区块数目

Fragments per group:      32768		每个区块组的片段数目，亦用来计算每个区块组中 block bitmap 的大小

Inodes per group:         8192		每个区块组的inode数目

Inode blocks per group:   512		每个区块组的inode区块数目

Flex block group size:    16		

Filesystem created:       Tue Jun  2 13:05:17 2015	文件系统建立时间

Last mount time:          Tue Jun  2 13:47:14 2015	上一次挂载此文件系统的时间

Last write time:          Tue Jun  2 13:47:14 2015	上一次改变此文件系统内容的时间

Mount count:              3		挂载次数。距上一次作完整文件系统检查后文件系统被挂载的次数，让fsck决定是否应进行另一次完整文件系统检查

Maximum mount count:      -1	最大挂载次数。文件系统进行另一次完整检查可以被挂载的次数，若挂载次数(Mount count)大于此值，fsck 会进行另一次完整文件系统检查

Last checked:             Tue Jun  2 13:05:17 2015	上一次文件系统作完整检查的时间

Check interval:           0 (<none>)	文件系统应该进行另一次完整检查的最大时间距

Lifetime writes:          5791 MB

Reserved blocks uid:      0 (user root)		保留区块使用者识别码 
Reserved blocks gid:      0 (group root)	保留区块群组识别码
First inode:              11		第一个可以用作存放正常文件属性的inode编号，在原格式此值一定为11，V2格式可以改变此值

Inode size:	          256	Inode 大小，传统为 128 字节，新系统会使用 256 字节的 inode 令扩充功能更方便

Required extra isize:     28	必须的额外isize
Desired extra isize:      28	请求的额外isize

Journal inode:            8		日志文件的 inode 编号

First orphan inode:       7969

Default directory hash:   half_md4		缺省目录 hash 算法

Directory Hash Seed:      04a70c20-f9ac-430b-9b6b-4179feaa85c6		目录 hash 种子

Journal backup:           inode blocks		日志备份

Journal features:         journal_incompat_revoke	日志特点

Journal size:             128M		日志文件的大小

Journal length:           32768		日志长度
Journal sequence:         0x00000a4f	日志序列
Journal start:            1		日志开始位置


Group 0: (Blocks 0-32767) [ITABLE_ZEROED]
  Checksum 0xdf8b, unused inodes 0
  Primary superblock at 0, Group descriptors at 1-1
  Reserved GDT blocks at 2-448
  Block bitmap at 449 (+449), Inode bitmap at 465 (+465)
  Inode table at 481-992 (+481)
  19210 free blocks, 56 free inodes, 1061 directories
  Free blocks: 9640-9649, 9652-9661, 10382, 10388, 10499, 10669, 10755, 10786, 10892-10893, 10899, 13400, 13572-13573, 13579, 13591-32767
  Free inodes: 7970-7975, 7977-8025, 8032
```

### 第二小问
结合课程的知识和ext2的目录结构，解析文件打开的过程。

#### ext2文件系统
![ext2文件系统示意图](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/linux/ext2文件系统.png)

文件系统中存储的最小单位是块（Block），一个块究竟多大是在格式化时确定的，例如mke2fs的-b选项可以设定块大小为1024、2048或4096字节。而上图中启动块（Boot Block）的大小是确定的，就是1KB，启动块是由PC标准规定的，用来存储磁盘分区信息和启动信息，任何文件系统都不能使用启动块。启动块之后才是ext2文件系统的开始，ext2文件系统将整个分区划成若干个同样大小的块组（Block Group），每个块组都由以下部分组成。

超级块（Super Block）
描述整个分区的文件系统信息，例如块大小、文件系统版本号、上次mount的时间等等。超级块在每个块组的开头都有一份拷贝。

块组描述符表（GDT，Group Descriptor Table）
由很多块组描述符组成，整个分区分成多少个块组就对应有多少个块组描述符。每个块组描述符（Group Descriptor）存储一个块组的描述信息，例如在这个块组中从哪里开始是inode表，从哪里开始是数据块，空闲的inode和数据块还有多少个等等。和超级块类似，块组描述符表在每个块组的开头也都有一份拷贝，这些信息是非常重要的，一旦超级块意外损坏就会丢失整个分区的数据，一旦块组描述符意外损坏就会丢失整个块组的数据，因此它们都有多份拷贝。通常内核只用到第0个块组中的拷贝，当执行e2fsck检查文件系统一致性时，第0个块组中的超级块和块组描述符表就会拷贝到其它块组，这样当第0个块组的开头意外损坏时就可以用其它拷贝来恢复，从而减少损失。

块位图（Block Bitmap）
一个块组中的块是这样利用的：数据块（Data Block）存储所有文件的数据，比如某个分区的块大小是1024字节，某个文件是2049字节，那么就需要三个数据块来存，即使第三个块只存了一个字节也需要占用一个整块；超级块、块组描述符表、块位图、inode位图、inode表这几部分存储该块组的描述信息。那么如何知道哪些块已经用来存储文件数据或其它描述信息，哪些块仍然空闲可用呢？块位图就是用来描述整个块组中哪些块已用哪些块空闲的，它本身占一个块，其中的每个bit代表本块组中的一个块，这个bit为1表示该块已用，这个bit为0表示该块空闲可用。

为什么用df命令统计整个磁盘的已用空间非常快呢？因为只需要查看每个块组的块位图即可，而不需要搜遍整个分区。相反，用du命令查看一个较大目录的已用空间就非常慢，因为不可避免地要搜遍整个目录的所有文件。

与此相联系的另一个问题是：在格式化一个分区时究竟会划出多少个块组呢？主要的限制在于块位图本身必须只占一个块。用mke2fs格式化时默认块大小是1024字节，可以用-b参数指定块大小，现在设块大小指定为b字节，那么一个块可以有8b个bit，这样大小的一个块位图就可以表示8b个块的占用情况，因此一个块组最多可以有8b个块，如果整个分区有s个块，那么就可以有s/(8b)个块组。格式化时可以用-g参数指定一个块组有多少个块，但是通常不需要手动指定，mke2fs工具会计算出最优的数值。

inode位图（inode Bitmap）
和块位图类似，本身占一个块，其中每个bit表示一个inode是否空闲可用。

inode表（inode Table）
我们知道，一个文件除了数据需要存储之外，一些描述信息也需要存储，例如文件类型（常规、目录、符号链接等），权限，文件大小，创建/修改/访问时间等，也就是ls -l命令看到的那些信息，这些信息存在inode中而不是数据块中。每个文件都有一个inode，一个块组中的所有inode组成了inode表。

inode表占多少个块在格式化时就要决定并写入块组描述符中，mke2fs格式化工具的默认策略是一个块组有多少个8KB就分配多少个inode。由于数据块占了整个块组的绝大部分，也可以近似认为数据块有多少个8KB就分配多少个inode，换句话说，如果平均每个文件的大小是8KB，当分区存满的时候inode表会得到比较充分的利用，数据块也不浪费。如果这个分区存的都是很大的文件（比如电影），则数据块用完的时候inode会有一些浪费，如果这个分区存的都是很小的文件（比如源代码），则有可能数据块还没用完inode就已经用完了，数据块可能有很大的浪费。如果用户在格式化时能够对这个分区以后要存储的文件大小做一个预测，也可以用mke2fs的-i参数手动指定每多少个字节分配一个inode。

数据块
根据不同的文件类型有以下几种情况

对于常规文件，文件的数据存储在数据块中。

- 对于目录，该目录下的所有文件名和目录名存储在数据块中，注意文件名保存在它所在目录的数据块中，除文件名之外，ls -l命令看到的其它信息都保存在该文件的inode中。注意这个概念：目录也是一种文件，是一种特殊类型的文件。

- 对于符号链接，如果目标路径名较短则直接保存在inode中以便更快地查找，如果目标路径名较长则分配一个数据块来保存。

- 设备文件、FIFO和socket等特殊文件没有数据块，设备文件的主设备号和次设备号保存在inode中。

#### inode table
inode记录的文件数据至少有：
1、该文件的访问模式；（rwx）
2、该文件的所有者与组（ower/group）；
3、该文件的大小；
4、该文件创建或状态改变的时间（ctime）；
5、最近一次读的时间（atime）；
6、最近修改的时间（mtime）；
7、该文件的特性的标志（flag）；
8、该文件真正内容的指向（pointer）；

而有这么强大功能的inode的大小均固定为每个128B。inode除了文件权限属性记录区域外，还有12个直接，1个间接，一个双间接与一个三间接记录区。12个直接指向号码的对照，这12个记录就能够直接取得block号码，至于所谓的间接就是再拿一个block来当作block号码的记录区，如果文件太大，就会使用间接的block来记录编号。同理，如果文件持续长大，那么就复用所谓的双间接，第一个仅再指出下一个记录编号的block在哪里，实际记录在第二个block当中。依此类推，三间接就是复用第三层block来记录编号。如下图所示：

![数据块的寻址](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/linux/数据块的寻址.png
)

#### 打开/opt/file
打开/opt/file，查找的顺序是：

1、读出inode表中第2项，也就是根目录的inode，从中找出根目录数据块的位置

2、从根目录的数据块中找出文件名为opt的记录，从记录中读出它的inode号

3、读出opt目录的inode，从中找出它的数据块的位置

4、从opt目录的数据块中找出文件名为file的记录，从记录中读出它的inode号

5、读出file文件的inode，找到它的数据块的位置

6、读出file文件的内容

#### ls、cp、mv等命令实现
不会。

## 课件分享
进程间通信PPT：http://yunpan.cn/cQTqJ2t3fPeCF  访问密码 a5c9

## 参考文档
《UNIX/Linux程序设计教程》，赵克佳 沈志宇 编著，机械工业出版社

---
linux中使用dumpe2fs查看EXT2(或EXT3)文件系统信息
http://blog.itpub.net/8183550/viewspace-678589/

---
初窥Linux 之 ext2/ext3文件系统
http://blog.csdn.net/ljianhui/article/details/8604140

---
Linux进程间通信
http://www.cnblogs.com/linshui91/archive/2010/09/29/1838770.html
http://blog.csdn.net/chen_shiyang/article/details/8011887

---
Linux下的进程间通信-详解
http://www.cnblogs.com/skyofbitbit/p/3651750.html

---
ext4 笔记二（dumpe2fs）
http://blog.csdn.net/lishuanglin131/article/details/8258447

---
ext2文件系统
http://docs.linuxtone.org/ebooks/C&CPP/c/ch29s02.html