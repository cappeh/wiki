# Escaping and Quoting 
Quoting tells the shell which characters to interpret and which to treat literally.
The shell has many metacharacters that perform actions, such as:

$ * ? ! \ | ( ) & < > [ ] { } ; ~ # space

Quoting or escaping prevents the shell from interpreting these characters.

quoting or escaping will treat these characters literally

- `single quotes` preserve all characters literally
- `double quotes` allow for command/variable expansion
- `backlash` will allow to escape a character

## Single Quotes '...'

- Preserve everything literally
- No variable expansion
- No command substitution
- Cannot nest single quotes inside single quotes

 ```bash
[linux_lab@localhost ~]$ echo 'hello $USER, today is $(date)'
hello $USER, today is $(date)
 ``` 

## Double Quotes "..."

double quotes allow variable and command expansion

- Prevent globbing (*, ?, [ ])
- variable expansion: $USER
- command substitution: $(date) or `date`

Backslash escapes only: ", \, `, $

```bash
[linux_lab@localhost ~]$ echo "hello $USER, today is $(date)"
hello linux_lab, today is Fri 15 May 19:13:37 BST 2026
 ``` 

 ## Backslash \

Useful for escaping $, ", \, or spacescape a meta character making it a literal

```bash
[linux_lab@localhost ~]$ echo "\"
.
[linux_lab@localhost ~]$ echo "\\"
\
 ``` 

in the first example, the `\` will escape the " so the double quotes are not correctly closed
