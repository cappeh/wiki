# Pinpoint Commands

these are commands that can locate files on your system such as utilities, configuration files or documentation.

## Which Command

The `which` command shows the full pathname of an executable if it is located in one of the directories listed in the $PATH environment variable.

```bash
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

the given utility passed as an argument shows the programs location on the filesystem. The `which` program is good to use to quickly determine
whether a program is installed on the system

the which command uses the `PATH` environment variable which contains all the directories the which program will search through

```bash
[linux_lab@localhost ~]$ which ls
alias ls='ls --color=auto'
	/usr/bin/ls
```

the `which` command can also be used to identify is a command is using an alias like the `ls` command in Rocky Linux

## Whereis

the `whereis` command will help you find where the executable is located as well as its source code files and man pages

```bash
[linux_lab@localhost ~]$ whereis diff
diff: /usr/bin/diff /usr/share/man/man1/diff.1.gz
```

in the above example, the binary for diff and a man page is found

## Locate

the `locate` program can be used to find files on the local system. It searches through a database called `mlocate.db` located in the `/var/lib/mlocate` directory

`locate [OPTION]... PATTERN...`

the PATTERN means that regex can be used to find a file or partial filenames
if the file you are locating is on your system and you have permission to view it, the `locate` utility will display the file path and name

```bash
[linux_lab@localhost ~]$ locate ps_out_original.txt 
/home/linux_lab/ps_out_original.txt
```

file globbing is used by default using wildcards such as `*` or `?` which can expand a filename in to multiple names
such as `passw*d` could be expanded in to 'password' or 'passwrd'

if no wildcards are included in the PATTERN, `locate` by default will add wildcards to the pattern
so `passwd` would become `*passwd*`
to search for the basename `passwd` you would need to enclose the pattern in quotation marks ('' / "") and precede the patter with \

```bash
[linux_lab@localhost ~]$ locate -b passwd
/etc/passwd
/etc/passwd-
/etc/pam.d/passwd
/etc/security/opasswd
/usr/bin/gpasswd
/usr/bin/grub2-mkpasswd-pbkdf2
/usr/bin/passwd
/usr/sbin/chgpasswd
/usr/sbin/chpasswd
/usr/sbin/lpasswd
/usr/sbin/saslpasswd2
/usr/share/awk/ns_passwd.awk
/usr/share/awk/passwd.awk
/usr/share/bash-completion/completions/smbpasswd
/usr/share/doc/passwd
/usr/share/doc/perl-Net-SSLeay/examples/passwd-cb.pl
/usr/share/licenses/passwd
/usr/share/locale/ar/LC_MESSAGES/passwd.mo
/usr/share/man/cs/man1/gpasswd.1.gz
/usr/share/man/de/man1/gpasswd.1.gz
[...]

[linux_lab@localhost ~]$ locate -bc passwd
128
```

with file globbing, the `locate` command finds 128 files. (the output has been reduced as it will be too long)
the `-b` option finds files that match only the basename, dont show files where the pattern matches a directory name
the `-c` option displays a count of how many files where found instead of printing out the files line by line

```bash
[linux_lab@localhost ~]$ locate -b '\passwd'
/etc/passwd
/etc/pam.d/passwd
/usr/bin/passwd
/usr/share/bash-completion/completions/passwd
/usr/share/doc/passwd
/usr/share/licenses/passwd

[linux_lab@localhost ~]$ locate -bc '\passwd'
6
```

with file globbing turned off, only 6 files are found

if you do not have permission to view the contents of a directory, then the `locate` command will not display those files that match the patterm
so some files may be missing from the output

the PATTERN is a list, so you can also add additional PATTERNS seperated by a space

`locate -b '\passwd' '\group'`

another issue that can appear, is searching for newly created or downloaded files, `locate` only searches the `mlocate.db` database not the entire virtual directory.
this database is typically updated once a day via a cron job, therefore if its newly created, `locate` wont find it
we can update this using the `updatedb` utility with `sudo` privileges. It can take a while to run
