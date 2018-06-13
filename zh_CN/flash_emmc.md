# 烧写 eMMC

## 启动模式

eMMC 一般都是直接焊在主板上，有些虽然是可插拔的，但没有专用的读卡器，因此需要使用板载烧写的方式来更新固件，即板上跑一个小系统，负责从主机或其它存储介质读取固件，再烧写到 eMMC 上。

取决于 eMMC 现存的内容，开发板有两种特殊的启动模式： [Rockusb 模式] 和 [Maskrom 模式]。

通常只需要进入 [Rockusb 模式] 即可升级现有的 Android 或 Ubuntu 系统。这种方式升级方式一般适用于 [RK 固件]或[分区映像]。

[Maskrom 模式] 则是系统未能识别到合法的启动设备而进入的模式。烧写[原始固件]到 eMMC 必须要进入该模式。

<span id="rockusb-mode"></span>

### Rockusb 模式

开发板启动后，CPU 如果在 eMMC 中找到有效的 IDB (IDentity Block)，它将从 eMMC 读取并加载 bootloader，并将执行控制权交给它。

如果 bootloader 检测到 Recovery 按钮按下并且 USB 已连接，它就会进入 [Rockusb 模式]，等待来自主机的命令。

进入 [Rockusb 模式] 的准备工作：
 - 5V2A 电源适配器。
 - 连接电源适配器和开发板的 Micro USB 线。
 - 用来连接电脑 PC 以及开发板的公对公 USB 线。
 - eMMC 。

操作步骤：
 1. 将所有 USB 线（包括 Micro USB 线和公对公 USB 线）拔出开发板，以保持电路板断电。
 2. 安装好 eMMC，拔出 SD 卡。
 3. 使用公对公 USB 线将主机的 USB 端口与开发板的靠近电路板的 USB 2.0端口相连：
    ![](img/hw_board_usbconn.png)
 4. 按住开发板上的 RECOVERY 按键。
 5. 从开发板上拔出 Micro USB 线。
 6. 等待大概 3 秒左右松开 RECOVERY 按键。

<span id="maskrom-mode"></span>

### Maskrom 模式

如果开发板上电后遇到以下情况之一：
 - eMMC 内容为空。
 - eMMC 上的 bootloader 损坏。
 - 将 eMMC 数据/时钟引脚接地，eMMC读取数据失败。

CPU 在 eMMC 中就会找不到有效的 IDB (IDentity Block)，转而执行一段小型的 ROM 代码，等待主机通过 USB 上传 bootloader 来初始化 DDR 内存并进入升级状态。这种模式称为 `Maskrom模式` 。

强制进入 `MaskRom模式` 涉及到硬件操作，具有一定的风险，因此操作上需要 **非常谨慎** 。

进入 [Maskrom 模式] 的准备工作：
 - 5V2A 电源适配器。
 - 用来连接电源适配器和开发板的 Micro USB 线。
 - 连接电脑 PC 和开发板的公对公 USB 线。
 - 用于将 eMMC 时钟引脚短接到地的金属镊子。
 - eMMC 。

操作步骤：
 1. 从开发板上拔出所有的 USB 线（包括 micro USB 线以及公对公 USB 线）,保持开发板的断电状态。
 2. 安装好 eMMC，拔出 SD 卡。
 3. 使用公对公 USB 线将主机的 USB 端口与开发板的靠近电路板的 USB 2.0端口相连：
    ![](img/hw_board_usbconn.png)
 4. 找到开发板上预留的 eMMC 的 CLK 引脚和 GND 脚，见下图：
    ![](img/rk3328_maskrom_pads.jpg)
 5. 用金属镊子短接 eMMC 的 CLK 和 GND 焊盘，并保持短接良好。
 6. 将 micro USB 线插到开发板上。
 7. 保持住一秒后松开镊子。

## 烧写工具

请根据所用主机的操作系统选择相应的烧写 eMMC工具：

- 烧写 eMMC
  + 图形界面烧写工具：
    * [AndroidTool] (Windows)
  + 命令行烧写工具：
    * [upgrade_tool] (Linux)
    * [rkdeveloptool] (Linux)

## AndroidTool

[AndroidTool] 是 Windows 下用来烧写[原始固件]、[RK 固件]和[分区映像]到 eMMC 的工具。

