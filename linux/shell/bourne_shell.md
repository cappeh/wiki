# Bourne Shell: /bin/sh

This is one of the most important parts of a Linux system. A shell is a program that runs commands such as ones that users enter into a terminal window.
The commands can either be other programs or builtin features of the shell.
The shell can also serve as a programming environment

Many important parts of Linux are shell scripts - text files that contain sequential shell commands.

There are many different shells such as
- /bin/bash
- /bin/zsh
- /bin/ksh
- /bin/csh

but all the shells derive features from the **Bourne Shell**
Linux uses an enhanced version of the **Bourne Shell** called the **Bourne Again Shell** `/bin/bash`

a terminal is often called a shell window. a terminal application starts a shell inside a new window

```bash
cat /etc/passwd
```

a command will usually start with a program to run `cat` which could be followed by one or more arguments which tells the program what to 
operate on and how to do so `/etc/passwd`
many arguments are options that modify the behaviour of the command and typically begin with a `-` (dash)
