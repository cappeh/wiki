# Viewing File Information

the `file` command provides information about the type of a file 

```bash
[linux_lab@localhost ~]$ file ps_out_original.txt 
ps_out_original.txt: ASCII text

[linux_lab@localhost ~]$ file hello_world.sh 
hello_world.sh: Bourne-Again shell script, ASCII text executable
```

`ps_out_original` is a normal text file whilst `hello_world.sh` is a shell script file

for more information about when the file was created, last accessed or last modified, we can use the `stat` command

```bash
[linux_lab@localhost ~]$ stat ps_out_original.txt 
  File: ps_out_original.txt
  Size: 303       	Blocks: 8          IO Block: 4096   regular file
Device: fd00h/64768d	Inode: 18431579    Links: 1
Access: (0644/-rw-r--r--)  Uid: ( 1000/linux_lab)   Gid: ( 1000/linux_lab)
Context: unconfined_u:object_r:user_home_t:s0
Access: 2026-05-01 08:50:05.376122970 +0100
Modify: 2026-03-27 13:02:45.064335456 +0000
Change: 2026-05-01 08:49:22.331871477 +0100
 Birth: 2026-03-27 13:02:45.044343241 +0000
```

this provides information such as filename, inode number, size and the physical device it is store on

## File Differences

the `diff` command allows us to explore differences between two files line by line
we can also use the `sdiff` command which is more human friendly 

```bash
[linux_lab@localhost ~]$ diff ps_out_original.txt hello_world.sh 
1,6c1,3
<     PID TTY      STAT   TIME COMMAND
<    5182 tty2     Ssl+   0:00 /usr/libexec/gdm-wayland-session --register-session gnome-session
<    5189 tty2     Sl+    0:00 /usr/libexec/gnome-session-binary
<   21957 pts/0    Ss     0:00 bash
<   22388 pts/0    R+     0:00 ps a
<   22389 pts/0    S+     0:00 tee ps.txt
---
> #!/bin/bash
> 
> echo "Hello World"
```

this tells us that lines 1-6 of ps_out_original.txt are different from lines 1-3 of hello_world.sh
`c` means `change`
if you see `a` it means that additions are needed or `d` for anything that needs deleted


```bash
[linux_lab@localhost ~]$ diff -y ps_out_original.txt hello_world.sh 
    PID TTY      STAT   TIME COMMAND			              |	#!/bin/bash
   5182 tty2     Ssl+   0:00 /usr/libexec/gdm-wayland-session |
   5189 tty2     Sl+    0:00 /usr/libexec/gnome-session-binar |	echo "Hello World"
  21957 pts/0    Ss     0:00 bash			                  <
  22388 pts/0    R+     0:00 ps a			                  <
  22389 pts/0    S+     0:00 tee ps.txt			              <

[linux_lab@localhost ~]$ diff -yW 200 ps_out_original.txt hello_world.sh 
    PID TTY      STAT   TIME COMMAND								                               |	#!/bin/bash
   5182 tty2     Ssl+   0:00 /usr/libexec/gdm-wayland-session --register-session gnome-session	   |
   5189 tty2     Sl+    0:00 /usr/libexec/gnome-session-binary					                   |	echo "Hello World"
  21957 pts/0    Ss     0:00 bash								                                   <
  22388 pts/0    R+     0:00 ps a								                                   <
  22389 pts/0    S+     0:00 tee ps.txt								                               <
```

here `-y` means display the files side by side
`-W` sets the total display width to be 200 characters
`|` these lines designate the second files lines that are different from the first
`<` these lines only exist in the first file
