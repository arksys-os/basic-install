# Basic installation

To install a Linux distribution, you typically encounter the following components:
- Linux kernel (6.8.1-arch1-1 or 5.17.12), the customized Linux kernel
- Bootloader (GRUB, systemd)
- Init system (systemd, sysvinit, runit, OpenRC)
- File system (EXT4, BTRFS, ZFS)
- Display server (Xorg / Wayland)
- Package manager (Pacman, Apt, zypper, dnf)
- Desktop environment (KDE, GNOME, XFCE), which includes components like the window manager, display manager, file manager, viewer, etc.
- Ways to install: via CLI (text-based), via GUI (Calamares installer, debian-installer, ananconda, ubiquity, void-installer)

Example of Linux distros:

Here's an improved and more detailed version of the table with added information on the installer types and desktop environments (DEs) for each Linux distribution:

| Linux Distro    | Base System Tools                 | Bootloader          | Init System     | File System      | Display Server | Package Manager  | DE preinstalled  | Installer (Type)                          |
|-----------------|-----------------------------------|---------------------|-----------------|------------------|----------------|------------------|------------------|-------------------------------------------|
| Arch Linux      | linux + archiso, base, base-devel | GRUB / systemd-boot | systemd         | EXT4, BTRFS, ZFS | Xorg / Wayland | pacman (PKGBUILD)| --               | Text-based CLI / archinstall (script CLI) |
| Debian          | linux + glibc                     | GRUB                | systemd         | EXT4, BTRFS, ZFS | Xorg / Wayland | apt (.deb)       | --               | Debian Installer (GUI and Text-based CLI) |
| Fedora          | linux + glibc                     | GRUB                | systemd         | EXT4, BTRFS      | Wayland        | dnf (.rpm)       | GNOME, KDE       | Anaconda (GUI)                            |
| NixOS           | linux + glibc                     | GRUB                | systemd         | EXT4, BTRFS, ZFS | Xorg / Wayland | nix (.nar)       | GNOME, KDE       | Text-based CLI / Calamares Installer GUI  |
| Gentoo          | linux + glibc                     | GRUB / systemd-boot | OpenRC, systemd | EXT4, BTRFS, ZFS | Xorg / Wayland | portage (.ebuild)| --               | Text-based CLI                            |
| Void Linux      | linux + musl                      | GRUB                | runit           | EXT4, BTRFS      | Xorg / Wayland | xbps (.xbps)     | --, XFCE         | Text-based CLI / Void-installer GUI       |
| Ubuntu          | linux + glibc                     | GRUB                | systemd         | EXT4             | Wayland        | apt, snap (.snap)| GNOME            | Ubiquity GUI                              |

