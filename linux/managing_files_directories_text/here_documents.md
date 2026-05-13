# Here Documents
a `here document` is another form of input redirection. AKA `here text` or `heredoc`
this lets you redirect multiple items into a command

the redirection operator for a `here document` is `<<` followed by a keyword that denotes the end of the data
the example below shows multiple strings being sent to the `sort` command.
`EOF` is used to indicate when the input has ended.

```
[linux_lab@localhost ~]$ sort <<EOF
> dog
> fish
> cat
> rabbit
> turtle
> EOF
cat
dog
fish
rabbit
turtle
[linux_lab@localhost ~]$
```
any keyword can be used to denote the end of the `heredoc` such as using the keyword `END`

```
[linux_lab@localhost ~]$ cat <<END
> hello
> the end is after this line
> END
hello
the end is after this line
[linux_lab@localhost ~]$ 
```



## Variable Expansion

```
[linux_lab@localhost ~]$ cat <<EOF
> Home: $HOME
> EOF
Home: /home/linux_lab
```
here we can use $VARIABLE syntax to expand a variable or environment variable

```
[linux_lab@localhost ~]$ cat <<'EOF'
> Home: $HOME
> EOF
Home: $HOME
```
if we quote the ending keyword like above, we cannot use variable expansion

```
[linux_lab@localhost ~]$ cat <<EOF
> Date: `date`
> EOF
Date: Wed 13 May 08:54:13 BST 2026
```

we can also use `` to run a command within the heredoc. using `date` will print out the current date and time
another option is to use `$(command)` syntax to run a command

```
[linux_lab@localhost ~]$ cat <<EOF
> Uptime: $(uptime)
> EOF
Uptime:  08:54:46 up 2 days,  1:03,  2 users,  load average: 0.54, 0.48, 0.45
```

## Working with Scripts

this shows an example of creating a config file for an application

```
[root@localhost ~]# cat <<EOF > /etc/myapp.conf
port=8080
mode=production
EOF

[root@localhost ~]# ls -l /etc/ | grep myapp
-rw-r--r--.  1 root root        26 May 13 09:10 myapp.conf

[root@localhost ~]# cat /etc/myapp.conf 
port=8080
mode=production
```
so this can be used within a shell script

```
#!/bin/bash

PORT=8080
MODE="production"
LOGFILE="/var/log/myapp.log"

cat <<EOF > /etc/myapp.conf
port=$PORT
mode=$MODE
logfile=$LOGFILE
EOF
```
the following script will create a multi-line string and use `echo` to output the multi-line message to STDOUT

```
#!/bin/bash

read -r -d '' MESSAGE <<EOF
Hello,
This is a multi-line message.
EOF

echo "$MESSAGE"
```

using `""` around the `MESSAGE` are important as they preserve the newlines and spacing
the `-d` OPTION sets the delimiter to a NUL character `''` which never appears in a `heredoc`. so `read` should keep reading until the end of the file or `EOF`
you could use `-d ':'` to read until the `:`

## Here String

this is a single line string only using `<<<`

```
[linux_lab@localhost ~]$ cat <<< "hello"
hello

[linux_lab@localhost ~]$ cat <<< "single line string"
single line string
```
## Tab Striping

using `<<-EOF` tabs are removed, spaces are not removed. which are useful in scripts

```
[linux_lab@localhost ~]$ ./remove_tab.sh 
this is a tabbed line

[linux_lab@localhost ~]$ cat remove_tab.sh 
#!/bin/bash

cat <<-EOF
	this is a tabbed line
EOF
[linux_lab@localhost ~]$ 
```
here we can see that the tabs at the beginning of the line have been removed


## Cheat Sheet

<<EOF      # heredoc with expansion
<<'EOF'    # no expansion
<<-EOF     # strip leading tabs
<<< "str"  # here-string (single line)
