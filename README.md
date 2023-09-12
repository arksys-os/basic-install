# Basic installation

## 0. Prerequisites
First you need to download the [ISO image](https://en.wikipedia.org/wiki/Optical_disc_image) of the Linux distribution that you want to install and write it to an optical medium like CD, DVD, pen-drive, HDD. 

- For disks just burn the ISO image into the CD / DVD / Blue-Ray with any [disk burning application](https://alternativeto.net/software/imgburn/).
- For USB drives (typical nowadays) you can use 'dd' a CLI utility on Unix to copy the ISO files into the USB drive. Or use GUI apps like [Ventoy](https://www.ventoy.net/en/index.html) that permit to have multiple ISO images on a single device and select the image to boot from a display menu, [BalenaEtcher](https://www.balena.io/etcher) (multiplatform) and [Rufus](https://rufus.ie/en/) (for Windows) programs that extract the ISO images to the USB drive.

If you use 'dd' command, you have to unmount your chooseable disk and format with a file system. There are various options for file systems (ext2, ext3, ext4, exfat, fat, vfat, minix, ntfs, msdos, xfs), but the most typical for OS are "fat" compatible with all systems, "ext4" for linux and "ntfs" for windows, "hfsplus" or "APFS" for macOS.

```sh
sudo fdisk –l         # find the disk to create the bootable system
umount /dev/sdb*      # unmount the disk
mkfs.vfat /dev/sdb –I
dd if=~/Downloads/arch.iso of=/dev/sdb bs=4M status=progress
```

Depending of your needs you can install:
- an Arch Linux system from scratch (1.A)
- an Arch-based system preconfigured (1.B)

## 1.A. 🥚 Install Arch from scratch
There are various options to install Arch Linux with this configuration system:

### 1.A.A. 🐍 Easy way: archinstall
When you boot into Arch, you can run "archinstall" an installer with forms options. Watch this video installing Arch with archinstall in less than 5 mins:
[![Watch the video](/img/archinstall-video.png)](https://www.youtube-nocookie.com/embed/8mEjwn_AjuQ?start=146)
 
You can also use archinstall with preconfigured options stored in local or remote json `archinstall --config <path-to-json> --disk-layout <path-to-json> --creds <path-to-json>'.
- From local, clone the repository with git and execute "archinstall-config.sh" that runs archinstall with preconfigured options:
```sh
sudo pacman -S git
git clone https://github.com/arksys-os/arksys-basic-install.git
cd ~/arksys-basic-install/installation/archinstall && sh archinstall-config.sh
```

> Similarly, you can use [ArcoLinux-D (Decision)](https://ftp.belnet.be/arcolinux/iso/v23.01.03/arcolinuxd-v23.01.03-x86_64.iso) an Arch-based distro where you can decide what packages to install with a GUI [calamares installer](https://calamares.io/) from XFCE as the live Desktop Environment.

### 1.A.B. 🐢 Expert mode: Arch in the shell
- Install Arch Linux manually typing commands following the [Arch Wiki](https://wiki.archlinux.org/title/Installation_guide). You have to make pre-installation (internet connection, partition  and mount disks), install essential packages, configure system (fstam, chroot, timezone, localizaion, network, intramfs, root password) and reboot.

### 1.A.C. 🐙 Pro mode: create script installation
> **Warning** Modify the script installation [arksys-installation.sh](installation/archinstall/archisntall-config.sh) and execute with your consent. (Send feedback if you know how to improve it)

- Download the script (with git or curl) and execute it with bash:
with curl:
```sh
curl -sL https://raw.githubusercontent.com/arksys-os/arksys_basic-install/main/installation/script/arksys.sh | bash
```
with git:
```sh
sudo pacman -S git
git clone https://github.com/arksys-os/arksys-basic-installation.git
cd ~/arksys-basic-installation/installation/script/ && sh arksys.sh
```

### 1.A.D. 🐧 + 🦑 Distro mode: Linux distro with calamares
Create your own Linux distro with a framework installer like calamares installer. You need to configure the [archiso](https://wiki.archlinux.org/title/Archiso) profile (for Archlinux) or [Debian live-build](https://salsa.debian.org/live-team/live-build) and the [calamares installer](https://calamares.io/) with modules and branding. 

## 1.B. 🐾 Install configured Arch Linux

### Arch Linux with KDE
If you don't want to configure Arch Linux, you can also install preconfigured Linux distributions with a desktop environment like KDE. Here are the most relevant:

- Arch-based with KDE: [ArcoLinux KDE](https://sourceforge.net/projects/arcolinux-community-editions/files/plasma/), [Garuda KDE Lite](https://iso.builds.garudalinux.org/iso/garuda/kde-lite/), [EndeavourOS KDE](https://endeavouros.com/latest-release/), [Manjaro KDE](https://download.manjaro.org/kde/22.0/manjaro-kde-22.0-221224-linux61.iso), [Xerolinux](https://sourceforge.net/projects/xerolinux/), [CachyOS KDE](https://mirror.cachyos.org/ISO/kde/), [Artix KDE](http://ftp.ntua.gr/pub/linux/artix-iso/?C=S;O=D)

> I recommend an Arch with basic KDE config like my distro [ArkSys]https://github.com/arksys-os/arksys-iso) or [Garuda KDE Lite](https://iso.builds.garudalinux.org/iso/garuda/kde-lite/)

### Other distros with KDE
- Debian-based with KDE: [KDE Neon](https://neon.kde.org/), [Kubuntu](https://kubuntu.org/), [MX Linux KDE](https://sourceforge.net/projects/mx-linux/files/Final/KDE/), [Nitrux KDE](https://sourceforge.net/projects/nitruxos/files/Release/ISO/)

- RPM-based with KDE: [Fedora KDE](https://ftp.plusline.net/fedora/linux/releases/37/Spins/x86_64/iso/), [Nobara Project (KDE)](https://nobara-images.nobaraproject.org/Nobara-37-Official-2023-04-02.iso), [openSUSE KDE](https://download.opensuse.org/tumbleweed/iso/openSUSE-Tumbleweed-DVD-x86_64-Current.iso?mirrorlist)

> Take in mind KDE is a desktop environment, you can choose others such as GNOME, XFCE, Deepin, etc or Window Managers (qtile, awesomeWM, hyprland) + essential packages.

## 2. [Post-installation](https://github.com/arksys-os/arksys_post-install)
Go to the GitHub repo [ArkSys-OS/Post-installation](https://github.com/arksys-os/arksys_post-install).
---

## Resources
- [Arch Linux - Download](https://archlinux.org/download/)
- [Arch Linux installation guide](https://wiki.archlinux.org/title/Installation_guide)
- [Archinstall - man page](https://man.archlinux.org/man/extra/archinstall/archinstall.1.en)
- [Archinstall · GitHub](https://github.com/archlinux/archinstall)
- [ALIS](https://github.com/picodotdev/alis/)
- [Bootloader: GRUB](https://wiki.archlinux.org/title/GRUB)
- [Distributions featuring KDE Plasma](https://community.kde.org/Distributions)
- [KDE - ArchWiki](https://wiki.archlinux.org/title/KDE)
- [10 Ways KDE is just better - YouTube](https://www.youtube.com/watch?v=3nX1YEQg5Z0)
- [Por estas 5 razones Arch Linux es simplemente MEJOR](https://www.youtube.com/watch?v=hk4t1RhnKVo)
