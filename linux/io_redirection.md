# I/O Redirection

- `stdin`
    - **standard input**. stdin typically comes from the keyboard
      with a pipe or redirector, stdin can also be taken from a file or the output of another command

- `stdout`
    - **standard output**. by default output is sent to the terminal (screen). The output can be used as stdin for another program by using | **pipe**
      the output can also be redirected a file on another storage device.
      output can also be sent to /dev/null - the bit bucket if you dont need to see the output on screen

- `stderr`
    - **standard error**. similar to stdout, if a command fails to execute correctly, an error message will appear on the screen
      provides a way of redirecting error messages to the bit bucket or storing error messages in a file

everything in linux / unix-like system is a file. stdin, stdout, stderr are all files in the /proc directory
the /dev directory has symbolic links to these files

```
[linux_lab@localhost dev]$ ls -l std*
lrwxrwxrwx. 1 root root 15 Mar  3 11:42 stderr -> /proc/self/fd/2
lrwxrwxrwx. 1 root root 15 Mar  3 11:42 stdin -> /proc/self/fd/0
lrwxrwxrwx. 1 root root 15 Mar  3 11:42 stdout -> /proc/self/fd/1
```

the files that represent stdin, stdout, stderr are known as **F**ile **D**escriptors in the fd directory we see above
0 = stdin
1 = stdout
2 = stderr

## STDOUT

you can redirect the output of a command to a text file.

```
[linux_lab@localhost ~]$ ls -al > results.txt
```

this will take the output of the ls command and sends it to the **results.txt** file

```
[linux_lab@localhost ~]$ cat results.txt 
total 36
drwx------. 14 linux_lab linux_lab 4096 Mar 27 09:19 .
drwxr-xr-x.  3 root      root        23 Feb  5 15:20 ..
-rw-------.  1 linux_lab linux_lab 1069 Mar  3 11:05 .bash_history
-rw-r--r--.  1 linux_lab linux_lab   18 Apr 30  2024 .bash_logout
-rw-r--r--.  1 linux_lab linux_lab  141 Apr 30  2024 .bash_profile
-rw-r--r--.  1 linux_lab linux_lab  492 Apr 30  2024 .bashrc
drwx------. 12 linux_lab linux_lab 4096 Feb  5 15:56 .cache
drwx------. 10 linux_lab linux_lab 4096 Feb  5 15:42 .config
drwxr-xr-x.  2 linux_lab linux_lab    6 Feb  5 15:34 Desktop
drwxr-xr-x.  2 linux_lab linux_lab    6 Feb  5 15:34 Documents
drwxr-xr-x.  2 linux_lab linux_lab    6 Feb  5 15:34 Downloads
-rw-------.  1 linux_lab linux_lab   20 Mar  3 13:58 .lesshst
drwx------.  4 linux_lab linux_lab   32 Feb  5 15:34 .local
drwxr-xr-x.  5 linux_lab linux_lab   54 Feb  5 15:56 .mozilla
drwxr-xr-x.  2 linux_lab linux_lab    6 Feb  5 15:34 Music
drwxr-xr-x.  2 linux_lab linux_lab    6 Feb  5 15:34 Pictures
drwxr-xr-x.  2 linux_lab linux_lab    6 Feb  5 15:34 Public
-rw-r--r--.  1 linux_lab linux_lab    0 Mar 27 09:19 results.txt
drwxr-xr-x.  2 linux_lab linux_lab    6 Feb  5 15:34 Templates
drwxr-xr-x.  2 linux_lab linux_lab    6 Feb  5 15:34 Videos
-rw-------.  1 linux_lab linux_lab  796 Mar  3 12:03 .viminfo
```

if the file your directing to already exists, the contents will be overwritten by default

```
[linux_lab@localhost ~]$ ls > results.txt 
[linux_lab@localhost ~]$ cat results.txt 
Desktop
Documents
Downloads
Music
Pictures
Public
results.txt
Templates
Videos
```

stdout can also be appended to a file rather than overwritting with the >> operator

## Prevent File Overwrite

the **noclobber** option can be set in the terminal to show an error message if we try and overwrite a file

```
[linux_lab@localhost ~]$ set -o noclobber
[linux_lab@localhost ~]$ ls -al > results.txt
bash: results.txt: cannot overwrite existing file
```
we could still overwrite the file by using the >| operator. no error message will appear and the file contents will be overwritten

```
[linux_lab@localhost ~]$ ls -al >| results.txt
```

**noclobber** is not a permanent option. If we exit/close the terminal the option will no longer be set
when **noclobber** is set, it wont prevent the contents being overwritten when we **mv** or **cp** commands or prevent us from deleting the file with the **rm** command

## File Descriptors

because stdout is File Descriptor 1, you could also do the following to redirect stdout to a file

```
[linux_lab@localhost ~]$ ls 1> results.txt
```

