# Linux 下 SD 卡的烧写

## 烧写原始固件

目前还没有 [SDCard Installer] 的Linux发行版。

不过可以使用你喜欢的烧写工具例如 [Etcher]，或者直接使用 `dd` 。

首先，插入SD卡，如果被文件管理器自动挂载，则将其卸载 。

然后通过检查内核的日志查找 SD 卡的设备文件:
> dmesg | tail

如果设备文件为 `/dev/mmcblk0`，使用 `dd` 命令去烧录:
> sudo dd if=/path/to/your/raw/firmware of=/dev/mmcblk0 conv=notrunc

`dd` 不会显示进度，可以使用另一个工具 `pv` 来完成进度条的显示

首先安装 `pv`:
> sudo apt-get install pv

然后添加 `pv` 到管道显示进度:
> pv -tpreb /path/to/your/raw/firmware | sudo dd of=/dev/mmcblk0 conv=notrunc

## 烧写 RK 固件

在 Linux 中没有将 RK固件烧写到 SD 卡的工具。

但可以解包 Rockchip 格式的固件，并将所有分区映像重新打包成原始格式固件，然后使用上一节中的方法继续去烧写。

[SDCard Installer]: http://www.t-firefly.com/share/index/index/id/acd8e1e37176fba5bf61fb7bf4503998.html

