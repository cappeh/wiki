# BASH

`Bourne Again Shell`
Bash works as REPL `Read, Eval, Print, Loop` inside the Terminal
Bash is a program, a shell waiting for a command

bash is always in a directory.

`pwd` shows which directory you are in (print working directory)
you can have finder (MacOS) and the terminal open together to see live what is happening such as creating/removing a file
the underlying filesystem is the same whether in the terminal or finder. So they are fully in sync and changes such as `touch file.tst` is immediate

## Baisc File Manipulation

`touch` will create a file

`touch lesson-1.txt lesson-2.txt`

if we accidentally create a file called `lesson-34.txt` we can use the `mv` command to rename it

`mv lesson-34.txt lesson-3.txt`


if we `mv` a file to an existing file such as `lesson-2.txt`, then `lesson-2.txt` will be overwritten
if we try to do this on finder, it will block us, the terminal is very powerful

to remove a file

`rm lesson*`
this uses a glob '*' meaning that lesson followed by any number of characters can match (as long as lesson is at the beginning)

`rm -i lesson*`
this is interactive. It will raised a message asking if you want to remove this file.
`remove lesson-1.txt` -------> you can answer with either 'y' or 'n'

if we want this to be the default behaviour, we can create an alias of the `rm` program as we cant change the program itself
`alias rm='rm -i'`

now when we run `rm` the '-i' option is added implicitly
we can run `alias rm` to see what the alias refers to

`CTRL + l` is another way of clearing the terminal instead of using `clear`

## Hidden Files

any file that begins with '.' is a hidden file
`touch .file.txt` is considered a hidden file

`ls` will not show hidden files by default
`ls -a` will show **all** files including hidden

we will also see './' (the current directory)
if you `cd .` we will stay in the current directory

and '../' which is the parent directory of the current directory
so '/Users/calupric/learn_bash', the parent is 'calupric'

`cd -` will change in to the last directory we were in

## Searching In Files

`cat` will print the contents of a file `cat file.txt`

`cat /usr/share/dict/words` ------> this has a lot of output

`grep`
grep is a program to search a file or input stream for a pattern

`grep dave /usr/share/dict/words`
this will search for all instances of the pattern dave in the given file

```
grep dave /usr/share/dict/words        
cadaver
cadaveric
cadaverine
cadaverize
cadaverous
cadaverously
cadaverousness
daven
davenport
daver
daverdy
```
so here grep through each line and will print the lines that contain 'dave' somewhere in the text

```
grep '^dave' /usr/share/dict/words
daven
davenport
daver
daverdy
```
the '^' will search for lines that begin with 'dave', you can use 'dave$' to search the end of a line

`grep -A1 b file.txt` - this will print a line that has a b in it and the next line immediately After it (A After 1 line)
`grep -B1 b file.txt` - similar to above, where it is 1 line above
`grep -C1 b file.txt` - this is the Context, so it shows 1 line before and 1 line after

grep is case sensitive, so if we search 'dave' it will match only on all lower case dave. we can use '-i' to match case insensitive so this would match on 'dave' or 'DAVE'

`grep -o` with `-o` grep will only print the part that matches

## Paging Files
a program that pages through input. the `less` command is used here. `paginate through data`
`less /usr/share/dict/words`
press `q` to quit/close the pager and can move with things such as vim keys j - down, k - up
you can also use `/` to start a search. `n` to move forward, 'N' to move backwards

`more` can also be used but `less` is more common

## Man Pages
manuals of the different programs installed on the machine.
it uses less to display the manual
we can search like in less such as `/-a` will search the man for the '-a' flag
man has sections such as `man 1 cp` -------> 1 is for general commands for example **check out `man man`**
man will work down the sections so if the man argument isnt a general command, it will look for a section 2 document and so on

`cp` and `mv` are examples of **external commands** 
the `history` command is an example of a **builtin** command. If you do `man history` it will show the builtin man page instead of the documentation for `history`
the **bash** way of getting the documentation is `help`

if you run `which history`, it doesnt provide an answer which means it is a builtin. if you ran `which ls` you would get `/bin/ls`

we can use the `type` command to determine what type of program we have
`type history` will show **history is a shell builtin**

`type -a ls` will show if you have an alias set up, then will show if its builtin or external

```
󰀵 calupric in ~/learn_bash ❯ type ls
ls is an alias for eza -lhbgum --icons=always --ignore-glob="$IGNORE_GLOBS"

󰀵 calupric in ~/learn_bash ❯ type -a ls
ls is an alias for eza -lhbgum --icons=always --ignore-glob="$IGNORE_GLOBS"
ls is /bin/ls
```

when we man echo we dont get the correct man page because it will default to the /bin/echo program not the shell builtin function
```
󰀵 calupric in ~/learn_bash ❯ type -a echo
echo is a shell builtin
echo is /bin/echo
```

`compgen -b` will list all the builtin commands

## Programs and Commands
