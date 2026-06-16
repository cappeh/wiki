# Linux Bootloader
a bootloader bridges the gap between a system firmware (BIOS / UEFI) and the Linux OS kernel. The main bootloaders:
- LILO
- GRUB Legacy (GRand Unified Bootloader)
- GRUB2
- systemd-boot

LILO was the original with the configuration in a single file at "/etc/lilo.conf"
this is very limited and was used with the BIOS startup.

GRUB was developed in 1999 to replace LILO as a more robust and configurable option and became the default for BIOS or UEFI
GRUB2 was a complete rewrite in 2005 and supports more advanced features such as loading Hardware Driver Modules

[Grub Legacy](./bootloaders/grub_legacy.md#grub-legacy)
