# GAWK Stream Editor
This is more powerful than sed due to its programming language which allows:
- defining variables to store data
- use artithmetic and string operators to work on data
- use structures such as if statements, loops
- format data into reports

`awk` was the initial program created for Unix, the GNU project rewrote the project and called it `GNU awk or gawk`.
both `awk` and `gawk` can be used but both will call the `gawk` program.

```
[linux_lab@localhost ~]$ which awk
/usr/bin/awk

[linux_lab@localhost ~]$ readlink -f $(which awk)
/usr/bin/gawk
```
## Syntax

`gawk [OPTIONS] [PROGRAM]... [FILENAME]`

- the program/script is typically enclosed in `''`.
- the programming language commands such be enclosed with '{}'

## Options

```
Short               Long                                Descriptions
-F d                --field-seperator d                 specify the delimiter seperating the data file fields
-f file             --file=file                         use a file for processing the text
-s                  --sandbox                           execute the command in sandbox mode
```

## Examples

```
[linux_lab@localhost ~]$ echo "Hello World" | gawk '{print $0}'
Hello World

[linux_lab@localhost ~]$ echo "Hello World" | gawk '{print $1}'
Hello

[linux_lab@localhost ~]$ echo "Hello World" | gawk '{print $2}'
World
```
- `$0` means print the entire text line
- `$1` means print the first field of the text line
- `$n` print the nth field


the `gawk` utility can also process text data from a file

```
[linux_lab@localhost ~]$ cat cake.txt
someone likes chocolate cake
another person likes lemon cake
a friend likes yellow cake
someone else does not like cake

[linux_lab@localhost ~]$ gawk '{print $1}' cake.txt
someone
another
a
someone
```

we can also use programming structures to make modifications to text.

```
[linux_lab@localhost ~]$ cat cake.txt
someone likes chocolate cake
another person likes lemon cake
a friend likes yellow cake
someone else does not like cake

[linux_lab@localhost ~]$ gawk '{$4="donuts"; print $0}' cake.txt
someone likes chocolate donuts
another person likes donuts cake
a friend likes donuts cake
someone else does donuts like cake
```
this changes the 4th text data field on each line to 'donuts' and then prints the entire text with `$0`.
this might not be the behaviour you want. What if you only wanted to change the 4th word if the word was 'cake'

```
[linux_lab@localhost ~]$ cat cake.txt
someone likes chocolate cake
another person likes lemon cake
a friend likes yellow cake
someone else does not like cake

[linux_lab@localhost ~]$ gawk '{if ($4 == "cake") {$4="donuts"; print $0}}' cake.txt
someone likes chocolate donuts
```
the if statement checks whether the 4th data field is equal to "cake". if the statement returns true, the data field is changed to donuts and displayed to STDOUT.
otherwise the text line is ignored.

```
[linux_lab@localhost ~]$ gawk -F: '{print $1}' /etc/passwd
root
bin
daemon
linux_lab
[...]
```
if fields are seperated by commas or colons etc, the `-F` option switch can be used, then we can print the first text field like in the above

```
[linux_lab@localhost ~]$ cat cake.txt
someone likes chocolate cake
another person likes lemon cake
a friend likes yellow cake
someone else does not like cake

[linux_lab@localhost ~]$ cat script.gawk 
{if ($4=="cake")
  {$4="donuts"; print $0}
else if ($5=="cake")
  {$5="donuts"; print $0}}

[linux_lab@localhost ~]$ gawk -f script.gawk cake.txt
someone likes chocolate donuts
another person likes lemon donuts
a friend likes yellow donuts
```
the above uses the `-f` option switch to specifiy a file to be used such as `script.gawk`.
no single quotes are needed in the file
