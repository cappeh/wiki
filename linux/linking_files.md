# Linking Files and Directories
There are two types of file links:
    - `Symbolic Links` AKA `soft link`
    - `Hard Links`

## Hard Links
A Hard Link is an `index (inode) number` with at least two filenames.
A single inode number means that there is one single file where the data is stored on a partition of a disk
having two or more filenames means there are multiple ways to access the file or its data 

filename#1 -----------> (inode #1234) <----------- filename#2
inode #1234 is one filesystem location on a disk partition

this is a psuedo copy without having to copy its data
can be used if not enough filesystem storage to back up the files data

to create a hardlink we use the `ln` command

```
[linux_lab@localhost ~]$ ln ps_out_original.txt ps_out_hardlink.txt

[linux_lab@localhost ~]$ ls -i ps_out_*
18431579 ps_out_hardlink.txt  18431579 ps_out_original.txt

[linux_lab@localhost ~]$ touch ps_out_new.txt

[linux_lab@localhost ~]$ ls -og ps_out_*
-rw-r--r--. 2 303 Mar 27 13:02 ps_out_hardlink.txt
-rw-r--r--. 1   0 Apr 14 14:05 ps_out_new.txt
-rw-r--r--. 2 303 Mar 27 13:02 ps_out_original.txt
```
the `ps_out_hardlink.txt` file was created by the `ln` command
`ls -i` shows that the indode number for the ps out files are the same

the `ls -og` command shows the link count of files. here we see that original and hardlink have 2
because they are both hardlinked to one other file. the ps_out_new has a link count of 1 because it is not hardlinked

this command is similar to `ls -l` but excludes the file owner and group

if you want to remove a linked file and keep the original use the `unlink` command with the linked filename as an argument

### Things to remember
the original file name must exist before issuing the `ln` command
the second file name must not exist prior to issuing the command
the original file and the hard links share the same inode number
the original and the hard link share the same data
an original and the hardlink can exist in different directories
an original and the hard linke must exist in the same filesystem

## Soft Links
A softlink provide a pointer to a file that may reside on another filesystem.
they do not share the same inode number because they dont point to the same data

filename#2 (inode #5678) -----------> filename#1 (inode #1234)

to create a softlink we use the `ln` command with the `-s` option or `--symbolic`

```
[linux_lab@localhost Documents]$ touch originalSFile.txt

[linux_lab@localhost Documents]$ ls
originalSFile.txt

[linux_lab@localhost Documents]$ ln -s originalSFile.txt softlink.txt

[linux_lab@localhost Documents]$ ls -i
838810 originalSFile.txt  838812 softlink.txt

[linux_lab@localhost Documents]$ ls -og
total 0
-rw-r--r--. 1  0 Apr 15 07:50 originalSFile.txt
lrwxrwxrwx. 1 17 Apr 15 07:51 softlink.txt -> originalSFile.txt
```
this shows that the inode number for the original file is different to the new softlink.txt file that has been created
the `ls -og` command also shows the pointer from softlink.txt to originalSFile.txt
the link count does also **not** increase like it does for hard links

### Things to remember
the original file must exist before issuing the `ln -s` command
the second file must not exist prior to issuing the command
an original file and its soflink file do not share the same inode number
the original file and its softlink file do not share the same data
the original and softlink can reside in different directories
the original and the softlink can reside in different filesystems
