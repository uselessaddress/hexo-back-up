title: 编译binutils报错
date: 2013-11-18 12:22:19
tags: 
- Linux
- binutils
- 错误
categories: 点滴发现
---

生成configure后，然后make，会报错如下：

make[3]: Leaving directory `/opt/mylinux/build/binutils/bfd/po' 
make[3]: Entering directory `/opt/mylinux/build/binutils/bfd/po' 
make[3]: Nothing to be done for `info'. 
make[3]: Leaving directory `/opt/mylinux/build/binutils/bfd/po' 
make[3]: Entering directory `/opt/mylinux/build/binutils/bfd' 
make[3]: Nothing to be done for `info-am'. 
make[3]: Leaving directory `/opt/mylinux/build/binutils/bfd' 
make[2]: *** [info-recursive] Error 1 
make[2]: Leaving directory `/opt/mylinux/build/binutils/bfd' 
make[1]: *** [all-bfd] Error 2 
make[1]: Leaving directory `/opt/mylinux/build/binutils' 
make: *** [all] Error 2 

试了网上各种方法无效，我了个去，这还怎么继续玩？都折腾了三天了都！
安装texinfo？安装了，无效。
在configure所在文件夹下编译？试过了，同样报错。
增加configure的参数？无效！
换一个binutils版本？试了三个版本，报错相同！

终极解决办法：见Binutils-2.23.2编译（官方文档 ）
http://demo.voidking.com/reprint/linux/Binutils-2.23.2编译（官方文档）.html
