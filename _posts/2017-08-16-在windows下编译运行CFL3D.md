---
layout: post
title:  "在windows下编译运行cfl3d"
date:   2017-08-16 10:00:00+0800
categories: CFL3D Windows MSYS2
typora-root-url: ..
comments: true
---

* 目录
{:toc}
# 用MSYS2安装使用cfl3d

MSYS2是windows下的一个模仿linux的环境，由于虚拟机使用前后处理软件不方便，在调试阶段可以先用MSYS2环境对cfl3d算例进行设置，然后再放到实际的linux环境中进行计算。

注：因为MSYS2是一个模拟环境，所以运行速度比Linux下原生的cfl3d要慢2倍以上。

## 安装

国内建议采用清华大学的MSYS2软件源。网址：https://mirrors.tuna.tsinghua.edu.cn/help/msys2/

首先下载MSYS2的安装文件，最新版是20161025版，链接是：https://mirrors.tuna.tsinghua.edu.cn/msys2/distrib/x86_64/msys2-x86_64-20161025.exe

下载安装到合适的位置（比如`D:\MSYS2`）后， 打开Linux模拟环境时有三个选项：MSYS/MinGW-w64/MinGW-w32 分别对应纯粹MSYS2/本地MinGW64位/本地MinGW32位环境，本文选择`MSYS2 MSYS`环境，这个环境最接近原生的Linux。

与其他Linux操作系统不同，MSYS采用的类似Arch Linux的pacman包管理工具，而不是如同Redhat/CentOS等用的yum或者Ubuntu/Debian用的apt。但是实际的功能是一样的。

但是与其他Linux相同的是，由于主要的软件源在国外，国内访问巨慢。所以需要改成国内源。参考网页https://mirrors.tuna.tsinghua.edu.cn/help/msys2/，可以用记事本打开`D:\MSYS2\etc\pacman.d\mirrorlist.mingw32, mingw64, msys`文件并加入清华的源的地址，或者直接在`MSYS`窗口中输入以下命令：

```shell
echo -e "Server = https://mirrors.tuna.tsinghua.edu.cn/msys2/mingw/i686\n""$(cat /etc/pacman.d/mirrorlist.mingw32)" \
>/etc/pacman.d/mirrorlist.mingw32
echo -e "Server = https://mirrors.tuna.tsinghua.edu.cn/msys2/mingw/x86_64\n""$(cat /etc/pacman.d/mirrorlist.mingw64)" \
>/etc/pacman.d/mirrorlist.mingw64
echo -e "Server = https://mirrors.tuna.tsinghua.edu.cn/msys2/msys/\$arch\n""$(cat /etc/pacman.d/mirrorlist.msys)" \
>/etc/pacman.d/mirrorlist.msys
```

更新源数据库并安装需要的软件：

```shell
pacman -Sy
pacman -S cmake vim gcc-fortran -y
```

附：离线安装方法（为了保密内网的哥们们）

```shell
pacman -Sy
pacman -Sp cmake make git vim gcc-fortran > down.list #生成下载列表
cat down.list
# 用wget 全下载下来。
wget -P ./down -i down.list #下载到当地down文件夹。
pacman -S tar -y #安装tar
tar cf down.tar down/ #打包成了down.tar文件，可以导入内网中。

#在内网中的操作
tar xf down.tar
cd down/
pacman -U *.tar.xz
```

## 下载安装cfl3d并测试

后续操作和linux下的操作几乎完全一样.

```shell
cd $HOME
mkdir cfl3d
cd cfl3d
git clone --depth=1 https://git.coding.net/chengdi123000/cfl3d.git
git clone --depth=1 https://git.coding.net/chengdi123000/cfl3d_testcase.git
cd cfl3d/build
cmake .. -DUSE_CGNS=OFF -DBUILD_MPI=OFF
make -j`nproc` cfl3d_seq splitter #其他的有问题，这两个还是可以编译的，这是最常用的。
cd bin

#测试，重新打开shell
use_cfl3d
cd $HOME/cfl3d/cfl3d_testcase/2DTestCases/NACA_4412
tar xf NACA_4412.tar.Z
cd NACA_4412
pwd -W #查看当前目录在windows下的实际位置。
splitter < split.inp_standard
cfl3d_seq < cfl3d.inp_standard &
tail -f cfl3d.out_standard
```

然后就可以用windows下的后处理工具进行处理了。