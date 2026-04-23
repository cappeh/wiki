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

## Reading Portions of a File

### GREP

`grep` can help you find a line or lines that contain a certain string within a given file

`grep [OPTIONS] PATTERN [FILE...]`

the pattern is the string you are searching for in the given file for example:

```
[linux_lab@localhost ~]$ grep linux_lab /etc/passwd
linux_lab:x:1000:1000:linux_lab:/home/linux_lab:/bin/bash

[linux_lab@localhost ~]$ grep Linux_lab /etc/passwd

[linux_lab@localhost ~]$ grep -i Linux_lab /etc/passwd
linux_lab:x:1000:1000:linux_lab:/home/linux_lab:/bin/bash
```
grep is case-sensitive as can be seen in the second command as the given pattern must match exactly
the `-i or --ignore-case` switch can be used to ingore the case during the search

### HEAD

the `head` command displays the first 10 lines of a text file by default

`head [OPTION]... [FILE]...`

```
[linux_lab@localhost ~]$ head /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
```
the `-n or --lines=` switch can change the default behaviour by showing the first X amount of lines of a file
you can also use a negative number `-30` to eliminate the files bottom lines

```
[linux_lab@localhost ~]$ head -n 4 /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin

[linux_lab@localhost ~]$ head --lines=3 /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
```


### TAIL

the `tail` command is the opposite to head and will show the last 10 lines of a file by default

`tail [OPTION]... [FILE]...`

as with the `head` command, the `-n or --lines=` option will change the default behaviour and show the last X number of lines.
if you add a `+` before the number the utility will display from that line number to the end of the file
the example below will start at line 25 and output the rest of the lines

```
[linux_lab@localhost ~]$ tail -n +25 /etc/passwd
flatpak:x:992:991:Flatpak system helper:/:/usr/sbin/nologin
libstoragemgmt:x:990:990:daemon account for libstoragemgmt:/:/usr/sbin/nologin
setroubleshoot:x:989:989:SELinux troubleshoot server:/var/lib/setroubleshoot:/usr/sbin/nologin
gdm:x:42:42:GNOME Display Manager:/var/lib/gdm:/usr/sbin/nologin
gnome-initial-setup:x:988:987::/run/gnome-initial-setup/:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/usr/share/empty.sshd:/usr/sbin/nologin
chrony:x:987:986:chrony system user:/var/lib/chrony:/sbin/nologin
dnsmasq:x:986:985:Dnsmasq DHCP and DNS server:/var/lib/dnsmasq:/usr/sbin/nologin
tcpdump:x:72:72::/:/sbin/nologin
linux_lab:x:1000:1000:linux_lab:/home/linux_lab:/bin/bash
```
the `tail` command is also a good utility to watch log files live, as new messages get appended to the bottom of the file
we can use the `-f or --follow` switch
to end the session, use `CTRL + C`

`sudo tail -f /var/log/auth.log` could also be `/var/log/secure`

some log files have been replaced in some linux distros and the messages are kept in a `journal` file managed by `journald`
to watch messages being added to the journal file use `journalctl --follow`

```
[linux_lab@localhost ~]$ journalctl --follow
Apr 23 08:07:11 localhost.localdomain NetworkManager[1076]: <info>  [1776928031.8509] dhcp4 (ens160): state changed new lease, address=172.16.72.129
Apr 23 08:07:11 localhost.localdomain systemd[1]: Starting Network Manager Script Dispatcher Service...
Apr 23 08:07:11 localhost.localdomain systemd[1]: Started Network Manager Script Dispatcher Service.
Apr 23 08:07:21 localhost.localdomain systemd[1]: NetworkManager-dispatcher.service: Deactivated successfully.
Apr 23 08:08:15 localhost.localdomain pipewire[2181]: spa.audioconvert: 0xaaaaf51e08d0: (0 suppressed) out of buffers on port 0 1
Apr 23 08:14:41 localhost.localdomain pipewire[2181]: spa.audioconvert: 0xaaaaf51e08d0: (0 suppressed) out of buffers on port 0 1
Apr 23 08:22:11 localhost.localdomain NetworkManager[1076]: <info>  [1776928931.8506] dhcp4 (ens160): state changed new lease, address=172.16.72.129
Apr 23 08:22:11 localhost.localdomain systemd[1]: Starting Network Manager Script Dispatcher Service...
Apr 23 08:22:11 localhost.localdomain systemd[1]: Started Network Manager Script Dispatcher Service.
Apr 23 08:22:21 localhost.localdomain systemd[1]: NetworkManager-dispatcher.service: Deactivated successfully.
Apr 23 08:22:43 localhost.localdomain systemd[2034]: Started Application launched by gnome-shell.
Apr 23 08:22:44 localhost.localdomain systemd[1]: Starting Hostname Service...
Apr 23 08:22:44 localhost.localdomain systemd[1]: Started Hostname Service.
Apr 23 08:22:44 localhost.localdomain gnome-shell[2120]: g_object_ref: assertion 'G_IS_OBJECT (object)' failed
^C
```
here at 08:22:43 I opened the network settings menu from the toolbar on a Rocky Linux VM
