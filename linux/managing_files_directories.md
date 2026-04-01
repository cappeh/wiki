# Files and Directories
files are stored in a single directory structure called the `Virtual Directory`
the single `virtual` directory contains files from all the computers storage devices merging them into a single directory 
the base directory is called the `root directory` AKA `root (/)` 

## Viewing Files
the `ls` (list) command is used to view a files name and their `metadata`
`metadata` is information that describes and provides additional details about data

basic ls syntax

```
ls [option] [file]
```

options AKA `switches` can be used to provide different kinds of metadata of the file --> these are optional
the file can either be a single file or a directory to see the metadata for a file or for files within the given directory --> also optional

```
[linux_lab@localhost ~]$ ls
Desktop  Documents  Downloads  find_results.txt  Music  Pictures  ps.txt  Public  Templates  Videos

[linux_lab@localhost ~]$ pwd
/home/linux_lab
```
ls without any options or filenames will display the files and sub-directories within the `Present Working Directory (PWD)`
the `PWD` is your login processes current location within the `virtual directory`

 -a | --all | display all file and directory names including hidden files             
 -d | --directory | show the directories own metadata instead of its contents         
 -F | --classify | classify each file type with an indicator (/ after a name to indicate directory)             
 -i | --inode | display file and sub directory name with their index number            
 -l | N/A | display file or sub dir metadata such as file type, file access permissions, hard link count, file owner and group, modification date and time             
 -R | N/A | show a directories contents and recursively show all sub-directories content within the original directory tree

```
[linux_lab@localhost ~]$ ls -l
total 84
drwxr-xr-x. 2 linux_lab linux_lab     6 Feb  5 15:34 Desktop
drwxr-xr-x. 2 linux_lab linux_lab     6 Feb  5 15:34 Documents
drwxr-xr-x. 2 linux_lab linux_lab     6 Feb  5 15:34 Downloads
-rw-r--r--. 1 linux_lab linux_lab 79883 Mar 27 14:28 find_results.txt
drwxr-xr-x. 2 linux_lab linux_lab     6 Feb  5 15:34 Music
drwxr-xr-x. 2 linux_lab linux_lab     6 Feb  5 15:34 Pictures
-rw-r--r--. 1 linux_lab linux_lab   303 Mar 27 13:02 ps.txt
drwxr-xr-x. 2 linux_lab linux_lab     6 Feb  5 15:34 Public
drwxr-xr-x. 2 linux_lab linux_lab     6 Feb  5 15:34 Templates
drwxr-xr-x. 2 linux_lab linux_lab     6 Feb  5 15:34 Videos
```
the `ls -lh` command displays extra metadata about each file or directory and will display the file size in a human readable format

```
[linux_lab@localhost ~]$ ls -lh
total 84K
drwxr-xr-x. 2 linux_lab linux_lab   6 Feb  5 15:34 Desktop
drwxr-xr-x. 2 linux_lab linux_lab   6 Feb  5 15:34 Documents
drwxr-xr-x. 2 linux_lab linux_lab   6 Feb  5 15:34 Downloads
-rw-r--r--. 1 linux_lab linux_lab 79K Mar 27 14:28 find_results.txt
drwxr-xr-x. 2 linux_lab linux_lab   6 Feb  5 15:34 Music
drwxr-xr-x. 2 linux_lab linux_lab   6 Feb  5 15:34 Pictures
-rw-r--r--. 1 linux_lab linux_lab 303 Mar 27 13:02 ps.txt
drwxr-xr-x. 2 linux_lab linux_lab   6 Feb  5 15:34 Public
drwxr-xr-x. 2 linux_lab linux_lab   6 Feb  5 15:34 Templates
drwxr-xr-x. 2 linux_lab linux_lab   6 Feb  5 15:34 Videos
```

the `tree` command will show a graphical view of the of the file structure showing which files are associated with which directory

