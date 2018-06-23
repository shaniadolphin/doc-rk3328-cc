# Getting Started

[ROC-RK3328-CC] supports booting from the following storage devices:

- SD card
- eMMC

You need to flash the firmware file to the SD card or eMMC, in order to make the board bootable.

[SDCard Installer] is the officially recommended SD card flashing tool, which is derived from Etcher / Rock64 Installer. It implements one-stop downloading and flashing of firmware file, which makes life easy.

## Firmware Format

There are two firmware file formats:

- Raw Firmware
- RK Firmware

<span id="raw_firmware_format"></span>

[Raw Firmware], when flashing, is copied to the storage device bit by bit. It is the raw image of the storage device. [Raw Firmware] is flashed to the SD card in common cases, but it can also be flashed to the eMMC. There are lots of flashing tools available:

- To flash to the SD card:
    + GUI:
        * [SDCard Installer] (Linux/Windows/Mac)
        * [Etcher] (Linux/Windows/Mac)
    + CLI:
        * [dd] (Linux)
- To flash to the eMMC:
    + GUI:
        * [AndroidTool] (Windows)
    + CLI:
        * [upgrade_tool] (Linux)
        * [rkdeveloptool] (Linux)

<span id="rockchip_firmware_format"></span>

[RK Firmware], is packed in Rockchip's proprietary format, which is flashed to the eMMC via Rockchip's [upgrade_tool] (Linux) or [AndroidTool] (Windows). It is Rockchip's traditional packing format, commonly used in Rockchip Android firmware. [RK Firmware] of Android can also be flashed into SD card using [SD Firmware Tool].

Since [Raw Firmware] has wider audience, for simplicity, we **DO NOT PROVIDE** [RK Firmware] to download any more.

<span id="partition_image"></span>

[Partition Image], is the raw image of the partition. When you build the Android SDK, you'll get a list of `boot.img`, `kernel.img`, `system.img`, etc, which is called [Partition Image] and will be flashed to the corresponding partition. For example, `kernel.img` is to be flashed to `kernel` partition in the eMMC or SD card.

## Download & Flash

We recommend to use [SDCard Installer] to flash [Raw Firmware] to SD card.

If you are using tools other than [SDCard Installer], please download the [Raw Firmware] in the [Download Page](http://en.t-firefly.com/doc/download/34.html) first.

Here's the available OS list of firmware:

- Android 7.1.2
- Ubuntu 16.04
- Debian 9
- LibreELEC 9.0

**WARNING**: Only [Raw Firmware]s are available in the download page. We **DO NOT PROVIDE** [RK Firmware] any more.

Then choose the flashing tool according to your host PC's OS:

- To flash to the SD card:
    + GUI:
        * [SDCard Installer] (Linux/Windows/Mac)
        * [Etcher] (Linux/Windows/Mac)
    + CLI:
        * [dd] (Linux)
- To flash to the eMMC:
    + GUI:
        * [AndroidTool] (Windows)
    + CLI:
        * [upgrade_tool] (Linux)
        * [rkdeveloptool] (Linux)

## System Boot Up

Before system boots up, make sure you have:

- A bootable SD card or eMMC
- 5V2A power adapter
- Micro USB cable

Then follow the procedures below:

1. Pull the power adapter out of the power socket.
2. Use the micro USB cable to connect the power adapter and the board.
3. Plug in the bootable SD card or eMMC (NOT BOTH).
4. Plug in the optional HDMI cable, USB mouse or keyboard.
5. Check everything is okay, then plug the power adapter into the power socket to power on the board.

[ROC-RK3328-CC]: http://en.t-firefly.com/product/rocrk3328cc.html "ROC-RK3328-CC Official Website"
[Raw Firmware]: started.html#raw_firmware_format
[RK Firmware]: started.html#rockchip_firmware_format
[Partition Image]: started.html#partition_image
[SDCard Installer]: flash_sd.html#sdcard-installer
[Etcher]: flash_sd.html#etcher
[dd]: flash_sd.html#dd
[SD Firmware Tool]: flash_sd.html#sd-firmware-tool
[AndroidTool]: flash_emmc.html#androidtool
[upgrade_tool]: flash_emmc.html#upgrade-tool
[rkdeveloptool]: flash_emmc.html#rkdeveloptool
