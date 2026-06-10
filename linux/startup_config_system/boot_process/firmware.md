# Firmware

IBM compatible workstations/servers utilize builtin firmware to control how the OS is started.
older systems utilize BIOS (Basic Input/Output System) and newer systems use UEFI (Unified Extensible Firmware Interface)

they maintain the system hardware settings and launch an installed OS


## BIOS

BIOS has a simplistic menu interface where users could change some settings to control how the system found hardware 
and what BIOS should use to load/start the OS
BIOS can only load the first 512 byte sector (MBR) into memory during the initial boot stage which is not enough space to load an Operating Sytem
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
the BIOS looks for the MBR and will read the program stored there in to memory.
the MBR bootloader typically loads a secondary bootloader which will then load the Operating System kernel.

### BIOS Boot Process

1. BIOS runs POST
2. BIOS checks the BOOT order
3. BIOS reads the first 512 bytes of the MBR
4. BIOS executes the bootloader/program found in the MBR
5. the bootloader will load any additional steps to load the OS kernel
