# Cut Command
the `cut` command is useful to extract small pieces of data from a text file.
it will also allow you to view specific fields within a files records

the `cut` command does not change any data within the text file, it will copy the data that we want to view and displays it to the terminal

`cut OPTION... [FILES]...`

## Text File Records
this is a single file line that ends in a newline linefeed which is the ASCII character 'LF'
can use `cat -e` to see the newline linefeed as '$'
if the text file records end with the ASCII character 'NUL', you can still use the `cut` command but will need the `-z` option

## Text File Record Delimeter
for some of the `cut` command options to be used effectively, fields must exist within the text file record.
this is just data seperated by some delimiter such as ':'

## Common Options


| SHORT | LONG | DESCRIPTION |
|-------|------|-------------|
| `-c nlist` | `--characters nlist` | display only the record characters in the `nlist` (1-5) showing the first to 5 character |
| `-b blist` | `--bytes blist` | similar to `-c` but shows the `blist` bytes |
| `-d d` | `--delimiter d` | this will set the delimiter to use for the records such as `":"` (the boundary between different data items) |
| `-f flist` | `--fields flist` | display only the records fields in `flist` such as `1,3` meaning the first field and the third field |
| `-s` | `--only-delimited` | display only records that contain the delimiter |
| `-z` | `--zero-delimited` | designate the record end-of-line character as ASCII NUL |


```bash
[linux_lab@localhost ~]$ head -2 /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin

[linux_lab@localhost ~]$ cut -d ":" -f 1,7 /etc/passwd
root:/bin/bash
bin:/sbin/nologin
daemon:/sbin/nologin
sync:/bin/sync
dbus:/sbin/nologin
polkitd:/sbin/nologin
linux_lab:/bin/bash

[linux_lab@localhost ~]$ cut -c 1-5 /etc/passwd
root:
bin:x
daemo
adm:x
lp:x:
sync:
shutd
```

the second command will split the records fields that are delimitered by ":" and show only the first and seventh fields
the final command will display the first five characters of each line
