# Arch Linux Manual Installation Guide

This guide outlines the steps for installing Arch Linux on a desktop system.
Note: This process assumes the installation medium has already been prepared.

## Initial Setup

Connect to your network and establish a remote session if necessary.

### Network Configuration

Use iwd to connect to your Wi-Fi: Setup

```bash
iwctl
station wlan0 connect <SSID>
# Enter password when prompted
```

### Remote Access (Optional)

Set a root password and identify your IP address to SSH into the machine:

```bash
passwd
ip a
# From another machine:
ssh root@<ip_address>
```

## System Configuration

prepare for the installation

### Set Console Keyboard

the default is US; set it to UK for the correct keymapping

```bash
loadkeys uk
```

### Verify Boot Mode

confirm that the system has booted in UEFI mode. A return value of 64 indicates a 64-bit UEFI system

```bash
cat /sys/firmware/efi/fw_platform_size
```

## System Clock

check that the system clock is synchronized to prevent package signature verification failures

```bash
# to check the current status
timedatectl
# if its not syn'd
timedatectl set-ntp true
```

## Disk Partitioning

identify the target disk with `fdisk -l`
on my system, I have 2 disks one for **Windows** and another for **Linux**

**Partition Layout**

```
Partition	Device	        Type	            Size
/boot	    /dev/nvme0n1p1	EFI System	        1G
[SWAP]	    /dev/nvme0n1p2	Linux Swap	        4G
/	        /dev/nvme0n1p3	Linux Filesystem	75G
/home	    /dev/nvme0n1p4	Linux Filesystem	Remaining
```

Use cfdisk to manage your partitions:

- Clear existing partitions: Use D to delete existing data on the target drive.
- Create Partitions: Use N to create new partitions.
- Assign Types: Use T to set types (EFI System for the boot partition; Linux Swap for the swap partition).
- Write Changes: Ensure you write your changes to the disk before exiting.

## Format Partitions

Once partitioned, format the drives for their use

```bash
# Format EFI System Partition
mkfs.fat -F 32 /dev/nvme0n1p1

# Format and activate Swap
mkswap /dev/nvme0n1p2

# Format Root and Home partitions
mkfs.ext4 /dev/nvme0n1p3
mkfs.ext4 /dev/nvme0n1p4
```

## Mount File Systems

```bash
# mount the root volume
mount /dev/nvme0n1p3 /mnt

# create the boot directory
mkdir /mnt/boot
# mount the EFI system partition
mount /dev/nvme0n1p1 /mnt/boot

# create the home directory
mkdir /mnt/home
# mount the home partition
mount /dev/nvme0n1p4 /mnt/home

# enable the swap mount
swapon /dev/nvme0n1p2
```

## Installation

### Install Mirrors

packages are installed from mirror servers. see `/etc/pacman.d/mirrorlist`
the higher a mirror is in the list, the more priority it has when downloading a package
use the `reflector` program to select the best five

```bash
reflector --latest 5 --protocol https --age 12 --sort rate --save /etc/pacman.d/mirrorlist
```

### Essential Packages

the only config carried over to the live environment to the installed system is the `mirrorlist`
there are some packages to install that will be carried over

```bash
pacstrap -K /mnt base linux linux-firmware amd-ucode networkmanager vim vi man-db man-pages
```

## Config the Sytem

### Fstab

to get the filesystems mounted on startup we generate an fstab

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

now directly interact with the new system

```bash
arch-chroot -S /mnt
```

## New System

### Time

set the correct timezone

```bash
ln -sf /usr/share/zoneinfo/Europe/London /etc/localtime
# generate /etc/adjtime
hwclock --systohc
```

### Localization

edit the /etc/locale.gen file and uncomment `en_GB.UTF-8`

```bash
locale-gen
```

edit /etc/locale.conf with the following line: `LANG=en_GB.UTF-8`
edit /etc/vconsole.conf with `KEYMAP=uk` to persist the keyboard layout
edit /etc/hostname with your chosen hostname

### Set Root Password and Add User

```bash
# change root password
passwd

# add your own user
useradd -m -G wheel,users <username>
# change the new users password
passwd <username>

# make sure the wheel group is available
visudo
# uncomment the line with %wheel
```

## Boot Loader Setup

I chose Limine for this.

```bash
# install required packages
pacman -S limine efibootmgr

# create the limine boot directory and copying the boot file
mkdir -p /boot/EFI/arch-limine
cp /usr/share/limine/BOOTX64.EFI /boot/EFI/arch-limine

# use efibootmgr to setup a Limine entry for the boot loader
efibootmgr --create --disk /dev/nvme0n1 --part 1 --label "Arch Linux Limine Boot Loader" --loader '\EFI\arch-limine\BOOTX64.EFI' --unicode

# create the Limine config file
vim /boot/EFI/arch-limine/limine.conf
```

```text
/boot/EFI/arch-limine/limine.conf
timeout: 5

/Arch Linux
    protocol: linux
    path: boot():/vmlinuz-linux
    cmdline: root=UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx rw quiet
    module_path: boot():/amd-ucode.img
    module_path: boot():/initramfs-linux.img
```

create the pacman hook to copy the new BOOTX64.EFI file after updating

```text
/etc/pacman.d/hooks/99-limine.hook
[Trigger]
Operation = Install
Operation = Upgrade
Type = Package
Target = limine

[Action]
Description = Deploying Limine after upgrade...
When = PostTransaction
Exec = /usr/bin/cp /usr/share/limine/BOOTX64.EFI esp/EFI/arch-limine/
```

## Reboot

```bash
# exit out of chroot
exit
# manually unmount all the partitions
umount -R /mnt
#
reboot
```
