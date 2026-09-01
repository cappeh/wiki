# GRUB Legacy
GRUB Legacy was designed to simplify creating boot menus and passing options to the Kernel. 
This lets us select multiple Kernel's or Operating Systems using both a menu interface or interactive shell.
The menu interface is configured to provide options for the Kernel or OS and the interactive shell to customize boot commands on the fly
both the menu and interactive shell use commands that control features of the bootloader.

## Configure
using the interactive menu, we need to use special grub menu commands to tell it what options to show.
GRUB legacy uses the standard text configuration file "menu.lst" to store menu commands and stored in the "/boot/grub" directory
Red-Hat derived linux distros use the "grub.conf" file instead of "menu.lst" for the configuration.

There are two sections in the configuration file:
Global definitions
OS boot definitions

global definitions define commands to control the boot menu and must appear first in the configuration file
to define a value for the command, list the value as a command-line parameter
the following are typical settings

default 0
timeout 0
color white/blue yellow/blue

typical menu options

```bash
color           foreground/background colours to use in the boot menu. first pair are for normal menu entries, second pair is the colours for the selected menu entry
default         the default menu option to select
fallback        a secondary menu option if the default fails
hiddenmenu      does not diplay the menu selection options
splashimage     an image file to use as the background for the boot menu
timeout         the amount of time to wait to select a menu option before using the default
```

the OS definitions follow where each OS should have its own definition.

```bash
title           first line for each boot definition (this appears in the boot menu)
root            defines the disk and partition (where the GRUB "/boot" directory partition is on the system) - defined as (hddrive, partition)
kernel          defines the kernel image stored in "/boot" to load
initrd          defines the intial RAM disk file containing the drivers necessary for the kernel to interact with system hardware
rootnoverify    defines non-linux boot partitions such as Windows
```

(hd0, 0): this references the first partition on the first hard drive of the system
(hd0, 1): references the second partition of the first hard drive on the system.

`initrd` defines a file that is mounted by the kernel at boot time as a RAM disk allowing access the hardware or filesystems not compiled into the kernel (using specialised hardware or filesystems)
its located in "/boot" and usually called initrd.img-kversion (kversion is the kernel version number)

sample file:

```conf
default 0
timeout 10
color white/blue yellow/blue

title Ubuntu Linux
root (hd1,0)
kernel (hd1,0)/boot/vmlinuz
initrd /boot/initrd

title Windows
rootnoverify (hd0,0)
```

the `z` in `vmlinuz` means that the kernel file is compressed using `bzImage` otherwise its `vmlinux`

## Install GRUB Legacy
with a legacy configuration file, the program must also be installed in the MBR (Master Boot Record)
a single command is used to install the program

`grub-install <partition>`

using linux format:
`grub-install /dev/sda`

using grub format:
`grub-install '(hd0)'`

if using chainloading, to install on a boot sector of a partition instead of MBR we need to specific the partition again

`grub-install /dev/sda1`
`grub-install '(hd0,0)'`

### Interacting with GRUB
we will eventually during the boot process, see the menu options defined in the configuration file.
if we wait for the timeout, the default will be selected or use the arrow keys to select another.
we can also edit boot options but using the arrow keys and pressing `E`
we can then use the `B` key to boot the system using the new values
`C` allows us to enter an interactive shell to submit commands on the fly
