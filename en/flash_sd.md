# Flashing to the SD Card

We will introduce how to flash the firmware to the SD card. If not explicitly stated, the following firmware is referred to the [Raw Firmware]. Read about [firmware format](started.md#firmware-format) if of any doubt.

We recommend to use [SDCard Installer] to flash the [Raw Firmware] to the SD card.

If you are using tools other than [SDCard Installer], please download the [Raw Firmware] in the [Download Page] first.

Here's the available OS list of firmware:

- Android 7.1.2
- Ubuntu 16.04
- Debian 9
- LibreELEC 9.0

**WARNING**: Only [Raw Firmware]s are available in the download page. We **DO NOT PROVIDE** [RK Firmware] any more.

## Preparing the SD Card

Please read this good article about [how to prepare a SD card](https://docs.armbian.com/User-Guide_Getting-Started/#how-to-prepare-a-sd-card) first, to make sure that you have a **good, reliable and fast** SD card, which is of essential importance for system stability.

## Flashing Tools

Please choose the flashing tool according to your host PC OS:

- To flash to the SD card
    + GUI:
        * [SDCard Installer] (Linux/Windows/Mac)
        * [Etcher] (Linux/Windows/Mac)
    + CLI:
        * [dd] (Linux)

## SDCard Installer

The easiest way to flash the [Raw Firmware] is to use the official [SDCard Installer], a handy firmware flashing tool derived from Etcher / Rock64 Installer. It saves time to search for available firmwares for your device. You just need to select the board, choose firmware, plugin in the SD card, and finally click the flash button, which is simple and straightforward.

**Instructions**:

1. Download [SDCard Installer] from the [Download Page].
2. Install and run:
    + Windows: Extract the archive file and run the setup executable inside. After installation, run [SDCard Installer] **as administrator** from the start menu.
    + Linux: Extract the archive file and run the `.AppImage` file inside.
    + Mac: Double click the `.dwg` file, install to the system or run directly.
3. Click the "Choose an OS" button, and select "ROC-RK3328-CC" in the "Please select your device" combobox.
4. A list of available firmware is updated from the network and presented to you, as illustrated below:

    ![](img/started_sdcard-installer.png)

5. Choose an firmware OS, and click "OK" button. To flash local firmware, drag it from your local drive and drop to [SDCard Installer].
6. Plug in the SD card. It should be automatically selected. If there are multiple SD cards, click the "Change" button to choose one.
7. Click the "Flash!" button. [SDCard Installer] will start to download the firmware, flash to the SD card, and verify the content. Please wait patiently.

    ![](img/started_sdcard-installer_flashing.png)

**Note**:

- To run [SDCard Installer] with proper permission in Windows, you need to right click the shortcut and select **Run as administrator**.
- Sometimes, when the progress reaches to 99% or 100%, an error of unmounting the SD card may occur, which can be ignored and does no harm to the data flashed to the SD card.

    ![](img/started_sdcard-installer_umount_fail.png)

- The downloaded firmware will be saved to the local directory, which will be reused the next time you flash the same firmware again. The download directory can be set by clicking the setting icon in the bottom left of the main window and changing the "Download Location:" field.

## Etcher

Compared with [SDCard Installer], [Etcher] lacks of firmware list and download. But its code is newer. If you have any flashing problem with the [SDCard Installer], you can try [Etcher], reusing the firmware file in the download directory of [SDCard Installer].

[Etcher] can be downloaded from the [Etcher official site](https://etcher.io). Installation and usage are similiar with [SDCard Installer].

## dd

[dd] is a commonly used command line tool in Linux.

First, plug in the SD card, and unmount it if it is automatically mounted by the file manager.

Then find the device file of the SD card by checking kernel log:

    dmesg | tail

If the device file is `/dev/mmcblk0`, use the following command to flash:

    sudo dd if=/path/to/your/raw/firmware of=/dev/mmcblk0 conv=notrunc

Flashing takes lots time and the command above does not show the progress. We can use another tool `pv` to do this job.

First install `pv`:

    sudo apt-get install pv

Then add `pv` to the pipe to report progress:

    pv -tpreb /path/to/your/raw/firmware | sudo dd of=/dev/mmcblk0 conv=notrunc

## SD Firmware Tool

**NOTE**: This section is about how to flash [RK Firmware] of Android to the SD card.

First, you will need to download [SD Firmware Tool] from the [SD Firmwware Tool Download Page](https://pan.baidu.com/s/1migPY1U#list/path=%2FPublic%2FDevBoard%2FROC-RK3328-CC%2FTools%2FSD_Firmware_Tool&parentPath=%2FPublic%2FDevBoard%2FROC-RK3328-CC) and extract it.

After extraction, in the directory of [SD Firmware Tool], edit `config.ini` by changing the 4th line from `Selected=1` to `Selected=2`, in order to select English as the default user interface language.

Run `SD_Firmware_Tool.exe`:

![](img/started_sdfirmwaretool.png)

1. Plug in the SD card.
2. Select the SD card from the combo box.
3. Check on the "SD Boot" option.
4. Click "Firmware" button, and select the firmware in the file dialog.
5. Click "Create" button.
6. A warning dialog will show up. By making sure you have the right SD card device selected, select "Yes" to continue.
7. Wait for the operation to complete, until the info dialog shows up.

    ![](img/started_sdfirmwaretool_done.png)

8. Plug out the SD card.

[Getting Started]: started.md
[FAQ]: faq.md
[Serial Debug]: debug.md
[Building Linux Root Filesystem]: linux_build_rootfilesystem.md
[Contact]: resource.md#community
[Raw Firmware]: started.md#raw-firmware-format
[RK Firmware]: started.md#rk-firmware-format
[Partition Image]: started.md#partition-image
[SDCard Installer]: flash_sd.md#sdcard-installer
[Etcher]: flash_sd.md#etcher
[dd]: flash_sd.md#dd
[SD Firmware Tool]: flash_sd.md#sd-firmware-tool
[AndroidTool]: flash_emmc.md#androidtool
[upgrade_tool]: flash_emmc.md#upgrade-tool
[rkdeveloptool]: flash_emmc.md#rkdeveloptool
[Rockusb Mode]: flash_emmc.md#rockusb-mode
[Maskrom Mode]: flash_emmc.md#maskrom-mode
[Rockusb Driver]: flash_emmc.md#rockusb-driver
[ROC-RK3328-CC]: http://en.t-firefly.com/product/rocrk3328cc.html "ROC-RK3328-CC Official Website"
[Download Page]: http://en.t-firefly.com/doc/download/34.html
[Forum]: http://bbs.t-firefly.com/
[Facebook]: https://www.facebook.com/TeeFirefly
[Google+]: https://plus.google.com/u/0/communities/115232561394327947761
[Youtube]: https://www.youtube.com/channel/UCk7odZvUrTG0on8HXnBT7gA
[Twitter]: https://twitter.com/TeeFirefly
[Shop]: http://shop.t-firefly.com/
[USB Serial Adapter]: http://shop.t-firefly.com/goods.php?id=32
[5V2A US Adapter]: http://shop.t-firefly.com/goods.php?id=68
[eMMC Flash]: http://shop.t-firefly.com/goods.php?id=69
[Storage Map]: http://opensource.rock-chips.com/wiki_Partitions#Default_storage_map
