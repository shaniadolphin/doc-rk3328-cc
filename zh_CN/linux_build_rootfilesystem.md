# 编译 Debian 根文件系统

## 准备编译系统
```bash
git clone https://github.com/FireflyTeam/rk-rootfs-build.git
cd rk-rootfs-build
sudo apt-get install binfmt-support qemu-user-static
sudo dpkg -i ubuntu-build-service/packages/*
sudo apt-get install -f
```

## 编译根文件系统

使用 ubuntu-build-service 编译基础 Debian 系统:
```bash
VERSION=stretch TARGET=desktop ARCH=armhf ./mk-base-debian.sh
```

# 编译 Ubuntu 根文件系统

环境:

- Ubuntu 16.04 64位

安装依赖包:

    sudo apt-get install qemu qemu-user-static binfmt-support debootstrap

下载 Ubuntu core:

    wget -c http://cdimage.ubuntu.com/ubuntu-base/releases/16.04.1/release/ubuntu-base-16.04.1-base-arm64.tar.gz

创建一个大小为 1000M 的根文件系统镜像文件，并使用 ubuntu 的基础 tar 文件填充:

    fallocate -l 1000M rootfs.img
    sudo mkfs.ext4 -F ROOTFS rootfs.img 
    mkdir mnt 
    sudo mount rootfs.img mnt
    sudo tar -xzvf ubuntu-base-16.04.1-base-arm64.tar.gz -C mnt/
    sudo cp -a /usr/bin/qemu-aarch64-static mnt/usr/bin/

`qemu-aarch64-static` 能使 chroot 在 amd64 主机系统变成为 arm64 文件系统

Chroot 到新的文件系统中去并初始化:

    sudo chroot mnt/

    # 这里可以修改设置
    USER=firefly
    HOST=firefly

    # 创建用户
    useradd -G sudo -m -s /bin/bash $USER
    passwd $USER
    # enter user password
    
    # Hostname & Network
    echo $HOST /etc/hostname
    echo "127.0.0.1    localhost.localdomain localhost" > /etc/hosts
    echo "127.0.0.1    $HOST" >> /etc/hosts
    echo "auto eth0" > /etc/network/interfaces.d/eth0
    echo "iface eth0 inet dhcp" >> /etc/network/interfaces.d/eth0
    echo "nameserver 127.0.1.1" > /etc/resolv.conf
    
    # 使能串口
    ln -s /lib/systemd/system/serial-getty\@.service /etc/systemd/system/getty.target.wants/serial-getty@ttyS0.service 

    # 安装包
    apt-get update
    apt-get upgrade
    apt-get install ifupdown net-tools network-manager
    apt-get install udev sudo ssh
    apt-get install vim-tiny

卸载文件系统:

    sudo umount rootfs/

Credit: [bholland](https://forum.armbian.com/topic/6850-document-about-compiling-a-kernel-and-rootfs-for-the-firefly-boards/)

## 参考
 - [http://opensource.rock-chips.com/wiki_Distribution](http://opensource.rock-chips.com/wiki_Distribution)
