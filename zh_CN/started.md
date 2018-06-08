# 上手指南

[ROC-RK3328-CC] 支持从以下存储设备启动：
 - SD 卡
 - eMMC 闪存

如果你需要使用 SD 卡来启动开发板，并且所使用 Windows 操作系统主机，那可以直接参阅[《在Windows 下烧写固件到SD卡》](flash_sd_windows.html)。

使用官方的烧写工具 [SDCard Installer]，让烧写固件的工作变得简单，轻松点击几下便可以开始。

如果你想要了解更多详情，以及更喜欢离线烧写固件，那么请继续往下看。

## 固件格式

固件有两种格式：
 - 原始固件(raw firmware)
 - RK固件(Rockchip firmware)
 
`原始固件(raw firmware)`，是一种能以逐位复制的方式烧写到存储设备的固件，是存储设备的原始映像。在 Linux 中，使用 `dd` 命令就能直接烧写这种类型的固件。。而在 Windows 中，可以使用图形化固件烧写工具，如 [Etcher] 或 [SDCard Installer]（后者基于 Rock64 Installer 订制）。

`RK 固件(Rockchip firmware)`，是以 Rockchip专有格式打包的固件，使用 Rockchip 提供的 `update_tool`(Linux) 或 `android_tool`(Windows) 工具烧写到eMMC 闪存中。另外，RK 固件也可以使用 [SD_Firmware_Tool] 工具烧写到 SD 卡中。

`RK 固件`和`原始固件`能够相互转换。这可能让人困惑，其实`RK固件`是 Rockchip 平台设备使用的传统格式固件，特别是在 Android 设备上，而`原始固件`用于 SD 卡的烧写更适合。

编译 Android SDK会构建出 `boot.img`、`kernel.img`和`system.img`等映像文件，这些映像文件称为`分区映像文件`，用作烧写到存储设备对应的分区中。例如，`kernel.img` 会烧写到eMMC 或 SD 卡的 `kernel` 分区。

## 下载和烧写固件

**RK固件** 下载列表:
 - Android 7.1.2 [💾](http://www.t-firefly.com/share/index/listpath/id/08cb58f6a5f8e4977275bd45a446764f.html)
 - Ubuntu 16.04 [💾](http://www.t-firefly.com/share/index/listpath/id/b99bb982578de0acf7261f96be2b8ba2.html)

`RK 固件`一般烧写到eMMC 中，根据操作系统的不同，参照相应指令即可：[Windows](flash_emmc_windows.html)， [Linux](flash_emmc_linux.html)。

Android 的`RK固件`也可以烧写到 SD 卡中，使用[SD_Firmware_Tool](flash_sd_windows.html#flashing-rockchip-firmware) 即可。

**原始固件** 下载列表：
 - Android 7.1.2 [💾](http://t-firefly.oss-cn-hangzhou.aliyuncs.com/product/RK3328/Firmware/Android/ROC-RK3328-CC_Android7.1.2_180411/ROC-RK3328-CC_Android7.1.2_180411.img.gz)
 - Ubuntu 16.04 [💾](http://download.t-firefly.com/product/RK3328/Firmware/Linux/ROC-RK3328-CC_Ubuntu16.04_Arch64_20180315/ROC-RK3328-CC_Ubuntu16.04_Arch64_20180315.zip)
 - Station OS [💾](http://download.t-firefly.com/product/Station%20OS/Station_OS_for_ROC-RK3328-CC_SDCard_Installer_v1.2.3.zip)
 - LibreELEC [💾](http://download.t-firefly.com/product/RK3328/Firmware/Linux/LibreELEC/ROC-RK3328-CC_LibreELEC9.0_180324/ROC-RK3328-CC_LibreELEC9.0_180324.zip)

`原始固件` 一般烧写到 SD 卡中，根据操作系统的不同，参照相应指令即可：[Windows](flash_sd_windows.html)，[Linux](flash_sd_linux.html)。

如果需要编译固件，请参考开发者指南。

## 开发板上电启动

在开发板上电启动前，确认以下事项：
 - 可启动的 SD 卡或eMMC
 - 5V2A 电源适配器
 - Micro USB 线

然后按照以下步骤操作：

 1. 将电源适配器拔出电源插座。
 2. 使用 micro USB 线连接电源适配器和主板。
 3. 插入可启动的 SD 卡或eMMC 之一（不能同时插入）。
 4. 插入 HDMI 线、USB 鼠标或键盘（可选）。
 5. 检查一切连接正常后，电源适配器上电。

[ROC-RK3328-CC]: http://en.t-firefly.com/product/rocrk3328cc.html "ROC-RK3328-CC Official Website"
[SDCard Installer]: http://www.t-firefly.com/share/index/index/id/acd8e1e37176fba5bf61fb7bf4503998.html
[Etcher]: https://etcher.io
[SD_Firmware_Tool]: https://pan.baidu.com/s/1migPY1U#list/path=%2FPublic%2FDevBoard%2FROC-RK3328-CC%2FTools%2FSD_Firmware_Tool&parentPath=%2FPublic%2FDevBoard%2FROC-RK3328-CC
[AndroidTool]: https://pan.baidu.com/s/1migPY1U#list/path=%2FPublic%2FDevBoard%2FROC-RK3328-CC%2FTools%2FAndroidTool&parentPath=%2FPublic%2FDevBoard%2FROC-RK3328-CC
