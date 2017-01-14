title: Linux中目录的粘贴位（t位）
date: 2013-10-19 16:55:34
tags: 
- Linux
- 粘贴位
categories: 点滴发现
---

directory permission(目录权限)

  same bits, but different semantics from those of files

    r: can list directory contents

    w: can add or remove files from a directory

    x: can enter a directory

  especially, when the "w" bit is set, anyone can remove anyone's files, how can we prevent this?

    the "t" bit (sticky bit)

    everyone can only remove herself's files

    e.g., run:

       $ ls -l /

    and then check the directory "/tmp"

      what's it's permmission bits? why?

----------------------------------------------------

关于linux下粘贴位(sticky位).
要删除一个文件，你不一定要有这个文件的写权限，但你一定要有这个文件的上级目录的写权限。也就是说，你即使没有一个文件的写权限，但你有这个文件的上级目录的写权限，你也可以把这个文件给删除，而如果没有一个目录的写权限，也就不能在这个目录下创建文件。
如何才能使一个目录既可以让任何用户写入文件，又不让用户删除这个目录下他人的文件，sticky就是能起到这个作用。stciky一般只用在目录上，用在文件上起不到什么作用。
在一个目录上设了sticky位后，（如/home，权限为1777)所有的用户都可以在这个目录下创建文件，但只能删除自己创建的文件(root除外)，这就对所有用户能写的目录下的用户文件启到了保护的作用。
<!--more-->
 

如果用户对目录有写权限，则可以删除其中的文件和子目录，即使该用户不是这些文件的所有者，而且也没有读或写许可。粘着位出现执行许可的位置上，用t表示，设置了该位后，其它用户就不可以删除不属于他的文件和目录。但是该目录下的目录不继承该权限，要再设置才可使用。

普通文件的sticky位会被linux内核忽略。

目录的sticky位表示这个目录里的文件只能被owner和root删除 。

/tmp常被我们用来存放临时文件，是所有用户。但是我们不希望别的用户随随便便的就删除了自己的文件，于是便有了粘连位，它的作用便是让用户只能删除属于自己的文件。

 $ ls -dl /tmp  
drwxrwxrwt 15 root   root  .........  
那么原来的执行标志x到哪里去了呢? 系统是这样规定的, 假如本来在该位上有x, 则这些特别标志 (suid, sgid, sticky) 显示为小写字母 (s, s, t). 否则, 显示为大写字母 (S, S, T) 。

另外：chmod 777 abc  

 　　  chmod +t abc   

等价于  

　　   chmod 1777 abc  