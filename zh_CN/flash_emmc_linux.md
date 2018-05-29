# Linux 下的 eMMC 升级

## 烧写原始镜像

原始镜像需要从 eMMC 的 offset 为 0 的起始地址烧录。但是，在 [Rockusb 模式] 中不能这样做，因为所有 LBA 写入都是从 0x2000 扇区开始的。因此，该设备必须强制进入 [Maskrom 模式]。

烧写原始镜像的步骤:
1. 强制设备进入 [Maskrom 模式]
2. 使用 `rkdeveloptool` 烧写统一固件
    ```
    rkdeveloptool db     out/u-boot/rk3328_loader_ddr786_v1.06.243.bin
    rkdeveloptool wl 0x0 out/system.img
    rkdeveloptool rd     # reset device to boot
    ```

更多安装 `rkdeveloptool` 信息和用法，点击 [这里](#rkdeveloptool).

## 烧写 Rockchip 固件

使用 `upgrade_tool` 将 Rockchip 固件烧写到 eMMC 中，可以在 [Rockusb 模式] 或 [Maskrom 模式]

烧写 Rockchip 固件的步骤为:
1. 强制设备进入 [Rockusb 模式] or [Maskrom 模式]
2. 使用 `upgrade_tool` 烧写 Rockchip 固件
    ```
    upgrade_tool uf /path/to/rockchip/firmware
    ```

更多安装 `upgrade_tool` 信息和用法，点击 check [这里](#upgrade-tool)

## 烧写分区镜像

可以将单独的分区镜像写入 eMMC , 但根据初始固件格式，说明可能会有所不同。

**原始固件**

如果初始固件格式是原始的，那么很可能它使用 `GPT` 分区方案，并且每个分区的预定义偏移量和大小可以在 `build / partitions.sh` 中找到

1. 强制设备进入 [Maskrom 模式]
2. 使用 `rkdeveloptool` 烧写分区镜像:
    ```
    rkdeveloptool db         out/u-boot/rk3328_loader_ddr786_v1.06.243.bin
    rkdeveloptool wl 0x40    out/u-boot/idbloader.img
    rkdeveloptool wl 0x4000  out/u-boot/uboot.img
    rkdeveloptool wl 0x6000  out/u-boot/trust.img
    rkdeveloptool wl 0x8000  out/boot.img
    rkdeveloptool wl 0x40000 out/linaro-rootfs.img
    rkdeveloptool rd         # reset device to boot
    ```

分区偏移量可以从这儿找到 [这里](#linux-partition-offset).

更多 `rkdeveloptool` 的安装和用法信息，点击 [这里](#rkdeveloptool)

**Rockchip 固件**

如果初始固件的格式是 Rockchip, 它使用 `parameter` 文件作为分区方案，并且可使用分区名称来刷新分区镜像

1. 强制设备进入 [Rockusb 模式] or [Maskrom 模式].
2. 使用 `upgrade_tool` 烧写分区镜像:
    ```
    upgrade_tool di -b /path/to/boot.img
    upgrade_tool di -k /path/to/kernel.img
    upgrade_tool di -s /path/to/system.img
    upgrade_tool di -r /path/to/recovery.img
    upgrade_tool di -m /path/to/misc.img
    upgrade_tool di resource /path/to/resource.img
    upgrade_tool di -p parameter   # flash parameter
    upgrade_tool ul bootloader.bin # flash bootloader
    ```

注意:
- `-b` 是 `boot` 分区的预定义缩写。如果没有缩写可用，请改为使用分区名称作为 `resource`
- 可根据 [Parameter file format](http://www.t-firefly.com/download/Firefly-RK3399/docs/Rockchip%20Parameter%20File%20Format%20Ver1.3.pdf) 自定义 kernel parameters 和 分区布局，分区布局更改后，必须重新刷新相应的分区

更多 `upgrade_tool` 的安装和用法信息，点击 [这里](#upgrade-tool)

## 烧写工具

### rkdeveloptool

`rkdeveloptool` 是由 Rockchip 开发的开源命令行烧写工具，它是闭源 `upgrade_tool`（＃升级工具）的替代品

`rkdeveloptool` 不支持专有 Rockchip 格式的固件

安装 `rkdeveloptool`:

    #install libusb and libudev
    sudo apt-get install pkg-config libusb-1.0 libudev-dev libusb-1.0-0-dev dh-autoreconf
    # clone source and make
    git clone https://github.com/rockchip-linux/rkdeveloptool
    cd rkdeveloptool
    autoreconf -i
    ./configure
    make
    sudo make install

**注意**: 添加 `udev` 规则依据此说明 [这里](#udev), 为了普通用户有权限烧写 Rockchip 设备. 若没有做这个，每次执行命令前就需要加 `sudo`

烧写分区镜像:

    rkdeveloptool db           out/u-boot/rk3328_loader_ddr786_v1.06.243.bin
    rkdeveloptool wl 0x40      out/u-boot/idbloader.img
    rkdeveloptool wl 0x4000    out/u-boot/uboot.img
    rkdeveloptool wl 0x6000    out/u-boot/trust.img
    rkdeveloptool wl 0x8000    out/boot.img
    rkdeveloptool wl 0x40000   out/linaro-rootfs.img
    rkdeveloptool rd           # reset device to boot

烧写原始镜像:

    rkdeveloptool db           out/u-boot/rk3328_loader_ddr786_v1.06.243.bin
    rkdeveloptool wl 0x0       out/system.img
    rkdeveloptool rd           # reset device to boot

分区偏移量看[这里](#partition%20offset)

### upgrade_tool

`upgrade_tool** 是由 Rockchip 提供的闭源的命令行工具, 支持 Rockchip 格式的分区镜像以及统一固件的烧写

下载 [Linux_Upgrade_Tool](https://gitlab.com/TeeFirefly/RK3328-Nougat/blob/roc-rk3328-cc/RKTools/linux/Linux_Upgrade_Tool/Linux_Upgrade_Tool_v1.24.zip**, 电脑上安装:

    unzip Linux_Upgrade_Tool_v1.24.zip
    cd Linux_UpgradeTool_v1.24
    sudo mv upgrade_tool /usr/local/bin
    sudo chown root:root /usr/local/bin/upgrade_tool


**注意**: 添加 `udev` 规则依据此说明 [这里](#udev), 为了普通用户有权限烧写 Rockchip 设备. 若没有做这个，每次执行命令前就需要加 `sudo`

烧写 Rockchip 固件:

    upgrade_tool uf update.img

烧写分区镜像:

    upgrade_tool di -b /path/to/boot.img
    upgrade_tool di -k /path/to/kernel.img
    upgrade_tool di -s /path/to/system.img
    upgrade_tool di -r /path/to/recovery.img
    upgrade_tool di -m /path/to/misc.img
    upgrade_tool di resource /path/to/resource.img
    upgrade_tool di -p parameter   # flash parameter
    upgrade_tool ul bootloader.bin # flash bootloader

如果由于闪存存储问题而出现错误，可以尝试使用低格或擦除闪存

    upgrade_tool lf   # 低格闪存
    upgrade_tool ef   # 擦除闪存

### udev
用如下内容创建 `/etc/udev/rules.d/99-rk-rockusb.rules` [1](https://github.com/rockchip-linux/rkdeveloptool/blob/master/99-rk-rockusb.rules). 如有必要，用实际 Linux 组替换 `users` 组:
```
SUBSYSTEM!="usb", GOTO="end_rules"

# RK3036
ATTRS{idVendor}=="2207", ATTRS{idProduct}=="301a", MODE="0666", GROUP="users"
# RK3229
ATTRS{idVendor}=="2207", ATTRS{idProduct}=="320b", MODE="0666", GROUP="users"
# RK3288
ATTRS{idVendor}=="2207", ATTRS{idProduct}=="320a", MODE="0666", GROUP="users"
# RK3328
ATTRS{idVendor}=="2207", ATTRS{idProduct}=="320c", MODE="0666", GROUP="users"
# RK3368
ATTRS{idVendor}=="2207", ATTRS{idProduct}=="330a", MODE="0666", GROUP="users"
# RK3399
ATTRS{idVendor}=="2207", ATTRS{idProduct}=="330c", MODE="0666", GROUP="users"

LABEL="end_rules"
```

非重启方式重载 udev 规则

    sudo udevadm control --reload-rules
    sudo udevadm trigger


## FAQ

### Linux 分区偏移

分区镜像的偏移量可以通过以下命令获得（假设位于 Firefly Linux SDK 的目录中）:

    (. build/partitions.sh ; set | grep _START | \
    while read line; do start=${line%=*}; \
    printf "%-10s 0x%08x\n" ${start%_START*} ${!start}; done )

会得到:

    ATF        0x00006000
    BOOT       0x00008000
    LOADER1    0x00000040
    LOADER2    0x00004000
    RESERVED1  0x00001f80
    RESERVED2  0x00002000
    ROOTFS     0x00040000
    SYSTEM     0x00000000

[rkdeveloptool]: https://github.com/rockchip-linux/rkdeveloptool
[Rockusb 模式]: bootmode.html#rockusb-mode
[Maskrom 模式]: bootmode.html#maskrom-mode
