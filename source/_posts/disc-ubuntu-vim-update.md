title: Ubuntu下vim更新
date: 2013-09-11 18:33:29
tags: 
- Linux
- Ubuntu
- Vim
categories: 点滴发现
---
Ubuntu下使用Vi时方向键变乱码 退格键不能使用的解决方法:
`sudo apt-get remove vim-common`
`sudo apt-get install vim`
如果安装失败，提示：
```
Package vim is not available, but is referred to by another package.
```
请先执行：
`sudo apt-get update`