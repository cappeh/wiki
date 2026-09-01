# Grep Command
`grep` is useful for searching and matching text patterns in files using regular expressions
this is usually installed by default on all linux distributions


| SHORT | LONG | DESCRIPTION |
|-------|------|-------------|
| `-c` | `--count` | show a count of text file records that contain a pattern match |
| `-d action` | `--directories=action` | when a file is a directory, `read` the directory, `skip` the directory, or `recurse` the directory |
| `-E` | `--extended-regexp` | the `PATTERN` is an extended regular expression |
| `-i` | `--ignore-case` | ignore the case in the `PATTERN` and when searching files |
| `-R`, `-r` | `--recursive` | search a directory's contents, and in any subdirectory within the original directory tree |
| `-v` | `--invert-match` | show only text files that do not contain the `PATTERN` |
| `-n` | `--line-number` | prefix each line with a line number within the input file |


## Regular Expressions
a `regular expression` is a pattern template for use in programs such as `grep` which users the pattern to filter text.

### Basic Regular Expressions (BRE)

BRE is the default syntax for `grep`. Some characters must be escaped.

#### Special Characters
- `.`    matches **any single character**
- `*`    matches **zero or more** of the previous character or group
- `.*`   matches **any number of any characters**
- `^`    matches the **start of a line**
- `$`    matches the **end of a line**

#### Character Classes
- `[aeiou]` matches **any vowel**
- `[A-Z]`   matches **any uppercase letter**
- `[0-9]`   matches **any digit**

#### Grouping (must be escaped in BRE)
- `\(abc\)` — literal grouping  
  (unescaped parentheses are treated as normal characters)

#### Examples

`grep -v nologin$ /etc/passwd`
this command is useful for auditing where you can produce a list of text file records that do not contain the pattern
so this command will show all records that do not end with 'nologin'

`grep daemon.*nologin /etc/passwd`
`grep "daemon.*nologin /etc/passwd`
`grep daemon\.\*nologin /etc/passwd`
this will search the passwd file for any instance of the word 'daemon' and display the record if it contains the word 'nologin' after daemon
double quotes or escaping with '\' are required when using `zsh` to correctly interpret the globbing pattern

`grep root /etc/passwd`
this will search for instances of the word 'root' inside the passwd file and display them

`grep ^root /etc/passwd`
this is similar to the previous example but will only show records in the passwd file if 'root' is at the beginning of a line

### Extended Regular Expressions

`grep -E` or using `egrep` allows for more complex patterns

### Additional Operators
- `(abc)` — grouping (no escaping needed)
- `a|b` — logical **OR**
- `+` — **one or more** of the previous item
- `?` — **zero or one** of the previous item

#### Examples

`grep -E "^root|^dbus" /etc/passwd`
this searches for any password records that either being with 'root' or with the word 'dbus'
the '|' means that the record can start with either word

`grep -E "(daemon|s).*nologin" /etc/passwd`
here there is a choice within the subexpression '()' of either 'daemon' or 's' and that there can be anything between the subexpression choice and the word
'nologin' because of '.*' in the text file record

The goal of the next example is to get the TTY keyboard layout from the /etc/vconsole.conf file

- cat command to see the contents of the file
- grep will search for KEYMAP at the beginning of the vconsole.conf file
- the cut command will split the line at the delimeter '=' and select the second string '"gb"'
- finally, tr will strip the double quotes from the string

```bash
[linux_lab@localhost ~]$ cat /etc/vconsole.conf 
KEYMAP="gb"
FONT="eurlatgr"

[linux_lab@localhost ~]$ grep '^KEYMAP=' /etc/vconsole.conf | cut -d = -f 2
"gb"

[linux_lab@localhost ~]$ grep '^KEYMAP=' /etc/vconsole.conf | cut -d = -f 2 | tr -d '"'
gb

[linux_lab@localhost ~]$ lab_language=$(grep '^KEYMAP' /etc/vconsole.conf | cut -d = -f 2 | tr -d '"')
[linux_lab@localhost ~]$ echo $lab_language
gb
```


