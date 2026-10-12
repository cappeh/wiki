# Man Pages

man pages AKA `manual pages` provide documentation for commands in Linux

```bash
man ls
```

man pages primarily focus on reference information, sometimes with examples. You can usually find what you are looking for

**IT IS NOT A TUTORIAL**

we can also search for a man page by using the `-k` flag or using `apropos`. The output will show the man page name, the section and a small
description of what the man page contains

```bash
man -k sort
apropos sort

bzip2(1), bunzip2(1)     - a block-sorting file compressor, v1.0.8 bzcat - decompresses files to stdout bzip2recover - recovers data from damaged bzip2 files
locate.code(8), locate.bigram(8) - sorted list compressor
lsort(ntcl)              - Sort the elements of a list
slapo-sssvlv(5)          - Server Side Sorting and Virtual List View overlay to slapd
slapo-valsort(5)         - Value Sorting overlay to slapd
sort(1)                  - sort or merge records (lines) of text and binary files
top(1)                   - display sorted information about processes
tsort(1)                 - topological sort of a directed grap
```

man pages are referenced by numbered sections such as the following


| **Section** | **Description** |
| --- | ---|
| 1 | User Commands |
| 2 | Kernel System Calls |
| 3 | High Level Linux Library Documentation |
| 4 | Device Interface and Driver Information | 
| 5 | File Descriptions (System Configuration Files) |
| 6 | Games |
| 7 | File Formats, Conventions and Encodings |
| 8 | Systems Commands and Servers |

by default man will diplay the first page that it finds
but we can use the section number to display different manual sections such as `man 5 passwd` shows information about how to read the `/etc/passwd` file
instead of information about using the `passwd` command

we could also use the `info` command which the GNU Project uses

some packages add their available documenation to `/usr/share/doc/`
