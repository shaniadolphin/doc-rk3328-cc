# 编译 Android 7.1

## 准备

### 硬件配置

编译 Android 7.1 开发环境硬件配置建议:
 - 64 位 CPU
 - 16GB  内存 + 交换内存
 - 30GB  空闲空间用来编译, 源码树另占 8GB

另外可参考 Google 官方文档硬件和软件配置:
 - [https://source.android.com/setup/build/requirements](https://source.android.com/setup/build/requirements)
 - [https://source.android.com/setup/initializing](https://source.android.com/setup/initializing)

### 软件配置

**安装 JDK 8**
```bash
sudo add-apt-repository ppa:openjdk-r/ppa
sudo apt-get update
sudo apt-get install openjdk-8-jdk
```

**安装环境包**

```bash
sudo apt-get install git-core gnupg flex bison gperf libsdl1.2-dev \
  libesd0-dev libwxgtk2.8-dev squashfs-tools build-essential zip curl \
  libncurses5-dev zlib1g-dev pngcrush schedtool libxml2 libxml2-utils \
  xsltproc lzop libc6-dev schedtool g++-multilib lib32z1-dev lib32ncurses5-dev \
  lib32readline-gplv2-dev gcc-multilib libswitch-perl

sudo apt-get install gcc-arm-linux-gnueabihf \
  libssl1.0.0 libssl-dev \
  p7zip-full
```

## 下载 Android SDK

由于 SDK 较大，请选择以下云盘之一下载 `ROC-RK3328-CC_Android7.1.2_git_20171204.7z`:
 - [Baiduyun](https://pan.baidu.com/s/1eRT6isE "Android 7.1 SDK baiduyun")
 - [Google Drive](https://drive.google.com/drive/folders/1N8fpfoeWLD4-VJcYN6Qfh_3-YBYzXxGq "Android 7.1 SDK Google Drive")

下载完成后，在解压前先校验下 MD5 码值:
```
$ md5sum /path/to/ROC-RK3328-CC_Android7.1.2_git_20171204.7z
6d34e51fd7d26e9e141e91b0c564cd1f ROC-RK3328-CC_Android7.1.2_git_20171204.7z
```

然后解压:
```bash
mkdir -p ~/proj/roc-rk3328-cc
cd ~/proj/roc-rk3328-cc
7z x /path/to/ROC-RK3328-CC_Android7.1.2_git_20171204.7z
git reset --hard
```

更新远程仓库:
```bash
git remote rm origin
git remote add gitlab  https://gitlab.com/TeeFirefly/RK3328-Nougat.git
```
从 gitlab 处同步源码:
```bash
git pull gitlab roc-rk3328-cc:roc-rk3328-cc
```

也可以到如下地址查看源码:
  [https://gitlab.com/TeeFirefly/RK3328-Nougat/tree/roc-rk3328-cc](https://gitlab.com/TeeFirefly/RK3328-Nougat/tree/roc-rk3328-cc)

## 使用 Firefly 脚本编译

**编译 Kernel**
```bash
./FFTools/make.sh -k -j8
```

**编译 U-Boot**
```bash
./FFTools/make.sh -u -j8
```

**编译 Android**    
```bash
./FFTools/make.sh -a -j8
```

**编译全部**

如下指令会编译出 kernel, U-Boot 以及 Android
```bash
./FFTools/make.sh -j8
```

## 不使用脚本编译

编译之前请先执行如下命令配置好环境变量:

```bash
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64 
export PATH=$JAVA_HOME/bin:$PATH 
export CLASSPATH=.:$JAVA_HOME/lib:$JAVA_HOME/lib/tools.jar
```

**编译 Kernel**
```bash
make ARCH=arm64 firefly_defconfig
make -j8 ARCH=arm64 rk3328-roc-cc.img
```

**编译 U-Boot**
```bash
make rk3328_box_defconfig
make ARCHV=aarch64 -j8
```

**编译 Android**
```bash
source build/envsetup.sh
lunch roc_rk3328_cc_box-userdebug
make installclean
make -j8
./mkimage.sh
```

## 打包 Rockchp 固件

**在 Linux 下打包固件**

编译完成后可以使用 Firefly 官方脚本打包所有的分区镜像成一个 Rockchip 的统一固件，用以下命令即可实现:
```bash
./FFTools/mkupdate/mkupdate.sh update
```

最终生成的文件是 `rockdev/Image-rk3328_firefly_box/update.img`.

**在 Windows 下打包固件**

在 Windows 下打包 Rockchip 固件 `update.img` 也是很简单的:
1. 拷贝所有在 `rockdev/Image-rk3328_firefly_box/` 目录下编译好的文件到 AndroidTool 的 `rockdev\Image` 目录下
2. 运行在 AndroidTool 的 `rockdev` 目录下的 `mkupdate.bat` 文件
3. 在 `rockdev\Image` 目录将会生成 `update.img`

## 分区镜像

`update.img` 是面向终端用户的固件, 为了方便升级开发板的系统而发布的
在只进行单个分区镜像修改的场合，这样做可以大大节省开发过程中的时间

下表是编译各个阶段分区镜像的总结:

```
|------------------|---------------------|-----------|
| Stage            | Product             | Partition |
|------------------|---------------------|-----------|
| Compiling Kernel | kernel/kernel.img   | kernel    |
|                  | kernel/resource.img | resource  |
|------------------|---------------------|-----------|
| Compiling U-Boot | u-boot/uboot.img    | uboot     |
|------------------|---------------------|-----------|
| ./mkimage.sh     | boot.img            | boot      |
|                  | system.img          | system    |
|------------------|---------------------|-----------|
```

注意，执行 `./mkimage.sh` 后, `boot.img` 和 `system.img` 将会被重新编译并打包在安卓目录 `out/target/product/rk3328_firefly_box/` 下，所有生成的镜像文件将会拷贝到目录 `rockdev/Image-rk3328_firefly_box/` 下
如下是镜像文件列表:
 - `boot.img`: Android 的 initramfs 镜像，包含Android根目录的基础文件系统，它负责初始化和加载系统分区
 - `system.img`: ext4 文件系统格式的 Android 文件系统分区镜像
 - `kernel.img`: kernel 镜像
 - `resource.img`: Resource 镜像, 包含 boot log 以及 kernel 设备树 blob
 - `misc.img`: misc 分区镜像, 负责启动模式的切换和急救模式参数的传递
 - `recovery.img`: Recovery 模式镜像
 - `rk3328_loader_v1.08.244.bin`: Loader 文件
 - `uboot.img`: U-Boot 镜像文件
 - `trust.img`: 睡眠启动相关文件
 - `parameter.txt`: 分区 layout 以及 kernel 命令行

