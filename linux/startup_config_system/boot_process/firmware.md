# Firmware

IBM compatible workstations/servers utilize builtin firmware to control how the OS is started.
older systems utilize BIOS (Basic Input/Output System) and newer systems use UEFI (Unified Extensible Firmware Interface)

they maintain the system hardware settings and launch an installed OS


## BIOS

BIOS has a simplistic menu interface where users could change some settings to control how the system found hardware 
and what BIOS should use to load/start the OS
BIOS can only load the first 512 byte sector (MBR - Master Boot Record) into memory during the initial boot stage which is not enough space to load an Operating Sytem
    BIOS loads this 512 byte sector from the boot device into memory and executes it

the BIOS runs a `bootloader` program (a small program that initializes the necessary hardware to find and run the OS)
often found at another location of the same hard drive but can be on an seperate internal/external storage device
the bootloader program uses a configuration file with details about how to find the OS. In some cases there is a menu
if multiple Operating Systems are available.

the BIOS must know where to find the `bootloader` on an installed storage device such as
- Internal/External drive
- CD/DVD drive
- USB Stick
- ISO
- Network Server (NFS, HTTP, FTP)

if using a hard drive an MBR must be present which designates where on the drive the `bootloader` is present.
the MBR is the first sector of the disk containing a small bootloader program.
the BIOS looks for the MBR and will read the program stored there into memory.
the MBR bootloader typically loads a secondary bootloader which will then load the Operating System kernel.

### BIOS Boot Process

1. BIOS runs POST
2. BIOS checks the BOOT order
3. BIOS reads the first 512 bytes of the MBR
4. BIOS executes the bootloader/program found in the MBR
5. the bootloader will load any additional steps to load the OS kernel

## UEFI
As systems grew more complex and BIOS became too limited, Intel introduced the Extensible Firmware Interface (EFI) in 1998. 
This later evolved into the Unified Extensible Firmware Interface (UEFI), which is now used by most desktop and server systems.

Instead of relying on a tiny boot sector at the start of a disk, UEFI uses a dedicated partition called the EFI System Partition (ESP) to store bootloader programs.
- The ESP uses the FAT32 filesystem.
- It can store multiple bootloaders for different operating systems.
- Bootloaders are standard executable files with the .efi extension.

On Linux systems, the ESP is typically mounted at /boot/efi, and bootloaders such as GRUB or systemd‑boot place their .efi files there.
A Shim bootloader can also be used which is a UEFI executable digitally signed by a trusted authority (like Microsoft) so it can execute while Secure Boot is active. Once shim has been validated
it then loads a second bootloader such as GRUB with the linux distros own cert chain of trust

UEFI includes a built‑in boot manager that decides which bootloader to run.
To appear in the boot menu, a bootloader’s .efi file must be registered in UEFI NVRAM.
If it is not registered, UEFI will not list it — even if the file exists on the ESP.

Most modern Operating Systems register their bootloaders automatically:

Windows Registers:
\EFI\Microsoft\Boot\bootmgfw.efi  
Entry name: Windows Boot Manager

Ubuntu / Debian / Fedora / Arch Registers:
\EFI\ubuntu\grubx64.efi (or distro equivalent)
Entry name: Ubuntu, Fedora, etc.

For mainstream operating systems like Windows, Ubuntu, Fedora, and most Linux distros, the installer automatically creates the UEFI boot entry so it appears in the firmware’s boot menu.

### Final
Once the firmware finds and runs the `bootloader`, its job is done and the bootloader takes over.
