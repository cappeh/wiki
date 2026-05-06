# Find

the `find` command allows us to find files based on metadata, such as:
    - who owns the file
    - when the file was last accessed
    - when the file was last modified
    - who owns the file
    - the permissions of the file

`find [PATH...] [OPTION] [EXPRESSION]`

the `PATH` argument is the starting point directory for `find` to search through. `find` resursively (walks) through the subdirectories from this starting point.
you can use `.` to indicate to `find` to start in the Present Working Directory as the starting point

the `OPTION` and `EXPRESSION` arguments control what type of metadata filters are applied to the search or any settings that could limit the search.

OPTION      EXPRESSION      DESCRIPTION
================================================================================================================================================================
-cmin       n               display names of files whose status changed `n` minutes ago
-empty                      display names of files that are empty and are a regular text file or directory
-gid        n               shows files whose group id is equal to `n`
-group      name            shows files whose group name is equal to `name`
-inum       n               show files whose inode number equals `n`
-maxdepth   n               number of levels to traverse down from the starting point directory
-mmin       n               show files whose data changed `n` mins ago
-name       pattern         display files that match the given `pattern` if using regex enclose the `pattern` in double quotes. use `-iname` to ignore case
-nogroup                    display files where no group name exists for the files group id
-nouser                     display files where no username exists for the files user id
-perm       mode            display files whose permissions match `mode` either `octal` or `symbolic` 
-size       n               show files whose size matches `n`. A suffix can be used to be human friendly like `G` for gigabytes
-user       name            display files whos owner is `name`


`find . -name "*.txt`
this will look for files from the present working directory with the '.txt' extension

`find . -maxdepth 2 -name "*.txt"`
this is similar to the first example but with `maxdepth` find will search only 2 directories: the current directory and one sub directory down

the `find` command is useful for auditing a system on a regular basis or if you fear the server has been hacked

`find /usr/bin -perm /4000`
this shows the `/usr/bin` direcptry being audited for the potentially dangerous SUID permission.
`/4000` will search for SUID settings (octal code 4) and ignore other file permissions due to / in front of the number and ignore other file permissions (000) 
the resulting files would legitimately used SUID so nothing suspicious is going on

```
[linux_lab@localhost ~]$ find /usr/bin -perm /4000
/usr/bin/chage
/usr/bin/gpasswd
/usr/bin/newgrp
/usr/bin/fusermount
/usr/bin/fusermount3
/usr/bin/umount
/usr/bin/mount
/usr/bin/su
/usr/bin/pkexec
/usr/bin/crontab
/usr/bin/sudo
/usr/bin/vmware-user-suid-wrapper
/usr/bin/passwd
/usr/bin/chfn
/usr/bin/chsh
/usr/bin/at
```

