title: 第二次编译gcc报错
date: 2013-11-21 21:55:55
tags: 
- Linux
- gcc
- 报错
categories: 点滴发现
---

../../gcc-4.8.2/gcc/config/linux.h:111:10: error: attempt to use poisoned "STANDARD_INCLUDE_DIR" 
   #undef STANDARD_INCLUDE_DIR 
          ^ 
../../gcc-4.8.2/gcc/config/linux.h:112:11: error: attempt to use poisoned "STANDARD_INCLUDE_DIR" 
   #define STANDARD_INCLUDE_DIR "/tools/include/" 
           ^ 
In file included from ./tm.h:34:0, 
                 from ../../gcc-4.8.2/gcc/genflags.c:26: 
../../gcc-4.8.2/gcc/config/i386/linux.h:25:10: error: attempt to use poisoned "STANDARD_INCLUDE_DIR" 
   #undef STANDARD_INCLUDE_DIR 
          ^ 
../../gcc-4.8.2/gcc/config/i386/linux.h:26:11: error: attempt to use poisoned "STANDARD_INCLUDE_DIR" 
   #define STANDARD_INCLUDE_DIR "/tools/include/" 
           ^ 
make[2]: *** [build/genflags.o] Error 1 
make[2]: Leaving directory `/opt/mylinux/build/build-gcc/gcc' 
make[1]: *** [all-gcc] Error 2 
make[1]: Leaving directory `/opt/mylinux/build/build-gcc' 
make: *** [all] Error 2 

过五关，斩六将，一步一个困难，能想到的不能想到的乱七八糟的问题都被我遇到了，我该说我运气好呢还是运气差呢？
要说运气差，我是差到家了，明明是照着教程来的，别人没错，我就各种错！这不是打击我的学习积极性吗？我靠，百度，谷歌，各种论坛，各种教程，各种官网！寻寻觅觅，冷冷清清！众里寻他千百度，蓦然回首，啥也没有！中国研究Linux的人太少了！很多给出答案的人，也是一知半解，或者胡乱猜测！所谓具体问题具体分析，想在网上找到和自己一模一样的报错太难了！也许哪次运气好找到一个和自己完全相同的报错，马上兴奋地点开，却发现，是个未解决问题！我了个心洼凉洼凉的！到论坛发帖，基本不会有大神出现给你个解答，人家大神也忙啊！到群里发问，因为没人会或者没有大神看到，很快就被淹没了！找学长，就像我刚才所说，研究Linux的人太少，而且领域还不一定相同，他们也很难给出解答！找老师，学校里有专门教授Linux的老师吗？术业有专攻，老师也不一定能在第一时间给出解答。好好学英语，看看外国人给的教程，结果失望地发现，国外的网络雷锋要比我们少的多，想看教程？自己去官网吧！种种种种，以至于，我这种把搜索工具玩到出神入化的人都找不到答案！
<!--more-->
要说运气好，我真是个幸运儿。所谓解决的问题越多成长地越快！因为百度谷歌都不会给你答案，你的圈子里的大神也不能给你答案，能依靠的，就只有自己！那就思考分析吧！思考，测试，补充知识，思考，测试，补充知识……最终，很多问题我还是解决了，通过参考官方文档（当然是蛋疼的英文版）！成就感如火山爆发般喷涌啊！要知道，这原本在我的世界里是一个解决不了的问题！

命途多舛的我又遇到了新的问题，好吧，就让我再次来解决它！

再次到了官网，诶？需要打补丁，那咱就打！重新来过，结果，还是报同样的错！
要不，全部按照官网来做？矮油，不错啊，果然没错了！但是过了好久之后，出现了另外一个错误！

/opt/mylinux/tools/bin/../lib/gcc/i686-pc-linux-gnu/4.8.2/../../../../i686-pc-linux-gnu/bin/ld: cannot find -lmpc 
collect2: error: ld returned 1 exit status 
make[2]: *** [cc1] Error 1 
make[2]: Leaving directory `/opt/mylinux/build/build-gcc/gcc' 
make[1]: *** [all-gcc] Error 2 
make[1]: Leaving directory `/opt/mylinux/build/build-gcc' 
make: *** [all] Error 2 

调整了以下，结果还是报错：
/opt/mylinux/tools/bin/../lib/gcc/i686-pc-linux-gnu/4.8.2/../../../../i686-pc-linux-gnu/bin/ld: cannot find -lmpc 
/opt/mylinux/tools/bin/../lib/gcc/i686-pc-linux-gnu/4.8.2/../../../../i686-pc-linux-gnu/bin/ld: cannot find -lz 
collect2: error: ld returned 1 exit status 
make[3]: *** [cc1] Error 1 
make[3]: Leaving directory `/opt/mylinux/build/build-gcc/gcc' 
make[2]: *** [all-stage1-gcc] Error 2 
make[2]: Leaving directory `/opt/mylinux/build/build-gcc' 
make[1]: *** [stage1-bubble] Error 2 
make[1]: Leaving directory `/opt/mylinux/build/build-gcc' 
make: *** [all] Error 2

我了个去，大哥你玩我呢？每次make都要20多分钟才出错，这个时间谁花得起啊？后来多次试验，还是失败，没办法了。只能强制安装了！
make -i 

然后，几分钟后就停止了，我靠！太坑了吧！继续想办法……

