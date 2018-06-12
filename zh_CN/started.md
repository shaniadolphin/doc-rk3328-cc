# 上手指南

[ROC-RK3328-CC] 支持从以下存储设备启动：
 - SD 卡
 - eMMC

我们需要将系统固件烧写到 SD 卡或 eMMC 里，这样开发板上电后才能正常启动进入操作系统。

[SDCard Installer] 是官方推荐的 SD 卡烧写工具，它基于 Etcher / Rock64 Installer 定制，实现了一站式的固件选择和烧录操作，让烧写工作变成轻松简单。

<span id="firmware_format"></span>
## 固件格式

固件有两种格式：
 - 原始固件(raw firmware)
 - RK固件(Rockchip firmware)
 
`原始固件(raw firmware)`，是一种能以逐位复制的方式烧写到存储设备的固件，是存储设备的原始映像。`原始固件`一般烧写到 SD 卡中，但也可以烧写到 eMMC 中。
烧写`原始固件`有许多工具可以选用：
- 烧写 SD 卡
  + 图形界面烧写工具：
    * [SDCard Installer] (Linux/Windows/Mac)
    * [Etcher] (Linux/Windows/Mac)
  + 命令行烧写工具
    * [dd] (Linux)
- 烧写 eMMC
  + 命令行烧写工具：
    * update_tool (Linux)
    * rkdeveloptool (Linux)
  + 图形界面烧写工具：
    * android_tool (Windows)

`RK 固件(Rockchip firmware)`，是以 Rockchip专有格式打包的固件，使用 Rockchip 提供的 `update_tool`(Linux) 或 `android_tool`(Windows) 工具烧写到eMMC 闪存中。`RK固件`是 Rockchip 的传统固件打包格式，常用于 Android 设备上。另外，Android 的 RK 固件也可以使用 [SD_Firmware_Tool] 工具烧写到 SD 卡中。

原始固件的通用性更好，因此，为了简单起见，官方**不再提供 RK 固件下载**。

另外，编译 Android SDK会构建出 `boot.img`、`kernel.img`和`system.img`等映像文件，这些映像文件称为`分区映像文件`，用于存储设备对应的分区的烧写中。例如，`kernel.img` 会被写到eMMC 或 SD 卡的 `kernel` 分区。

## 下载和烧写固件

推荐使用 [SDCard Installer] 来烧写固件到 SD 卡。

如果使用 [SDCard Installer] 以外的工具，需要手工到[这里](http://www.t-firefly.com/doc/download/page/id/34.html)下载固件。

以下是支持的系统列表：
 - Android 7.1.2
 - Ubuntu 16.04
 - Debian 9
 - LibreELEC 9.0

**注意**：下载页面提供的全是原始固件，官方**不再提供RK固件**。

固件的烧写方法：
- [烧写 SD 卡](flash_sd.html)
- [烧写 eMMC](flash_emmc.html)

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

[ROC-RK3328-CC]: http://www.t-firefly.com/product/rocrk3328cc.html "ROC-RK3328-CC 官网"
[SDCard Installer]: flash_sd.html#sdcard-installer
[Etcher]: flash_sd.html#etcher
[dd]: flash_sd.html#dd
[SD Firmware Tool]: flash_sd.html#sd-firmware-tool