## STDIN

command < inputfile.txt

the `<` operator will inputfile.txt as the stdin for the command instead of the input from a keyboard

```
[linux_lab@localhost ~]$ tr [:lower:] [:upper:] < results.txt 
DESKTOP
DOCUMENTS
DOWNLOADS
MUSIC
PICTURES
PUBLIC
RESULTS.TXT
TEMPLATES
VIDEOS
```
the **tr** command translates things. In this example the content of results.txt (ls command output) is taken as stdin for the translate command
and will translate the contents to UPPER CASE
the original results.txt file is not changed, the contents are echoed to the stdout

```
[linux_lab@localhost ~]$ tr [:lower:] [:upper:] < results.txt > results_upper.txt 
```
this will translate the contents of results.txt and redirect the output to the results_upper.txt file in all UPPER CASE

using `>>` stdout will be appended to the file rather than overwritting

```
[linux_lab@localhost ~]$ tr [:lower:] [:upper:] < results.txt >> results.txt 
[linux_lab@localhost ~]$ cat results.txt 
Desktop
Documents
Downloads
Music
Pictures
Public
results.txt
Templates
Videos
DESKTOP
DOCUMENTS
DOWNLOADS
MUSIC
PICTURES
PUBLIC
RESULTS.TXT
TEMPLATES
VIDEOS
```

## STDERR

the file descriptor id for stderr is 2 making the redirector operators `2>` or `2>>`

command 2> errorfile.txt

messages will be sent to stderr (same as stdout) if there is an error when running a command so stderr gets mixed with stdout messages
`2>>` will append error messages to an existing file

```
[linux_lab@localhost ~]$ find / -name README
find: ‘/boot/efi’: Permission denied
find: ‘/boot/grub2’: Permission denied
find: ‘/boot/loader/entries’: Permission denied
```

you will get error messages like the above if as a normal user you search for files across the entire filesystem
this is because a non root user does not have permissions to access all files across the filesystem

you can use `2>` to send the error messages to the bit bucket `/dev/null` or to a file so you will only see the stdout (good) output on the terminal/screen

```
[linux_lab@localhost ~]$ find / -name README 2> /dev/null
/etc/fonts/conf.d/README
```

we can combine redirectors so that stdout can be sent to one file but stderr sent to another
below `find_results.txt` will contain the good (stdout) output while `find_err.txt` will contain the error messages

```
[linux_lab@localhost ~]$ find / -name README > find_results.txt 2> find_err.txt
```

the below sends all stdout output to the bitbucket so only error messages are displayed

```
[linux_lab@localhost ~]$ find / -name README > /dev/null
```

to send both stdout and stderr to the same file we can use `2>&1` at the end of the command
    - this means stderr (fd 2) will go to the same place as stdout (fd 1)

this will send all output (stdout and stderr) to find_results.txt

```
[linux_lab@localhost ~]$ find / -name README > find_results.txt 2>&1
```

## TEE

**tee** is a utility that will print the output of a command to the screen and to a file at the same time
instead of using a redirect symbol, we pipe the output of the first command to the **tee** command

here we print the output of the **ps a** command to both the screen and to the file **ps.txt**
the **-a** option of **tee** will append the output to the file instead of overwritting

```
[linux_lab@localhost ~]$ ps a | tee ps.txt
    PID TTY      STAT   TIME COMMAND
   5182 tty2     Ssl+   0:00 /usr/libexec/gdm-wayland-session --register-session gnome-session
   5189 tty2     Sl+    0:00 /usr/libexec/gnome-session-binary
  21957 pts/0    Ss     0:00 bash
  22388 pts/0    R+     0:00 ps a
  22389 pts/0    S+     0:00 tee ps.txt

[linux_lab@localhost ~]$ ls -l ps.txt 
-rw-r--r--. 1 linux_lab linux_lab 303 Mar 27 13:02 ps.txt

[linux_lab@localhost ~]$ cat ps.txt 
    PID TTY      STAT   TIME COMMAND
   5182 tty2     Ssl+   0:00 /usr/libexec/gdm-wayland-session --register-session gnome-session
   5189 tty2     Sl+    0:00 /usr/libexec/gnome-session-binary
  21957 pts/0    Ss     0:00 bash
  22388 pts/0    R+     0:00 ps a
  22389 pts/0    S+     0:00 tee ps.txt
```

**tee** is useful in a shell script when you need to create or update a conf file in the /etc directory
by default even with sudo, you will get a permission denied when using redirection such as >, >>, >|
(you can do this is if logged in as root)

below, the will echo "new setting" to stdout and write the text to the someconfig.conf file

```
[linux_lab@localhost ~]$ echo "new setting" | sudo tee /etc/someconfig.conf
```

**tee** will always send both good and error content to the screen but will only send good content to the file

