## 启动模式

过去，eMMC 闪存是一般直接焊到电路板上的，因此以须使用板载方式来更新固件。

要将固件刷入 eMMC 闪存，需要用公对公的 USB 线将主机 PC 和开发板上的 USB OTG 口(双层 USB端口靠近板子的那个）连接起来。

![](img/hw_board_usbconn.png)

取决于 eMMC 闪存现存的内容，开发板有两种特殊的启动模式： [Rockusb 模式] 和 [Maskrom 模式]

通常只需要进入 [Rockusb 模式] 即可升级现有的 Android 或 Ubuntu 系统，而升级固件需是 RK 固件格式。

[Maskrom 模式] 则是系统未能识别到合法的启动设备而进入的模式。烧写原始固件到 eMMC也需进入该模式。

### Rockusb 模式

如果开发板启动并在 eMMC 中找到有效的 IDB（IDentity Block），它将继续在 eMMC 中加载 bootloader（此处使用U-Boot）并将执行控制权交给它。

如果 bootloader 检测到 Recovery 按钮按下并且 USB 已连接，它就会进入 [Rockusb 模式]，等待来自主机的进一步命令。

准备工作：
 - 5V2A 电源适配器。
 - 连接电源适配器和开发板的 USB 线。
 - 用来连接电脑 PC 以及开发板的公对公 USB 线。

操作步骤：
 1. 将所有 USB 线（包括 Micro USB 线和公对公 USB 线）拔出开发板，以保持电路板断电。
 2. 拔出 SD 卡。
 3. 使用公对公 USB 线将主机与开发板的较低 USB 2.0端口相连。
 4. 按住开发板上的 RECOVERY 按键。
 5. 从开发板上拔出 micro USB 线。
 6. 等待大概 3 秒左右松开 RECOVERY 按键。

### Maskrom 模式

若开发板上电后遇到以下情况：
 - eMMC 为空。
 - eMMC 上的 bootloader 坏了。
 - 将 eMMC 数据/时钟引脚接地，eMMC读取数据失败。

这些表示在 eMMC 中将找不到有效的 IDB（IDentity Block），CPU 将执行一个小型的 ROM 代码，等待主机通过 USB 上传 bootloader 来初始化DDR内存和进入升级状态。这种模式称为 `Maskrom模式` 。

强制进入 `MaskRom模式` 涉及到硬件操作，具有一定的风险，因此操作上需要 **非常谨慎** 。

准备工作：
 - 5V2A 电源适配器。
 - 用来连接电源适配器和开发板的 Micro USB 线。
 - 连接电脑 PC 和开发板的公对公 USB 线。
 - 用于将 eMMC 时钟引脚短接到地的金属镊子。

操作步骤：
 1. 从开发板上拔出所有的 USB 线（包括 micro USB 线以及公对公 USB 线）,保持开发板的断电状态。
 2. 从开发板上拔出 SD 卡。
 3. 使用公对公 USB 线连接电脑 PC 和开发板上较低的 USB 2.0 口。
 4. 找到开发板上预留的 eMMC 的 CLK 引脚和 GND 脚，见下图：
 ![](img/rk3328_maskrom_pads.jpg)
 5. 用金属镊子短接 eMMC 的 CLK 和 GND 焊盘，并保持短接良好。
 6. 将 micro USB 线插到开发板上。
 7. 保持住一秒后松开镊子。

[Rockusb 模式]: bootmode.html#rockusb-mode
[Maskrom 模式]: bootmode.html#maskrom-mode
