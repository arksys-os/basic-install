*ArkSys* is an Arch Linux distribution with vanilla KDE (K Desktop Environment).
![](/img/arksys.png)

# Why to choose this system?
After installing multiple Linux distros with different desktop environments and  window managers, I always end up with a similar configuration and visual design. So this is the type of OS that I recommend to use in a daily drive for most computers. Here you can view all [basic Linux software](./linux-software.md) that I have selected.

ArkSys = ArchLinux + Vanilla KDE (by default)

## [Kernel = Linux](https://kernel.org/)
I have chosen Linux as the Operating System beacause is the biggest open-source project, is private, secure, very customazible and there is a big community that can help you . Also is the only one that can compite against the proprietary system such as Windows and macOS. And in case you don't know, Linux is only the Kernel of the Operating System that communicates with the computer hardware, the rest of the OS is constrcuted on top of that.

## Linux distribution = [Arch-Linux](https://archlinux.org/)
Arch Linux is a very lightway Linux distribution with around 300 packages preinstalled and with 800 MiB of space (perfect for old CDs). It has a big community that grows every day and it has a package manager (pacman) very useful with tons of packages from [arch](https://archlinux.org/packages/) and [AUR](https://aur.archlinux.org/). There are others distributions like Debian, Fedora, openSUSE, Slackware, Red hat, Alpine Linux, Void Linux...
The difference between Linux distributions is just the preselect packages: linux-kernel modifiead, the package manager and the desktop environment / window manager (DE / WM).

## Desktop Environment = [KDE](https://kde.org/)
Most of us are familiar with Windows or perhaps MacOS before using Linux, and these operating systems are great in terms of usability, but not for privacy.To achieve a similar design and good usability, the best option is to use a desktop environment such as KDE (K Desktop Environment) or, for something simpler, GNOME or XFCE. KDE is under development and is the biggest open-source project that develop free user applications. Also it has mouse gestures on laptops and is customizable to the exteme.

![](/img/linux-architecture.jpg)


# System configuration
Here is my system configuration, you can define yours.

## Config
- Language-system: en-US
- Encoding: utf-8
- Timezone: UTC

## User
You can change the username or password after the installation, or put another in the script.
- username: ark
- hostname: arksys
- password (root and user): 123

## Hardware compatibility
- Graphic card: NVDIA / AMD (AMD better for Wayland)
- Processor: AMD / Intel

## Partitions
| Mount point | Partition                   | Partition type        | Size                    | Type           |
| ----------- | --------------------------- | --------------------- | ----------------------- | -------------- |
| `/mnt/boot` | `/dev/efi_system_partition` | EFI system partition  | 203 MiB                 | FAT32          |
| `[SWAP]`    | `/dev/swap_partition`       | Linux swap            | 2203 MiB                | SWAP           |
| `/mnt`      | `/dev/root_partition`       | Linux x86-64 root     | Remainder of the device | BTRFS or EXT4  |

- For BTRFS partition use subvolumes (@.snapshots, @, @home, @log, @var, @pkg)
- For EXT4, one partition root (/) with (/bin /boot /dev /etc /home /lib /lib64 /mnt /opt /proc /root /run /sbin /srv /sys /tmp /usr /var) or separate /home

## Linux software
- Linux-distribution: arch-linux
- Kernel: linux
- Bootloader: grub
- Shell: bash
- Sys-init: systemd
- Drivers: 
    - Audio-drivers: pìpewire
    - Video-drivers: vulkan-radeon / nvidia, nvdia-open
- Desktop environment: KDE-plasma
    - Display-server: xorg / wayland
    - File-manager: dolphin
    - Terminal: konsole
    - Terminal-text-editor: nano vim
    - Text-editor-GUI: kate
    - Theme: breeze
    - Window-manager: kwin
- Extra-packages: firefox libreoffice-fresh

---

# How to install

## 0. Prerequisites
Download a [ISO image](https://en.wikipedia.org/wiki/Optical_disc_image) of a Linux distribution and write it to an optical medium like CD, DVD, pen-drive, HDD. 

- For disk you just need to burn the ISO image into the CD / DVD / Blue-Ray with any [disk burning application](https://alternativeto.net/software/imgburn/).
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

## 1.A. Install Arch from scratch
There are various options to install this configuration system inside Arch Linux:

### A. Easy way: archinstall
When you boot into Arch, you can run "archinstall" an installer with forms options, in less than 5 minutes you can start your system. Watch this video using archinstall:
[![Watch the video](/img/archinstall-video.png)](https://www.youtube-nocookie.com/embed/8mEjwn_AjuQ?start=146)
 
You can also use archinstall with preconfigured options stored in local or remote json `archinstall --config <path-to-json> --disk-layout <path-to-json> --creds <path-to-json>'.
- From local, clone the repository with git and execute "archinstall-config.sh" that runs archinstall with preconfigured options:
```sh
sudo pacman -S git
git clone https://github.com/david7ce/arksys.git
sh ./arksys/scripts/archinstall-config.sh
```

> Similarly, you can use [ArcoLinux-D (Decision)](https://ftp.belnet.be/arcolinux/iso/v23.01.03/arcolinuxd-v23.01.03-x86_64.iso) an Arch-based distro where you can decide what packages to install with a GUI [calamares installer](https://calamares.io/) from XFCE as the live Desktop Environment.

### B. Expert mode: Arch way
- Install Arch Linux manually typing commands following the [Arch Wiki](https://wiki.archlinux.org/title/Installation_guide)

### C. Create and execute your own script
> **Warning** Modify the script installation "arksys-installation.sh" and execute with your consent. (Send feedback if you know how to improve it)
> The script is an automatization of the manual installation you can decide when is executed.

- Download the script and execute, to download it you can use curl or git and for executing use bash: 
```sh
# curl -sL raw.githubusercontent.com/david7ce/arksys/master/scripts/arksys-installation.sh | bash

# sudo pacman -S git
# git clone https://github.com/david7ce/arksys.git
# cd ~/arksys/installation/basic/ && sh arksys.sh
```


## 1.B. Install Linux distros with KDE

If you don't want to configure Arch Linux, you can also install preconfigured Linux distributions in this case KDE. Here are the most relevant:

- Arch-based with KDE: [ArcoLinux KDE](https://sourceforge.net/projects/arcolinux-community-editions/files/plasma/), [Garuda KDE Lite](https://iso.builds.garudalinux.org/iso/garuda/kde-lite/), [EndeavourOS KDE](https://endeavouros.com/latest-release/), [Manjaro KDE](https://download.manjaro.org/kde/22.0/manjaro-kde-22.0-221224-linux61.iso), [Xerolinux](https://sourceforge.net/projects/xerolinux/), [CachyOS KDE](https://mirror.cachyos.org/ISO/kde/), [Artix KDE](http://ftp.ntua.gr/pub/linux/artix-iso/?C=S;O=D)

- Debian-based with KDE: [KDE Neon](https://neon.kde.org/), [Kubuntu](https://kubuntu.org/), [MX Linux KDE](https://sourceforge.net/projects/mx-linux/files/Final/KDE/), [Nitrux KDE](https://sourceforge.net/projects/nitruxos/files/Release/ISO/)

- RPM-based with KDE: [Fedora KDE](https://ftp.plusline.net/fedora/linux/releases/37/Spins/x86_64/iso/), [Nobara Project (KDE)](https://nobara-images.nobaraproject.org/Nobara-37-Official-2023-04-02.iso), [openSUSE KDE](https://download.opensuse.org/tumbleweed/iso/openSUSE-Tumbleweed-DVD-x86_64-Current.iso?mirrorlist)

- Other Linux distros with KDE: [Alt Workstation KDE](https://getalt.org/en/alt-kworkstation/), [KaOS - SourceForge](https://sourceforge.net/projects/kaosx/files/ISO/KaOS-2022.12-x86_64.iso/download), [Rosa Linux KDE](https://mirror.rosalinux.ru/rosa/rosa2021.1/iso/ROSA.FRESH.12/plasma5/)

> I recommend [Garuda KDE Lite](https://iso.builds.garudalinux.org/iso/garuda/kde-lite/), very similar to my taste.


## 2. [Post-installation](https://github.com/ArkSys-linux/arksys-post-install)

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

