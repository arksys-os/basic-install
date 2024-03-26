# Basic installation

To install a Linux distribution, you typically encounter the following components:
- Linux kernel (6.8.1-arch1-1 or 5.17.12), the customized Linux kernel
- Bootloader (GRUB, systemd)
- Init system (systemd, sysvinit, runit, OpenRC)
- File system (EXT4, BTRFS, ZFS)
- Display server (Xorg / Wayland)
- Package manager (Pacman, Apt, zypper, dnf)
- Desktop environment (KDE, GNOME, XFCE), which includes components like the window manager, display manager, file manager, viewer, etc.

Example of Linux distros:

| Distribución    | Linux kernel                   | Base system tools            | Bootloader      | Init system     | File system      | Display server | Package manager  | Desktop environment |
|-----------------|--------------------------------|------------------------------|-----------------|-----------------|------------------|----------------|------------------|---------------------|
| Arch Linux      | 6.8.1-arch1-1 (rolling-release)| archiso, base, base-devel    | GRUB, systemd   | systemd         | EXT4, BTRFS, ZFS | Xorg / Wayland | Pacman (PKGBUILD)| --                  |
| Debian          | 5.17.12 (fixed-release)        | glibc                        | GRUB, systemd   | systemd         | EXT4, BTRFS, ZFS | Xorg / Wayland | Apt (.deb)       | --                  |
| Fedora          | 5.19.12-200 (fixed-release)    | glibc                        | GRUB, systemd   | systemd         | EXT4, BTRFS      | Wayland        | DNF (.rpm)       | GNOME               |
| NixOS           | 5.17.12 (rolling-release)      | glibc                        | systemd         | systemd         | EXT4, BTRFS, ZFS | Xorg / Wayland | Nix (.nar)       | -- / GNOME          |
| Gentoo          | 5.17.12 (rolling-release)      | glibc                        | GRUB, systemd   | OpenRC, systemd | EXT4, BTRFS, ZFS | Xorg / Wayland | Portage (.ebuild)| --                  |
| Void Linux      | 6.3.12_1 (rolling-release)     | musl                         | GRUB            | runit           | EXT4, BTRFS      | Xorg / Wayland | xbps (.xbps)     | -- / XFCE           |
| Ubuntu          | 5.17.12 (fixed-release)        | glibc                        | GRUB            | systemd         | EXT4             | Wayland        | Apt, snap (.snap)| GNOME               |


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

## 1. Options to install Arch Linux
- 1.A. Install configured Linux distro (presaved config files) -> 🐧💾
- 1.B. Install Linux fromt the terminal (typping on the shell) -> 🐧🐢
- 1.C. Create your own Linux distro (base profile + calamares) -> 🐧🛠️🦑

### 1.A. Install configured Arch-based distro

Download and install live-Isos based with a preconfigured desktop environment. You can desktops like KDE, GNOME, XFCE, etc or customized the parts of the desktop with Window Managers / Wayland compositors (awesomeWM, qtile, hyprland) + bar managers (waybar).

- Arch-based with KDE:  [Garuda KDE Lite](https://iso.builds.garudalinux.org/iso/garuda/kde-lite/), [EndeavourOS](https://endeavouros.com/latest-release/), [CachyOS KDE](https://mirror.cachyos.org/ISO/kde/)

- Debian-based with KDE: [KDE Neon](https://neon.kde.org/), [Kubuntu](https://kubuntu.org/)

- RPM-based with KDE: [Fedora KDE](https://ftp.plusline.net/fedora/linux/releases/39/Spins/x86_64/iso/Fedora-KDE-Live-x86_64-39-1.5.iso), [Nobara Project](https://nobara-images.nobaraproject.org/Nobara-39-Official-2024-01-24.iso.sha256sum), [openSUSE KDE](https://download.opensuse.org/tumbleweed/iso/openSUSE-Tumbleweed-KDE-Live-x86_64-Current.iso)

> You also can use [ArcoLinux-D (Decision)](https://ftp.belnet.be/arcolinux/iso/v23.01.03/arcolinuxd-v23.01.03-x86_64.iso) an Arch-based distro where you can decide which packages to install with a GUI [calamares installer](https://calamares.io/)

### 1.B. Installation from Arch Linux terminal
There are various options to install Arch Linux with this configuration system:

#### 1.B.A. Shell mode: Arch in the shell
Install Arch Linux manually typing commands following the [Arch Wiki](https://wiki.archlinux.org/title/Installation_guide). You have to make pre-installation (internet connection, partition  and mount disks), install essential packages, configure system (fstam, chroot, timezone, localizaion, network, intramfs, root password) and reboot.

#### 1.B.B. Scripting mode: burn as a shell (BASH)
> **Warning** Modify the script installation [arksys-installation.sh](installation/archinstall/archisntall-config.sh) and execute with your consent. (Send feedback if you know how to improve it)

#### 1.B.C. CLI mode: archinstall
When you boot into Arch, you can run "archinstall" an installer with forms options. Watch this video installing Arch with archinstall in less than 5 mins:
[![Watch the video](/img/archinstall-video.png)](https://www.youtube-nocookie.com/embed/8mEjwn_AjuQ?start=146)
 
You can also use archinstall with preconfigured options stored in local or remote json `archinstall --config <path-to-json> --disk-layout <path-to-json> --creds <path-to-json>'.

```sh
# locally with
sudo pacman -S git
git clone https://github.com/arksys-os/arksys-basic-install.git
cd ~/arksys-basic-install/installation/archinstall && sh archinstall-config.sh

# remote with curl
curl -sL https://raw.githubusercontent.com/arksys-os/arksys_basic-install/main/installation/script/arksys.sh | bash

# remote with git
sudo pacman -S git
git clone https://github.com/arksys-os/arksys-basic-installation.git
cd ~/arksys-basic-installation/installation/script/ && sh arksys.sh
```

### C. Distro mode: Create you own Arch-based distro
Create your own Linux distro with a framework installer like calamares installer. You need to configure the base profile [archiso](https://wiki.archlinux.org/title/Archiso) profile for Archlinux or for Debian with [Debian live-build](https://salsa.debian.org/live-team/live-build) and configure the graphical installer with [calamares installer](https://calamares.io/). 


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
