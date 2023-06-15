Labs: JOS, a small O/S for x86 in an exokernel style

kernel interface: expose hardware, but protect -- few abstractions!
unprivileged user-level library: fork, exec, pipe, ...
applications: file system, shell, ..
development environment: gcc, qemu
lab 1 is out
(demo: build JOS and run ls)

# 环境搭建
安装QEMU https://www.cnblogs.com/gatsby123/p/9746193.html

# Lab 1
`make qemu-nox`可不弹出额外的终端界面，通过`Ctrl+a x`退出。

32 bit 操作系统

## 什么是CS和IP
CS和IP是8086CPU中两个关键的寄存器，它们指示了CPU当前要读取指令的地址。

CS代码段寄存器；IP指令指针寄存器。在8086机中，任意时刻，CPU将CS:IP指向的内容当作指令来执行。

## Boot Loader
Bootable硬盘会在第一个存储块（512字节）中保存 boot loader 的代码
boot loader的组成：一份汇编语言源文件boot/boot.S, 一份C语言源文件 boot/main.c

## 遇到的错误
### WSL图形化界面的支持

    $make qemu
    Could not initialize SDL(No available video device) - exiting
解决：缺少环境

    sudo apt-get install -y build-essential libtool libglib2.0-dev libpixman-1-dev zlib1g-dev git libfdt-dev gcc-multilib gdb

### VSCode 与 GDB 调试
`launch.json`文件设置端口
