# Pinpoint Commands

these are commands that can locate files on your system such as utilities, configuration files or documentation.

## Which Command

The `which` command shows the full pathname of an executable if it is located in one of the directories listed in the $PATH environment variable.

```
[linux_lab@localhost ~]$ which diff
/usr/bin/diff

[linux_lab@localhost ~]$ which shutdown
/usr/sbin/shutdown

[linux_lab@localhost ~]$ which line
/usr/bin/which: no line in (/home/linux_lab/.local/bin:/home/linux_lab/bin:/usr/local/bin:/usr/local/sbin:/usr/bin:/usr/sbin)

[linux_lab@localhost ~]$ echo $PATH
/home/linux_lab/.local/bin:/home/linux_lab/bin:/usr/local/bin:/usr/local/sbin:/usr/bin:/usr/sbin
[linux_lab@localhost ~]$ 
```

the given utility passed as an argument shows the programs location on the filesystem. The which program is good to use to quickly determine
whether a program is installed on the system

the which command uses the `PATH` environment variable which contains all the directories the which program will search through

```
[linux_lab@localhost ~]$ which ls
alias ls='ls --color=auto'
	/usr/bin/ls
```
the `which` command can also be used to identify is a command is using an alias like the `ls` command in Rocky Linux

## Whereis

the `whereis` command will help you find where the executable is located as well as its source code files and man pages

```
[linux_lab@localhost ~]$ whereis diff
diff: /usr/bin/diff /usr/share/man/man1/diff.1.gz
```
in the above example, the binary for diff and a man page is found

## Locate

the `locate` program can be used to find files on the local system. It searches through a database called `mlocate.db` located in the `/var/lib/mlocate` directory

`locate [OPTION]... PATTERN...`

the PATTERN means that regex can be used to find a file or partial filenames
if the file you are locating is on your system and you have permission to view it, the `locate` utility will display the file path and name

```
[linux_lab@localhost ~]$ locate ps_out_original.txt 
/home/linux_lab/ps_out_original.txt
```
