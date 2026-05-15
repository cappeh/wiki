# Stream Editors
this allows you to modify text within a file without using a text editor like vim/nvim, nano vscode etc.
special commands make changes to the text as it 'streams' through the editor.

## Sed (Stream Editor)
invoked using the `sed` command
the command processes input one line at a time
this edits a stream of text data with commands that are provided ahead of time. 
Text is only passed through (streamed) once which makes the command fast
the `sed` editor changes data based on commands entered through the command line or in a text file.

1. Reads one text line at a time from the input stream
2. matches the text based on the supplied commands
3. modifies the text based on the supplied commands
4. outputs the modified text to STDOUT

`sed [OPTIONS] [SCRIPT]... [FILENAME]`

by default `sed` will use text from STDIN to modify text.

```
[linux_lab@localhost ~]$ echo "I like cake." | sed 's/cake/donuts/'
I like donuts.
```
the `echo` command is piped as STDIN for the `sed editor`
the `sed` `s (substitute)` utility will change the first instance of 'cake' it comes across to 'donuts' in the output 

the command after `sed` enclosed in '' is considered a `SCRIPT`
the words are delimited from one another with '/'

```
[linux_lab@localhost ~]$ cat cake.txt 
I like cake.
I like cake.
I like cake.
[linux_lab@localhost ~]$ 

[linux_lab@localhost ~]$ sed 's/cake/donuts/' cake.txt 
I like donuts.
I like donuts.
I like donuts.
```
because `sed` goes line by line, each line that matches the command are modified and sent to STDOUT. the actual file is not modified

in this example, the first command shows that only the first instance of the command is changed per line
to apply the substitution for each instance of the word on a line, the `g` flag (global) is added at the end of the `sed` script

```
[linux_lab@localhost ~]$ echo "I like cake and even more cake." | sed 's/cake/donuts/'
I like donuts and even more cake.

[linux_lab@localhost ~]$ echo "I like cake and even more cake." | sed 's/cake/donuts/g'
I like donuts and even more donuts.
[linux_lab@localhost ~]$
```

by providing the `-i` option, we can modify the file 

```
[linux_lab@localhost ~]$ sed -i 's/cake/donuts/' cake.txt 
[linux_lab@localhost ~]$ cat cake.txt 
I like donuts.
I like donuts.
I like donuts.
[linux_lab@localhost ~]$ 
```
### Some Options
```
SHORT           LONG                        DESCRIPTION
=========================================================================================================================
-e script       --expression=script         with this option we can create multiple scripts for the sed command
-f script       --file=script               use a file with the script for the sed command
-r              --regexp-extended           use extended regular expressions in the script
```

here we use the `-e` option which allows us to use multiple script commands. Both scripts should be seperated by a semi-colon `;` but both should still be enclosed in `''`
allowing both to be processed on the text stream
the first instance of 'cake' will be substituted by 'donuts' and like wise for 'like' and 'love'

```
[linux_lab@localhost ~]$ cat cake.txt
someone likes chocolate cake
another person likes lemon cake
a friend likes yellow cake
someone else does not like cake

[linux_lab@localhost ~]$ sed -e 's/cake/donuts/; s/like/love/' cake.txt
someone loves chocolate donuts
another person loves lemon donuts
a friend loves yellow donuts
someone else does not love donuts
```
we can also create a `.sed` file that contains multuple scripts that can be deployed to a text stream
no quotes are needed when using a sed file

```
[linux_lab@localhost ~]$ cat script.sed 
s/cake/donuts/
s/like/love/

[linux_lab@localhost ~]$ sed -f script.sed cake.txt
someone loves chocolate donuts
another person loves lemon donuts
a friend loves yellow donuts
someone else does not love donuts
```
