# Filesystems

Linux uses filesystems to manage data stored on storage devices
a filesystem uses a method of maintaining a map to locate each file on a storage device

Windows manages files and folders by assigning drive letters to its storage devices with the main storage device being assigned `C:`
A Windows path is typically `C:\Users\<user>\Documents\somefile.txt`
the file is located in the Documents folder of the user account stored on the disk paritioned with the label `C:`

Windows tells us what physical device the file is stored on. Linux on the otherhand uses a **Virtual Filesystem** which contains filepaths
from all installed storage devices packed into a single directory structure

## Virtual Directory

the **virtual filesystem** contains a single base directory called **root** which lists files and folders beneath it similar to Windows
`/home/<user>/Documents/test.txt`
the path just states that the test.txt file exists in the Documents directory fro the user. There is no indication of which drive the file is stored on unlike Windows

Linux makes physical storage devices accessible through the virtual directory using **mount points**
A **mount point** is a folder in the virtual directory that acts as an access point for a storage device or filesystem

You could for example have 2 hard drives, the first could be associated with the root `/` of the virtual directory and the other drive is mounted at `/home`
once the second drive is mounted to the virtual directory, files and folders stored on the drive become available

## FHS (Filesystem Hierarchy Standard)

Since Linux sotres everything in the **virtual directory** it can become messy which led to the **FHS** which defines core folder names and locations
that should be present on every system and what data should be stored in each folder


| Folder | Description |
|---|---|
| `/bin` | Executable programs necessary for the system to run in single-user mode |
| `/boot` | Contains bootloader files used to boot the system |
| `/dev` | Device files such as physical storage devices |
| `/etc` | System service configuration files |
| `/home` | User data files |
| `/lib` | Libraries required by executable programs |
| `/media` | Used as a mount point for external/removable drives |
| `/mnt` | Also used as a mount point for removable drives |
| `/opt` | Contains data for third-party programs |
| `/proc` | Provides kernel and process information as files that are updated in real time |
| `/root` | The home directory of the root user account |
| `/sbin` | Executable programs required by the system |
| `/sys` | Provides device, driver, and some kernel information as files updated in real time |
| `/tmp` | Contains temporary files created by system users |
| `/usr` | Contains data for standard Linux programs |
| `/usr/bin` | Contains local user programs and data |
| `/usr/local` | Contains data for programs unique to the local installation |
| `/usr/sbin` | Contains data for system programs and data |
| `/var` | Files whose contents are expected to change frequently, such as log files |



