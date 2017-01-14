title: 编译gcc报错
date: 2013-11-18 12:47:43
tags: 
- Linux
- gcc
- mpc
categories: 点滴发现
---

在configure时报错如下：
……
checking for the correct version of gmp.h... yes 
checking for the correct version of mpfr.h... yes 
checking for the correct version of mpc.h... no 
configure: error: Building GCC requires GMP 4.2+, MPFR 2.3.1+ and MPC 0.8.0+. 
Try the --with-gmp, --with-mpfr and/or --with-mpc options to specify 
their locations.  Source code for these libraries can be found at 
their respective hosting sites as well as at 
ftp://gcc.gnu.org/pub/gcc/infrastructure/.  See also 
http://gcc.gnu.org/install/prerequisites.html for additional info.  If 
you obtained GMP, MPFR and/or MPC from a vendor distribution package, 
make sure that you have installed both the libraries and the header 
files.  They may be located in separate packages. 

照着网上的解决办法，安装了gmp，mpfr，mpc后仍然报错，不过这次只是缺少一个mpc.h，很奇怪，为什么只有这个头文件没有安装成功呢？
输入sudo yum install mpc，会提示已经安装好了最新版mpc-0.22-4.fc19.i686。捜噶，可能是因为fedora19里的最新版还是太旧了。于是到ftp://ftp.gnu.org/gnu/mpc/下载了真正最新版，并且安装成功后，问题完美解决！
