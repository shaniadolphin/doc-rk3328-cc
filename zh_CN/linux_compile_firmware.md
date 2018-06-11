# 编译 Linux 固件

这一章将介绍编译 [ROC-RK3328-CC] Linux 固件的整个流程。

## 准备工作

Linux 固件在如下的环境中编译:
 - 64 位 CPU
 - Ubuntu 16.04

安装以下包:
```bash
sudo apt-get install git repo gnupg flex bison gperf build-essential \
     zip tar curl libc6-dev gcc-arm-linux-gnueabihf \
     gcc-aarch64-linux-gnu device-tree-compiler lzop libncurses5-dev \
     libssl1.0.0 libssl-dev mtools
```

## 下载 Linux SDK

创建工程目录:
```bash
# create project dir
mkdir ~/proj/roc-rk3328-cc
cd ~/proj/roc-rk3328-cc
```

下载 Linux SDK:
```
# U-Boot
git clone -b release https://github.com/FireflyTeam/u-boot
# Kernel
git clone -b release-4.4 https://github.com/FireflyTeam/kernel --depth=1
# Build
git clone -b debian https://github.com/FireflyTeam/build
# Rkbin
git clone -b master https://github.com/FireflyTeam/rkbin
```

提示：通过以上链接可以在线浏览源码。

开发板编译配置在：

    build/board_configs.sh 

## 编译 U-Boot

编译 U-Boot:
```
./build/mk-uboot.sh roc-rk3328-cc
```

输出:
```
out/u-boot/
├── idbloader.img
├── rk3328_loader_ddr786_v1.06.243.bin
├── trust.img
└── uboot.img
```
 - `rk3328_loader_ddr786_v1.06.243.bin`: DDR 初始化文件。
 - `idbloader.img`: DDR 初始化与 miniloader 结合的文件。
 - `trust.img`: ARM trusted 固件。
 - `uboot.img`: U-Boot映像文件。


相关文件:
- `configs/roc-rk3328-cc_defconfig`: 默认 U-Boot 配置

## 编译 Kernel

编译 kernel:
```
./build/mk-kernel.sh roc-rk3328-cc
```

输出:
```
out/
├── boot.img
└── kernel
    ├── Image
    └── rk3328-roc-cc.dtb
```

 - `boot.img`: 包含 `Image` and `rk3328-roc-cc.dtb` 的映像文件, 为 fat32 文件系统格式。
 - `Image`: 内核映像。
 - `rk3328-roc-cc.dtb`: 设备树 blob。
 
相关文件:
- `arch/arm64/configs/fireflyrk3328_linux_defconfig`: 默认内核配置。
- `arch/arm64/boot/dts/rockchip/rk3328-roc-cc.dts`: 开发板设备树描述。
- `arch/arm64/boot/dts/rockchip/rk3328.dtsi`: CPU 设备树描述。
 
自定义内核配置和更新默认配置:

```
# 这非常重要!
export ARCH=arm64

cd kernel

# 首先使用默认配置
make fireflyrk3328_linux_defconfig

# 自定义你的 kernel 配置
make menuconfig

# 保存为默认配置
make savedefconfig
cp defconfig arch/arm64/configs/fireflyrk3328_linux_defconfig
```

**注意**: 构建脚本不会将内核模块复制到根文件系统。

## 编译根文件系统

可以下载预编译的根文件系统，或参考 [Linux Building Root Filesystem](linux_build_rootfilesystem.html) 自己编译一个。

## 打包原始固件

把你的 Linux 根文件系统映像文件放在 `out/rootfs.img`

`out` 目录将包含以下文件:
```
$ tree out
out
├── boot.img
├── kernel
│   ├── Image
│   └── rk3328-roc-cc.dtb
├── rootfs.img
└── u-boot
    ├── idbloader.img
    ├── rk3328_loader_ddr786_v1.06.243.bin
    ├── trust.img
    └── uboot.img

2 directories, 8 files
```

创建原始固件:
```
./build/mk-image.sh -c rk3328 -t system -r out/rootfs.img
```

这条命令根据[《存储映射》](http://opensource.rock-chips.com/wiki_Partitions#Default_storage_map)所描述的布局，将分区映像文件写到指定位置，最终打包成 `out/system.img`，

烧写原始映像格式的固件步骤，请参考 [《开始上手》](started.html) 一章。

[ROC-RK3328-CC]: http://www.t-firefly.com/product/rocrk3328cc.html "ROC-RK3328-CC 官网"

