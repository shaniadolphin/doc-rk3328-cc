# Windows 下 SD 卡的烧写

## 烧写原始固件

在 Windows 下烧写原始固件最轻松的方式就是使用官方的 [SDCard Installer]，一个衍生自 [Etcher] 的方便易用的固件烧写工具。

[SDCard Installer] 节省了搜索开发板可用固件的时间。你只需要选择板型、固件，之后插入 SD 卡，并点击烧写按钮即可完成整个写卡工作，实在简单又方便。

1. 运行 [SDCard Installer]。
2. 点击 "Choose an OS" 按钮， 在 "Please select your device" 组合框中选择 "ROC-RK3328-CC"。 可用的固件列表将从网络更新，如下图所示:
![](img/started_sdcard-installer.png)
4. 选择一个系统，点击 "OK" 按钮。亦可以选择本地的一个固件文件， 而不是从网上下载。
5. 拔出 SD 卡。
6. 点击 "Flash!" 按钮，等待。
![](img/started_sdcard-installer_flashing.png)

**注意**: 有时，当进度达到 99％ 或 100％ 时，可能会出现卸载 SD 卡的错误，这可以忽略，并且不会损坏烧写到 SD 卡的数据：
![](img/started_sdcard-installer_umount_fail.png)
 
## 烧写 RK 固件

本节介绍如何将 RK固件烧写到 SD 卡。

首先，需要下载并解压 [SD_Firmware_Tool]。

运行 `SD_Firmware_Tool.exe`:
![](img/started_sdfirmwaretool.png)

1. 插入 SD 卡。
2. 从组合框中选择 SD 卡。
3. 勾选 "SD Boot" 选项。
4. 点击 "Firmware" 按钮， 在文件对话框中选择固件。
5. 点击 "Create" 按钮。
6. 然后会显示警告对话框，选择 "是" 来确保选择了正确的SD卡设备。
7. 等待操作完成，直到信息对话框出现。
![](img/started_sdfirmwaretool_done.png)
8. 拔出 SD 卡。

[Etcher]: https://etcher.io
[SD_Firmware_Tool]: https://pan.baidu.com/s/1migPY1U#list/path=%2FPublic%2FDevBoard%2FROC-RK3328-CC%2FTools%2FSD_Firmware_Tool&parentPath=%2FPublic%2FDevBoard%2FROC-RK3328-CC
[AndroidTool]: https://pan.baidu.com/s/1migPY1U#list/path=%2FPublic%2FDevBoard%2FROC-RK3328-CC%2FTools%2FAndroidTool&parentPath=%2FPublic%2FDevBoard%2FROC-RK3328-CC
[SDCard Installer]: http://www.t-firefly.com/share/index/index/id/acd8e1e37176fba5bf61fb7bf4503998.html
[DriverAssistant]: https://pan.baidu.com/s/1migPY1U#list/path=%2FPublic%2FDevBoard%2FROC-RK3328-CC%2FTools%2FRKTools%2Fwindows&parentPath=%2FPublic%2FDevBoard%2FROC-RK3328-CC
