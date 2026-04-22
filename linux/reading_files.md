# Reading Files


## Reading an entire file
the concatenate command or `cat` is the utility to use for viewing a text file.
the commands original purpose was to join text files together and output them. However it is used to display a single text file

`cat [OPTION]... [FILE]...`

the command will print the entire file to the screen. When your prompt is returned, the line above is the last line of the file.

```
[linux_lab@localhost ~]$ cat ps_out_original.txt 
    PID TTY      STAT   TIME COMMAND
   5182 tty2     Ssl+   0:00 /usr/libexec/gdm-wayland-session --register-session gnome-session
   5189 tty2     Sl+    0:00 /usr/libexec/gnome-session-binary
  21957 pts/0    Ss     0:00 bash
  22388 pts/0    R+     0:00 ps a
  22389 pts/0    S+     0:00 tee ps.txt
[linux_lab@localhost ~]$ 
```
the `-n` or `--number` option will add line numbers to the output

```
[linux_lab@localhost ~]$ cat --number ps_out_original.txt 
     1	    PID TTY      STAT   TIME COMMAND
     2	   5182 tty2     Ssl+   0:00 /usr/libexec/gdm-wayland-session --register-session gnome-session
     3	   5189 tty2     Sl+    0:00 /usr/libexec/gnome-session-binary
     4	  21957 pts/0    Ss     0:00 bash
     5	  22388 pts/0    R+     0:00 ps a
     6	  22389 pts/0    S+     0:00 tee ps.txt
[linux_lab@localhost ~]$ 
```
an alternative to `cat` is `bat` (https://github.com/sharkdp/bat)


another command that can be used to display an entire text file is the `pr` command
its original use was to format text files for printing now it can be used to display a file with special formatting

`pr [OPTION]... [FILE]...`

to display one file, you need to use the page length option `-l or --length`. Without this option a small text file will roll off screen. You need to scroll up to see it
the `-t or --omit` switch is useful to only see the text within the file

```
[linux_lab@localhost ~]$ pr -tl 10 ps_out_original.txt 
    PID TTY      STAT   TIME COMMAND
   5182 tty2     Ssl+   0:00 /usr/libexec/gdm-wayland-session --register-session gnome-session
   5189 tty2     Sl+    0:00 /usr/libexec/gnome-session-binary
  21957 pts/0    Ss     0:00 bash
  22388 pts/0    R+     0:00 ps a
  22389 pts/0    S+     0:00 tee ps.txt
[linux_lab@localhost ~]$ 
```

the `pr` command can also be used to show two short files side by side using the `-m or --merge` switch

```
[linux_lab@localhost ~]$ pr -mtl 15 ps_out_original.txt numbers.txt 
    PID TTY	 STAT	TIME COMMAN         42
   5182 tty2	 Ssl+	0:00 /usr/l     2A
   5189 tty2	 Sl+	0:00 /usr/l     52
  21957 pts/0	 Ss	0:00 bash           0010 1010
  22388 pts/0	 R+	0:00 ps a           *
  22389 pts/0	 S+	0:00 tee ps 
```

could also use the `paste` command
