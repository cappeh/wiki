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