```
[linux_lab@localhost home]$ tree
.
└── linux_lab
    ├── Desktop
    ├── Documents
    ├── Downloads
    ├── find_results.txt
    ├── Music
    ├── Pictures
    ├── ps.txt
    ├── Public
    ├── Templates
    └── Videos

9 directories, 2 files

```

## LSOF
this is a similar to command to `ls` but `lsof` lists `open files`.
lists files currently open by a user process. allows us to find files open by a specific user, program or network connection

```
[linux_lab@localhost ~]$ lsof -u linux_lab
COMMAND     PID      USER   FD      TYPE             DEVICE  SIZE/OFF       NODE NAME
systemd    5160 linux_lab  cwd       DIR              253,0       235        128 /
systemd    5160 linux_lab  rtd       DIR              253,0       235        128 /
systemd    5160 linux_lab  txt       REG              253,0    134720   17961708 /usr/lib/systemd/systemd
systemd    5160 linux_lab  mem       REG              253,0    601969   35348946 /etc/selinux/targeted/contexts/files/file_contexts.bin
systemd    5160 linux_lab  mem       REG              253,0     69096   51305898 /usr/lib64/libffi.so.8.1.0
systemd    5160 linux_lab  mem       REG              253,0    202480   51305886 /usr/lib64/libgpg-error.so.0.32.0
systemd    5160 linux_lab  mem       REG              253,0     69392   51376353 /usr/lib64/libattr.so.1.1.2501
systemd    5160 linux_lab  mem       REG              253,0    595648   51305907 /usr/lib64/libpcre2-8.so.0.11.0
systemd    5160 linux_lab  mem       REG              253,0    135104   51305642 /usr/lib64/libz.so.1.2.11
```

## Creating Files
the `touch` command lets you to create empty files
the original purpose of the `touch` command was to modify access and modification timestamps of a file

you can create a single or multiple files with the touch commmand
for creating multiple files, list out the file names seperated bu a space

```
[linux_lab@localhost Documents]$ touch project1.txt
[linux_lab@localhost Documents]$ ls
project1.txt
[linux_lab@localhost Documents]$ touch project12.txt project22.txt project32.txt
[linux_lab@localhost Documents]$ ls
project12.txt  project1.txt  project22.txt  project32.txt
[linux_lab@localhost Documents]$ 
```
from a user perspective, a directory contains files. But the directory is a special `file used to locate other files`
a file for which the directory is responsible has some of its metadata stored within the directory file such as the filename and inode number.
    - a file can then be located by its managing directory
the inode contains the actual files permissions, size, timestamps etc.

the `mkdir` command will create an empty directory in the current working directory. we can use the `-v` switch to show us whether the directory has been created

```
[linux_lab@localhost Documents]$ mkdir Projects
[linux_lab@localhost Documents]$ ls
Projects

[linux_lab@localhost Documents]$ mkdir -v Projects
mkdir: created directory 'Projects'
```
to create a directory in a different location than the cwd, an absolute path can be used. a sub-directory cannot be created without a parent directory.
the `-p` switch overwrites this and will create any non-existent directory in the path provided

for example, when creating the sub-dir `something_else`, the parent directory `some_dir` does not exist,
using the `-p` switch, some_dir is created first, followed by something else

```
[linux_lab@localhost Documents]$ mkdir -p /home/linux_lab/some_dir/something_else
[linux_lab@localhost Documents]$ cd
[linux_lab@localhost ~]$ ls
Desktop  Documents  Downloads  find_results.txt  Music  Pictures  ps.txt  Public  some_dir  Templates  Videos
[linux_lab@localhost ~]$ 
```

```
[linux_lab@localhost Documents]$ mkdir -pv /home/linux_lab/some_dir/something_else
mkdir: created directory '/home/linux_lab/some_dir'
mkdir: created directory '/home/linux_lab/some_dir/something_else'
[linux_lab@localhost Documents]$
```
