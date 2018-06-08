# Windows 下的 eMMC 烧写

`AndroidTool` 是 Windows 下用来烧写固件和分区镜像到 eMMC 的工具。

使用 `AndroidTool` 之前， 应先安装 `Rockusb 驱动`。

## 安装 Rockusb 驱动

下载 [DriverAssistant]， 解压后运行里面的 `DriverInstall.exe`：

![](img/started_driverassistant.png)

点击 "驱动安装" 按钮安装驱动；如果想卸载驱动，则点击 "驱动卸载" 按钮。

若设备处于 [Rockusb 模式] 或 [Maskrom 模式]，在设备管理器中会出现`Rockusb Device`：

![](img/started_driverassistant_dev.png)

## 安装 AndroidTool

下载 [AndroidTool]，解压后运行里面的 `AndroidTool.exe`：

![](img/androidtool.png)

若设备处于 [Rockusb 模式]，状态行将显示 "Found One LOADER Device"。

若设备处于 [Maskrom 模式]，状态行将显示 "Found One MASKROM Device"。

## 烧写原始固件

原始固件需要从 eMMC 的偏移地址为 0 的位置开始烧写。但在 [Rockusb 模式] 下无法做到这点，因为在该模式下所有 LBA 写入操作会偏移 0x2000 个扇区（即 LBA0 对应于 存储设备上的 0x2000 扇区）。因此，开发板必须强制进入 [Maskrom 模式] 才能烧写原始固件。

安装 `Rockusb Driver` 和 `AndroidTool`后， 烧写原始固件的步骤如下：

1. 强制设备进入 [Maskrom 模式]。
2. 运行 `AndroidTool`。
3. 打开 `Download Image` 制表页。
4. 保持表格的第一行不变， 使用默认的 `MiniLoader` bin。
5. 点击第二行右侧的空白单元格，这会弹出一个文件对话框来打开原始固件文件
6. 点击 "Run" 按钮开始烧写

![](img/androidtool_flash_image.png)

## 烧写 Rockchip 固件

安装 `Rockusb 驱动` and `AndroidTool` 之后， 烧写 Rockchip 固件步骤为：

1. 强制设备进入 [Rockusb 模式] 或 [Maskrom 模式]。
2. 运行 `AndroidTool`。
3. 打开 `Upgrade Firmware` 制表页。
4. 点击 "Firmware" 按钮， 这会弹出一个文件对话框来打开原始固件文件。
5. 固件版本号、loader 版本号和芯片信息会从固件中读取并显示。
6. 点击 "Upgrade" 按钮烧录。

## 烧写分区镜像

安装 `Rockusb 驱动` and `AndroidTool` 后， 烧写分区镜像的步骤为：

1. 强制设备进入 [Rockusb 模式] 或 [Maskrom 模式]。
2. 运行 `AndroidTool`。
3. 打开 `Download Image` 制表页。
4. 保持表格第一行不变。
5. 通过从右键单击弹出式菜单中选择 "Delete Item" ，删除所有其他未使用的行。
   ![](img/androidtool_del.png)
6. 通过从右键单击弹出菜单中选择 "Add Item" 将分区镜像添加到闪存：

	* 选中第一个单元格上的复选框。
	* 填入 `parameter.txt` 中该分区的起始扇区作为烧写地址（如果是 [Maskrom 模式] 则须再加上 `0x2000`）。
	* 单击右侧空白单元格以便定位到对应的分区镜像文件。
	![](img/androidtool_add.png)
7. 点击 "Run" 按钮烧录。

**注意**：

- 您可以通过重复步骤 6 将多个分区镜像烧写到闪存。
- 通过取消选中地址单元格前面的复选框，可以跳过分区烧写。
- 在 [Maskrom 模式] 中， `parameter.txt` 中分区的起始扇区必须再加上 `0x2000` 作为烧写地址。

为了方便起见，下一章有查找偏移量的简易方法。

## 分区偏移量

这里有一个脚本能列出 `parameter.txt` 中的分区偏移量：

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


[AndroidTool]: http://www.t-firefly.com/share/index/listpath/id/acd8e1e37176fba5bf61fb7bf4503998.html
[DriverAssistant]: https://pan.baidu.com/s/1migPY1U#list/path=%2FPublic%2FDevBoard%2FROC-RK3328-CC%2FTools%2FRKTools%2Fwindows&parentPath=%2FPublic%2FDevBoard%2FROC-RK3328-CC
[Rockusb 模式]: bootmode.html#rockusb-mode
[Maskrom 模式]: bootmode.html#maskrom-mode
