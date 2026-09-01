# Boot Process
the boot process could be split into 3 parts

1. Workstation Firmware starts and checks the hardware with the POST (Power On Self Test)
   then looks for a bootloader program to run from a bootable drive

2. The bootloader runs and determines which Linux kernel program to load.

3. The kernel program loads into memory and starts the background programs required for the system to operate
   such as Graphical Desktop Manager for Desktops or Web/Database services for a Server

we can view the boot process by watching the system console as the system is booting.
there is a lot of information as the system detects hardware and software - this is quick
if you cant see it press either ESC or CTRL+ALT+F1

you can review the boot messages using the `dmesg` command.
most Linux Distros copy the kernel messages in to a `ring buffer` in memory `kernel ring buffer`
 - this is circular and set to a predetermined size so old messages are overwritten

some Linux Distros also store the boot messages in a log file usually in the `/var/log` folder usually `/var/log/boot.log`

```bash
[linux_lab@localhost ~]$ dmesg
[    0.000000] Booting Linux on physical CPU 0x0000000000 [0x610f0000]
[    0.000000] Linux version 5.14.0-611.55.1.el9_7.aarch64 (mockbuild@iad1-prod-build-aarch001.bld.equ.rockylinux.org) (gcc (GCC) 11.5.0 20240719 (Red Hat 11.5.0-11), GNU ld version 2.35.2-67.el9_7.1) #1 SMP PREEMPT_DYNAMIC Tue May 12 18:53:59 UTC 2026
[    0.000000] The list of certified hardware and cloud instances for Enterprise Linux 9 can be viewed at the Red Hat Ecosystem Catalog, https://catalog.redhat.com.
[    0.000000] KASLR disabled due to lack of seed
[    0.000000] efi: EFI v2.7 by EDK II
[    0.000000] efi: ACPI 2.0=0xfc080000 SMBIOS 3.0=0xfc020000 MEMATTR=0xfe67e218 MOKvar=0xff5b0000 MEMRESERVE=0xfbc04298 
[    0.000000] secureboot: Secure boot disabled
[    0.000000] ACPI: Early table checksum verification disabled
[    0.000000] ACPI: RSDP 0x00000000FC080000 000024 (v02 VMWARE)
[    0.000000] ACPI: XSDT 0x00000000FC070000 000054 (v01 VMWARE VMWVBSA! 20201221 VMW  00000001)
[    0.000000] ACPI: FACP 0x00000000FC060000 000114 (v06 VMWARE VMWVBSA! 20201221 VMW  00000001)
[    0.000000] ACPI: DSDT 0x00000000FC030000 000ED4 (v01 VMWARE VMWVBSA! 01343F06 INTL 20130823)
[    0.000000] ACPI: GTDT 0x00000000FC050000 000068 (v03 VMWARE VMWVBSA! 20201221 VMW  00000001)
[    0.000000] ACPI: MCFG 0x00000000FC040000 00003C (v01 VMWARE VMWVBSA! 20201221 VMW  00000001)
[    0.000000] ACPI: APIC 0x00000000FC000000 00010C (v04 VMWARE VMWVBSA! 20201221 VMW  00000001)
[    0.000000] ACPI: SSDT 0x00000000FBFE0000 000073 (v02 VMWARE VMWVBSA! 20201221 VMW  00000001)
[    0.000000] ACPI: PPTT 0x00000000FBFC0000 0000B8 (v02 VMWARE VMWVBSA! 20201221 VMW  00000001)
[    0.000000] NUMA: Failed to initialise from firmware
[    0.000000] NUMA: Faking a node at [mem 0x0000000080000000-0x00000000ffffffff]
[    0.000000] NUMA: NODE_DATA [mem 0xff9ecf00-0xff9f9fff]
[    0.000000] Zone ranges:
[    0.000000]   DMA      [mem 0x0000000080000000-0x00000000ffffffff]
[    0.000000]   DMA32    empty
[    0.000000]   Normal   empty
[    0.000000]   Device   empty
[    0.000000] Movable zone start for each node
[    0.000000] Early memory node ranges
[    0.000000]   node   0: [mem 0x0000000080000000-0x00000000fbcdcfff]
[    0.000000]   node   0: [mem 0x00000000fbcdd000-0x00000000fbfbffff]
[    0.000000]   node   0: [mem 0x00000000fbfc0000-0x00000000fc01ffff]
[    0.000000]   node   0: [mem 0x00000000fc020000-0x00000000fc02ffff]
[    0.000000]   node   0: [mem 0x00000000fc030000-0x00000000fc08ffff]
[    0.000000]   node   0: [mem 0x00000000fc090000-0x00000000fc133fff]
[    0.000000]   node   0: [mem 0x00000000fc134000-0x00000000ff40ffff]
[    0.000000]   node   0: [mem 0x00000000ff410000-0x00000000ff49ffff]
[    0.000000]   node   0: [mem 0x00000000ff4a0000-0x00000000ff5affff]
[    0.000000]   node   0: [mem 0x00000000ff5b0000-0x00000000ff5cffff]
[    0.000000]   node   0: [mem 0x00000000ff5d0000-0x00000000ffffffff]
[    0.000000] Initmem setup node 0 [mem 0x0000000080000000-0x00000000ffffffff]
[    0.000000] crashkernel reserved: 0x00000000e1a00000 - 0x00000000f1a00000 (256 MB)
[    0.000000] psci: probing for conduit method from ACPI.
[    0.000000] psci: PSCIv0.2 detected in firmware.
[    0.000000] psci: Using standard PSCI v0.2 function IDs
[    0.000000] psci: Trusted OS migration not required
[    0.000000] percpu: Embedded 33 pages/cpu s98024 r8192 d28952 u135168
[    0.000000] pcpu-alloc: s98024 r8192 d28952 u135168 alloc=33*4096
[    0.000000] pcpu-alloc: [0] 0 [0] 1 
[    0.000000] Detected PIPT I-cache on CPU0
[    0.000000] CPU features: detected: GIC system register CPU interface
[    0.000000] CPU features: detected: Spectre-v4
[    0.000000] CPU features: detected: Spectre-BHB
[    0.000000] alternatives: applying boot alternatives
[    0.000000] Kernel command line: BOOT_IMAGE=(hd0,gpt2)/vmlinuz-5.14.0-611.55.1.el9_7.aarch64 root=/dev/mapper/rl-root ro crashkernel=1G-4G:256M,4G-64G:320M,64G-:576M rd.lvm.lv=rl/root rd.lvm.lv=rl/swap rhgb quiet
[    0.000000] Unknown kernel command line parameters "rhgb BOOT_IMAGE=(hd0,gpt2)/vmlinuz-5.14.0-611.55.1.el9_7.aarch64", will be passed to user space.
[    0.000000] Dentry cache hash table entries: 262144 (order: 9, 2097152 bytes, linear)
[    0.000000] Inode-cache hash table entries: 131072 (order: 8, 1048576 bytes, linear)
[    0.000000] Fallback order for Node 0: 0 
[    0.000000] Built 1 zonelists, mobility grouping on.  Total pages: 516096
```