使用 [AndroidTool] 之前， 应先[安装 Rockusb 驱动]，然后再安装运行 [AndroidTool]。

<span id="install-rockusb-driver"></span>

### 安装 Rockusb 驱动

下载 [DriverAssistant](https://pan.baidu.com/s/1migPY1U#list/path=%2FPublic%2FDevBoard%2FROC-RK3328-CC%2FTools%2FRKTools%2Fwindows&parentPath=%2FPublic%2FDevBoard%2FROC-RK3328-CC
)， 解压后运行里面的 `DriverInstall.exe`：

![](img/started_driverassistant.png)

点击 "驱动安装" 按钮安装驱动；如果想卸载驱动，则点击 "驱动卸载" 按钮。

若设备处于 [Rockusb 模式] 或 [Maskrom 模式]，在设备管理器中会出现 "Rockusb Device"：

![](img/started_driverassistant_dev.png)

这表示驱动安装成功。

### 安装 AndroidTool

> [下载地址](http://www.t-firefly.com/share/index/listpath/id/acd8e1e37176fba5bf61fb7bf4503998.html)
 
解压后运行里面的 `AndroidTool.exe`：

![](img/androidtool.png)

若设备处于 [Rockusb 模式]，状态行将显示 "Found One LOADER Device"。

若设备处于 [Maskrom 模式]，状态行将显示 "Found One MASKROM Device"。

### 烧写原始固件

[原始固件]需要从 eMMC 的偏移地址为 0 的位置开始烧写。但在 [Rockusb 模式] 下无法做到这点，因为所有 LBA 写入操作会偏移 0x2000 个扇区（即 LBA0 对应于存储设备上第 0x2000 个扇区）。因此，开发板必须强制进入 [Maskrom 模式] 才能烧写[原始固件]。

使用 [AndroidTool] 烧写[原始固件]的步骤如下：

1. 强制设备进入 [Maskrom 模式]。
2. 运行 [AndroidTool]。
3. 打开 “Download Image” 制表页。
4. 保持表格的第一行不变， 使用默认的 “MiniLoader” 文件。
5. 点击第二行右侧的空白单元格，在弹出的文件对话框里打开[原始固件]文件。
6. 点击 "Run" 按钮开始烧写。

![](img/androidtool_flash_image.png)

### 烧写 RK 固件

使用 [AndroidTool] 烧写 [RK 固件]的步骤如下：

1. 强制设备进入 [Rockusb 模式] 或 [Maskrom 模式]。
2. 运行 [AndroidTool]。
3. 打开 "Upgrade Firmware" 制表页。
4. 点击 "Firmware" 按钮， 这会弹出一个文件对话框来打开原始固件文件。
5. 固件版本号、loader 版本号和芯片信息会从固件中读取并显示。
6. 点击 "Upgrade" 按钮烧录。

### 烧写分区映像

 使用 [AndroidTool] 烧写[分区映像]的步骤如下：

1. 强制设备进入 [Rockusb 模式] 或 [Maskrom 模式]。
2. 运行 `AndroidTool`。
3. 打开 `Download Image` 制表页。
4. 保持表格第一行不变。
5. 鼠标右键点击其它行，在弹出菜单中选择 "Delete Item" ，重复直至删除第一行除外的所有行。
   ![](img/androidtool_del.png)
6. 鼠标右键点击表格，在弹出菜单中选择 "Add Item" 以便添加[分区映像]：
	* 选中第一个单元格上的复选框。
	* 填入 `parameter.txt` 中该分区的起始扇区作为烧写地址（如果是 [Maskrom 模式] 则须再加上 `0x2000`）。
	* 单击右侧空白单元格以便打开本地对应的[分区映像]文件。
	![](img/androidtool_add.png)
7. 点击 "Run" 按钮烧录。

**注意**：

- 您可以通过重复步骤 6 将多个分区映像烧写到闪存。
- 通过取消选中地址单元格前面的复选框，可以跳过此分区的烧写。
- 在 [Maskrom 模式] 中， `parameter.txt` 中分区的起始扇区必须再加上 `0x2000` 作为烧写地址。参见[分区偏移量](#partition-offset)一章以便使用脚本获取该地址。

## upgrade_tool

[upgrade_tool] 是 Linux 下用来烧写[原始固件]、[RK 固件]和[分区映像]到 eMMC 的工具。

[upgrade_tool] 是由 Rockchip 提供的闭源的命令行工具。

### 安装 upgrade_tool

首先到[这里](https://gitlab.com/TeeFirefly/RK3328-Nougat/blob/roc-rk3328-cc/RKTools/linux/Linux_Upgrade_Tool/Linux_Upgrade_Tool_v1.24.zip)下载 [upgrade_tool]，并安装到 Linux 系统上：

    unzip Linux_Upgrade_Tool_v1.24.zip
    cd Linux_UpgradeTool_v1.24
    sudo mv upgrade_tool /usr/local/bin
    sudo chown root:root /usr/local/bin/upgrade_tool
    sudo chmod 0755 /usr/local/bin/upgrade_tool

然后根据[此处](#udev)的说明去添加 `udev` 规则。这是为了让普通用户有权限烧写 Rockchip 设备。如果跳过这步，那么所有的烧写命令需要在前面加 `sudo ` 才能成功执行。

### 烧写原始固件

[原始固件]需要从 eMMC 的偏移地址为 0 的位置开始烧写。但在 [Rockusb 模式] 下所有 LBA 写入操作会偏移 0x2000 个扇区（即 LBA0 对应于存储设备上的第 0x2000 个扇区）。因此，开发板必须强制进入 [Maskrom 模式] 才能烧写[原始固件]。

使用 [upgrade_tool] 烧写[原始固件]的步骤如下：
1. 强制设备进入 [Maskrom 模式]。
2. 运行以下命令：
    ```
    upgrade_tool db           out/u-boot/rk3328_loader_ddr786_v1.06.243.bin
    upgrade_tool wl 0x0       out/system.img
    upgrade_tool rd           # reset device to boot
    ```

其中：
 - `rk3328_loader_ddr786_v1.06.243.bin` 是编译 `U-boot` 时复制进去的 loader 文件，也直接到[此处](https://github.com/rockchip-linux/rkbin/tree/master/rk33)下载 `rk3328_loader_xxx.bin` 文件。
 - `system.img` 是打包后的[原始固件]，也可以是官网上下载并解压后的[原始固件]文件。

### 烧写 RK 固件

使用 [upgrade_tool] 烧写 [RK 固件]的步骤如下：
1. 强制设备进入 [Rockusb 模式] 或 [Maskrom 模式]。
2. 使用 `upgrade_tool` 烧写 RK 固件：
    ```
    upgrade_tool uf /path/to/rk/firmware
    ```
### 烧写分区映像

取决开发板原有的固件，烧写[分区映像]到 eMMC 的指令会有所不同。

**原始固件**

如果开发板原有系统是[原始固件]，那么很可能使用了 `GPT` 分区方案。每个分区的预定义偏移量和大小可以在 SDK 里的 `build/partitions.sh` 中找到，可以参考[《分区偏移量》](#partition-offset)一章。

使用 [upgrade_tool] 烧写 [分区映像]的步骤如下：
1. 强制设备进入 [Maskrom 模式]
2. 使用 `upgrade_tool` 烧写分区映像：
    ```
    upgrade_tool db         out/u-boot/rk3328_loader_ddr786_v1.06.243.bin
    upgrade_tool wl 0x40    out/u-boot/idbloader.img
    upgrade_tool wl 0x4000  out/u-boot/uboot.img
    upgrade_tool wl 0x6000  out/u-boot/trust.img
    upgrade_tool wl 0x8000  out/boot.img
    upgrade_tool wl 0x40000 out/linaro-rootfs.img
    upgrade_tool rd         # reset device to boot
    ```

**RK 固件**

如果开发板原有系统是 [RK固件]，那么它使用 `parameter` 文件作为分区方案，这样就可直接使用分区名称来烧写分区映像：

1. 强制设备进入 [Rockusb 模式] 或 [Maskrom 模式] 。
2. 使用 `upgrade_tool` 烧写分区映像：
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

注意：
- `-b` 是 `boot` 分区的预定义缩写。如果没有缩写可用，请改为分区名称，例如上述例子中的 `resource`。
- 可根据 [《参数文件格式》](http://www.t-firefly.com/download/Firefly-RK3399/docs/Rockchip%20Parameter%20File%20Format%20Ver1.3.pdf) 这份文档的说明自定义内核参数和分区布局。分区布局更改后，必须先重新烧写该 `parameter` 文件，方可重新烧写受影响的相应分区。

### 常见问题

如果由于闪存问题而出现错误，可以尝试使用低格或擦除闪存：

    upgrade_tool lf   # 低格闪存
    upgrade_tool ef   # 擦除闪存

## rkdeveloptool

[rkdeveloptool] j是 Linux 下用来烧写[原始固件]和[分区映像]到 eMMC 的工具。它**不支持**烧写 [RK 固件]。

[rkdeveloptool] 是由 Rockchip 开发的命令行烧写工具，是闭源工具 [upgrade_tool] 的开源替代品。在日常使用中，一般都能取代 [upgrade_tool]。

### 安装 rkdeveloptool

首先是下载、编译和安装 [rkdeveloptool]：

    #install libusb and libudev
    sudo apt-get install pkg-config libusb-1.0 libudev-dev libusb-1.0-0-dev dh-autoreconf
    # clone source and make
    git clone https://github.com/rockchip-linux/rkdeveloptool
    cd rkdeveloptool
    autoreconf -i
    ./configure
    make
    sudo make install

然后根据[此处](#udev)的说明去添加 `udev` 规则。这是为了让普通用户有权限烧写 Rockchip 设备。如果跳过这步，则所有的烧写命令需要在前面加 `sudo ` 才能成功执行。

### 烧写原始固件

[原始固件]需要从 eMMC 的偏移地址为 0 的位置开始烧写。但在 [Rockusb 模式] 下所有 LBA 写入操作会偏移 0x2000 个扇区（即 LBA0 对应于存储设备上的第 0x2000 个扇区）。因此，开发板必须强制进入 [Maskrom 模式] 才能烧写[原始固件]。

使用 [rkdeveloptool] 烧写[原始固件]的步骤如下：
1. 强制设备进入 [Maskrom 模式]。
2. 运行以下命令：
    ```
    rkdeveloptool db           out/u-boot/rk3328_loader_ddr786_v1.06.243.bin
    rkdeveloptool wl 0x0       out/system.img
    rkdeveloptool rd           # reset device to boot
    ```

其中：
 - `rk3328_loader_ddr786_v1.06.243.bin` 是编译 `U-boot` 时复制进去的 loader 文件，也直接到[此处](https://github.com/rockchip-linux/rkbin/tree/master/rk33)下载 `rk3328_loader_xxx.bin` 文件。
 - `system.img` 是打包后的[原始固件]，也可以是官网上下载并解压后的[原始固件]文件。

### 烧写分区映像：

以下的说明**仅适用**于开发板原有系统是[原始固件]时的[分区映像]烧写。那么很可能使用了 `GPT` 分区方案。每个分区的预定义偏移量和大小可以在 SDK 里的 `build/partitions.sh` 中找到，可以参考[《分区偏移量》](#partition-offset)一章。

使用 [rkdeveloptool] 烧写[分区映像]的步骤如下：
1. 强制设备进入 [Maskrom 模式]。
2. 运行以下命令：
    ```
    rkdeveloptool db           out/u-boot/rk3328_loader_ddr786_v1.06.243.bin
    rkdeveloptool wl 0x40      out/u-boot/idbloader.img
    rkdeveloptool wl 0x4000    out/u-boot/uboot.img
    rkdeveloptool wl 0x6000    out/u-boot/trust.img
    rkdeveloptool wl 0x8000    out/boot.img
    rkdeveloptool wl 0x40000   out/linaro-rootfs.img
    rkdeveloptool rd           # reset device to boot
    ```

## udev

创建 `/etc/udev/rules.d/99-rk-rockusb.rules`，并插入以下内容[1](https://github.com/rockchip-linux/rkdeveloptool/blob/master/99-rk-rockusb.rules)。 如有必要，用实际 Linux 组替换 `users` 组：
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

重新加载 udev 规则：

    sudo udevadm control --reload-rules
    sudo udevadm trigger


<span id="partition-offset"></span>

## 分区偏移量

### GPT 分区

如果使用[原始固件]，那么系统很可能使用了 `GPT` 分区方案。每个分区的预定义偏移量和大小可以在 SDK 里的 `build/partitions.sh` 中找到。

分区映像的偏移量可以通过以下命令获得（假设位于 Firefly Linux SDK 的目录中）：

    (. build/partitions.sh ; set | grep _START | \
    while read line; do start=${line%=*}; \
    printf "%-10s 0x%08x\n" ${start%_START*} ${!start}; done )

会得到：

    ATF        0x00006000
    BOOT       0x00008000
    LOADER1    0x00000040
    LOADER2    0x00004000
    RESERVED1  0x00001f80
    RESERVED2  0x00002000
    ROOTFS     0x00040000
    SYSTEM     0x00000000

### parameter

如果使用[RK固件]，那么系统是使用 `parameter.txt` 文件来定义分区方案，该文件的格式参见文档[《参数文件格式》](http://www.t-firefly.com/download/Firefly-RK3399/docs/Rockchip%20Parameter%20File%20Format%20Ver1.3.pdf) 。

这里有一个 Linux 下的 Bash 脚本能列出 `parameter.txt` 中的分区偏移量：

```
#!/bin/sh

PARAMETER_FILE="$1"
[ -f "$PARAMETER_FILE" ] || { echo "Usage: $0 <parameter_file>"; exit 1; }

show_table() {
	echo "$1"
	echo "--------"
	printf "%-20s %-10s %s\n" "NAME" "OFFSET" "LENGTH"
	for PARTITION in `cat ${PARAMETER_FILE} | grep '^CMDLINE' | sed 's/ //g' | sed 's/.*:\(0x.*[^)])\).*/\1/' | sed 's/,/ /g'`; do
		NAME=`echo ${PARTITION} | sed 's/\(.*\)(\(.*\))/\2/'`
		START=`echo ${PARTITION} | sed 's/.*@\(.*\)(.*)/\1/'`
		LENGTH=`echo ${PARTITION} | sed 's/\(.*\)@.*/\1/'`
		START=$((START + $2))
		printf "%-20s 0x%08x %s\n" $NAME $START $LENGTH
	done
}

show_table "Rockusb Mode" 0
echo
show_table "Maskrom Mode" 0x2000
```

将这个脚本保存为 `/usr/local/bin/show_rk_parameter.sh` ，然后添加脚本的执行权限。

下面是一个显示 `RK3328 Android SDK` 中定义的分区偏移量的例子：

```
$ show_rk_parameter.sh device/rockchip/rk3328/parameter.txt 
Rockusb Mode
--------
NAME                 OFFSET     LENGTH
uboot                0x00002000 0x00002000
trust                0x00004000 0x00004000
misc                 0x00008000 0x00002000
baseparamer          0x0000a000 0x00000800
resource             0x0000a800 0x00007800
kernel               0x00012000 0x00010000
boot                 0x00022000 0x00010000
recovery             0x00032000 0x00010000
backup               0x00042000 0x00020000
cache                0x00062000 0x00040000
metadata             0x000a2000 0x00008000
kpanic               0x000aa000 0x00002000
system               0x000ac000 0x00300000
userdata             0x003ac000 -

Maskrom Mode
--------
NAME                 OFFSET     LENGTH
uboot                0x00004000 0x00002000
trust                0x00006000 0x00004000
misc                 0x0000a000 0x00002000
baseparamer          0x0000c000 0x00000800
resource             0x0000c800 0x00007800
kernel               0x00014000 0x00010000
boot                 0x00024000 0x00010000
recovery             0x00034000 0x00010000
backup               0x00044000 0x00020000
cache                0x00064000 0x00040000
metadata             0x000a4000 0x00008000
kpanic               0x000ac000 0x00002000
system               0x000ae000 0x00300000
userdata             0x003ae000 -
```

[Rockusb 模式]: bootmode.html#rockusb-mode
[Maskrom 模式]: bootmode.html#maskrom-mode
[原始固件]: started.html#raw_firmware_format
[RK 固件]: started.html#rockchip_firmware_format
[分区映像]: started.html#partition_image
[AndroidTool]: flash_emmc.html#androidtool
[upgrade_tool]: flash_emmc.html#upgrade-tool
[rkdeveloptool]: flash_emmc.html#rkdeveloptool
[安装 Rockusb 驱动]: flash_emmc.html#install-rockusb-driver