## 0. Prerequisites
First you need to download the [ISO image](https://en.wikipedia.org/wiki/Optical_disc_image) of the Linux distribution that you want to install and write it to an optical medium like CD, DVD, pen-drive, HDD. 

- For disks just burn the ISO image into the CD / DVD / Blue-Ray with any [disk burning application](https://alternativeto.net/software/imgburn/).
- For USB drives (typical nowadays) you can use 'dd' a CLI utility on Unix to copy the ISO files into the USB drive. Or use GUI apps like [Ventoy](https://www.ventoy.net/en/index.html) that permit to have multiple ISO images on a single device and select the image to boot from a display menu, [BalenaEtcher](https://www.balena.io/etcher) (multiplatform) and [Rufus](https://rufus.ie/en/) (for Windows) programs that extract the ISO images to the USB drive.

These are the commands for using `dd`:
```sh
sudo fdisk –l         # find the disk to create the bootable system
umount /dev/sdb*      # unmount the disk
mkfs.vfat /dev/sdb –I
dd if=~/Downloads/arch.iso of=/dev/sdb bs=4M status=progress
```

## 1. Options to install Linux
- 1.A. Install configured Linux distro (presaved config files) -> 🐧 💾
- 1.B. Install Linux fromt the terminal (typping on the shell) -> 🐧 🐢
- 1.C. Create your own Linux distro (base profile + installer) -> 🐧 🛠️

### 1.A. Install base distro with KDE

- Arch-based with KDE:  [Garuda KDE Lite](https://iso.builds.garudalinux.org/iso/garuda/kde-lite/), [EndeavourOS](https://endeavouros.com/), [CachyOS KDE](https://mirror.cachyos.org/ISO/kde/), [ArcoLinux KDE](https://sourceforge.net/projects/arconetpro/files/arcoplasma/)
- Debian-based with KDE: [KDE Neon](https://neon.kde.org/), [Kubuntu](https://kubuntu.org/getkubuntu/)
- RPM-based with KDE: [Fedora KDE](https://ftp.plusline.net/fedora/linux/releases/39/Spins/x86_64/iso/Fedora-KDE-Live-x86_64-39-1.5.iso), [Nobara](https://nobara-images.nobaraproject.org/Nobara-39-Official-2024-01-24.iso), [openSUSE KDE](https://download.opensuse.org/tumbleweed/iso/openSUSE-Tumbleweed-KDE-Live-x86_64-Current.iso)

### 1.B. Installation from terminal
Normally Linux is intalled via terminal typing and executing commands. Is the simplest ways to develop, but the most time consuming to the user.

Here are some examples of the installation guide of some Linux distro: [installation guide of the Arch Wiki](https://wiki.archlinux.org/title/Installation_guide), [installation guide of Gentoo wiki](https://wiki.gentoo.org/wiki/Installation), [installation guide of the NixOS wiki](https://nixos.wiki/wiki/NixOS_Installation_Guide)

Typically you need to do pre-installation (internet connection, partition and mount disks), install essential packages, configure system (fstam, chroot, timezone, localizaion, network, intramfs, root password) and reboot.

To automate the installation process we can create an use scripts on the terminal using BASH. 

- You can use scripts created by the community like **archinstall** a script installer for Arch Linux with selection options in a formulary. Watch this video installing Arch in less than 5 mins:
[![Watch the video](/img/archinstall-video.png)](https://www.youtube-nocookie.com/embed/8mEjwn_AjuQ?start=146)
 
You can specify preconfigured options stored in a json to the archinstall command, `archinstall --config <path-to-json> --disk-layout <path-to-json> --creds <path-to-json>'. Example to install Arch with KDE, ext4 file system and extra pks.

Install locally with git:
```sh
sudo pacman -S git
git clone https://github.com/arksys-os/arksys-basic-install.git
cd ~/arksys-basic-install/installation/archinstall && sh archinstall-config.sh
```

Install remottely with curl:
```sh
curl -sL https://raw.githubusercontent.com/arksys-os/arksys_basic-install/main/installation/script/arksys.sh | bash
```

- Or you can create you own scripts. Here's an example I've made to install automatize the Arch Linux installation, check [arksys.sh](installation/script/arksys.sh)

### C. Distro mode: Create you own Linux distro
Create your own Linux distro with a framework installer. You need to configure the base profile for Arch Linux ([archiso](https://wiki.archlinux.org/title/Archiso) profile), for Debian with [Debian live-build](https://salsa.debian.org/live-team/live-build) and configure a graphical installer like [calamares installer](https://calamares.io/). 

## 2. [Post-installation](https://github.com/arksys-os/arksys_post-install)
Continue reading on [ArkSys-OS/Post-installation](https://github.com/arksys-os/arksys_post-install).

---

## Resources
- [Arch Linux - Download](https://archlinux.org/download/)
- [Arch Linux installation guide](https://wiki.archlinux.org/title/Installation_guide)
- [Archinstall - man page](https://man.archlinux.org/man/extra/archinstall/archinstall.1.en)
- [Archinstall · GitHub](https://github.com/archlinux/archinstall)
- [ALIS · GitHub](https://github.com/picodotdev/alis/)
- [Bootloader: GRUB](https://wiki.archlinux.org/title/GRUB)
- [Distributions featuring KDE Plasma](https://community.kde.org/Distributions)
- [KDE - ArchWiki](https://wiki.archlinux.org/title/KDE)
- [10 Ways KDE is just better - YouTube](https://www.youtube.com/watch?v=3nX1YEQg5Z0)
- [Por estas 5 razones Arch Linux es simplemente MEJOR](https://www.youtube.com/watch?v=hk4t1RhnKVo)
