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

32-bit 操作系统

## 启动过程
BIOS运行,初始化PCI总线和所有BIOS负责的重要设备后，开始搜索软盘、硬盘、或是CD-ROM等可启动的设备。最终，当它找到可引导（bootable）磁盘时，从磁盘读取引导加载程序（boot loader）并将控制权转移给它。

## CS和IP
CS和IP是8086CPU中两个关键的寄存器，它们指示了CPU当前要读取指令的地址。

CS代码段寄存器；IP指令指针寄存器。在8086机中，任意时刻，CPU将CS:IP指向的内容当作指令来执行。

## Boot Loader
Bootable硬盘会在第一个存储块（扇区，512字节）中保存 boot loader 的代码
boot loader的组成：一份汇编语言源文件boot/boot.S, 一份C语言源文件 boot/main.c

## GDB 命令
`b` 指断点，后可跟地址`*0x7c00`、函数、文件位置`file:line`
`si` 执行一步，`si N`执行 N 步
`c` 继续执行直到断点，或 `ctrl+c`打断

## VMA (link address) & LMA (load address)
LMA指该扇区应该加载到内存的位置（起始地址）；VMA指该扇区从某处地址开始执行。

Boot loader的VMA和LMA相同；kernel不同，内核通常在高地址（虚拟地址）运行

## boot/main.c
从磁盘依次读取kernel的各个扇区，并根据LMA加载到内存，最后从ELF的link address（e_entry）开始执行

## 虚拟地址
借助处理器内存管理硬件，将链接地址（虚拟地址）映射到加载地址（物理地址）。



## 遇到的错误
### WSL图形化界面的支持

    $make qemu
    Could not initialize SDL(No available video device) - exiting
解决：缺少环境

    sudo apt-get install -y build-essential libtool libglib2.0-dev libpixman-1-dev zlib1g-dev git libfdt-dev gcc-multilib gdb

### VSCode 与 GDB 调试
`launch.json`文件设置端口

# Lab 2
256MB物理内存：0x00000000 ~ 0x0fffffff
虚拟地址：0xf0000000 ~ 0xffffffff
