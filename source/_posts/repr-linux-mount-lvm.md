title: Lvm分区挂载
date: 2013-11-30 19:54:24
tags: Linux
categories: 精华转载
---

对LVM分区进行挂载，前提要能够被探测到，然后激活，再挂载。
1. 探测VolGroup
`vgscan`
2. 激活
`vgchange -a y VolGroup00`
3. 挂载
`mount /dev/VolGroup00/LogVol01 /mnt/hdb2`

<!--more-->
1.首先使用vgscan 扫描 lvm 结果如下
zhq@zhq:/$ sudo vgscan
  Reading all physical volumes.  This may take a while...
  Found volume group "VolGroup00" using metadata type lvm2
2.通过 vgdisplay VolGroup00 查看 lvm 的 VG UUID
zhq@zhq:/$ sudo vgdisplay VolGroup00
  --- Volume group ---
  VG Name               VolGroup00
  System ID             
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  3
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                2
  Open LV               0
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               19.62 GiB
  PE Size               32.00 MiB
  Total PE              628
  Alloc PE / Size       628 / 19.62 GiB
  Free  PE / Size       0 / 0   
  VG UUID               xPw6Yb-1SYW-ayBT-TqOP-hF4i-w02x-vY0165
3.重命名（本人认为此步可省，可能是源于简洁吧，呵呵，没试，你可以试试，呵呵）
`sudo vgrename VolGroup00 /dev/vg0`
Volume group "VolGroup00" successfully renamed to "vg0"

这个时候通过 vgdisplay 就可以看到 VG的信息了
`sudo vgdisplay`
  --- Volume group ---
  VG Name               vg0
  System ID            
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  4
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                2
  Open LV               0
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               19.62 GiB
  PE Size               32.00 MiB
  Total PE              628
  Alloc PE / Size       628 / 19.62 GiB
  Free  PE / Size       0 / 0  
  VG UUID               xPw6Yb-1SYW-ayBT-TqOP-hF4i-w02x-vY0165

4.激活VG
VG重命名后，默认是非ACTIVE状态,我们要通过以下方式激活VG
`sudo lvscan`
  ACTIVE            '/dev/vg0/LogVol00' [18.16 GiB] inherit
  ACTIVE            '/dev/vg0/LogVol01' [1.47 GiB] inherit

激活 VG
`sudo vgchange -ay /dev/vg0`
  2 logical volume(s) in volume group "vg0" now active

查看状态
`sudo lvscan`
/LogVol00' [18.16 GiB] inherit
  ACTIVE            '/dev/vg0/LogVol01' [1.47 GiB] inherit

`sudo vgscan`
  Reading all physical volumes.  This may take a while...
  Found volume group "vg0" using metadata type lvm2
