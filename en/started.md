# Getting Started

[ROC-RK3328-CC] supports booting from the following storage devices:
 - SD card
 - eMMC

You need to flash the firmware to SD card or eMMC, so as to make the board bootable after power up. 

[SDCard Installer] is the officially recommended SD card flashing tool, which is based on Etcher / Rock64 Installer, and implements one-stop downloading and flashing of firmware file. It makes life easy.

## Firmware Format

There are two firmware file formats:
 - raw firmware
 - Rockchip firmware
 
The `raw firmware`, when flashing, is copied to the storage device bit by bit. It is the raw image of the storage device. `Raw firmware` is flashed to SD card in common cases, but it can also be flashed to eMMC. There are lots of flashing tools available:
 - To flash to SD card:
   + CLI: dd (Linux)
   + GUI: Etcher (Linux/Windows/Mac), [SDCard Installer] (Linux/Windows/Mac), WinDD (Windows)
 - To flash to eMMC:
   + CLI: update_tool (Linux)，rkdeveloptool (Linux)
   + GUI: android_tool (Windows)

The `Rockchip firmware`, is packed in Rockchip's proprietary format, which is flashed to eMMC via Rockchip's `update_tool` in Linux or `android_tool` in Windows. It is Rockchip's traditional packing format, commonly used in Rockchip Android firmware. The Rockchip Android firmware can also be flashed into SD card using [SD_Firmware_Tool].

Besides, when you build the Android SDK, you'll get a list of `boot.img`, `kernel.img`, `system.img`, etc, which is called `partition image file` and will be flashed to the corresponding partition. For example, `kernel.img` is to be flashed to `kernel` partition of eMMC or SD card.

## Download & Flash

It is strongly recommend to use [SDCard Installer] for easy downloading and flashing firmware to SD card with a few clicks.

Here come the instructions of manual downloading and flashing. First, check [this page](http://www.t-firefly.com/doc/download/page/id/34.html) to download the `raw firmware` you want. Here's the OS support list:
 - Android 7.1.2
 - Ubuntu 16.04
 - Debian 9
 - LibreELEC 9.0

**Warning**: The download page provides no `Rockchip firmware` any more.

To flash `raw firmware`, follow the steps below:
- To flash to SD card: [Windows](flash_sd_windows.html) / [Linux](flash_sd_linux.html)
- To flash to eMMC: [Windows](flash_emmc_windows.html) / [Linux](flash_emmc_linux.html)

If you want to build your own firmware, please check the Developer's Guide.

## System Boot Up

Before system boots up, make sure you have:
 - A bootable SD card or eMMC
 - 5V2A power adapter
 - Micro USB cable

Then follow the procedures below:

 1. Pull power adapter out of power socket.
 2. Use the micro USB cable to connect power adapter and main board.
 3. Plug in bootable SD card or eMMC (NOT BOTH).
 4. Plug in optional HDMI cable, USB mouse or keyboard.
 5. Check everything is okay, then plug the power adapter into the power socket to power on the board.

[ROC-RK3328-CC]: http://en.t-firefly.com/product/rocrk3328cc.html "ROC-RK3328-CC Official Website"
[SDCard Installer]: http://www.t-firefly.com/share/index/index/id/acd8e1e37176fba5bf61fb7bf4503998.html
[Etcher]: https://etcher.io
[SD_Firmware_Tool]: https://pan.baidu.com/s/1migPY1U#list/path=%2FPublic%2FDevBoard%2FROC-RK3328-CC%2FTools%2FSD_Firmware_Tool&parentPath=%2FPublic%2FDevBoard%2FROC-RK3328-CC
[AndroidTool]: https://pan.baidu.com/s/1migPY1U#list/path=%2FPublic%2FDevBoard%2FROC-RK3328-CC%2FTools%2FAndroidTool&parentPath=%2FPublic%2FDevBoard%2FROC-RK3328-CC
