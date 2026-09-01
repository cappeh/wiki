## User Input
we use the `read` bash command
we use the `-p` option to provide a prompt to the user followed by a name for the variable to store the input in

`read -p 'Enter your name: ' name`

```bash
./hello_input_name
Enter your name: Calum
Hello Calum
```

we can also pipe the standard input if you know what is being asked

```bash
echo Calum | ./hello_input_name
Hello Calum
```

in this example, we bypassed the prompt by providing the input directly in the pipeline

the `yes` program is also useful if you want to answer y to a bunch of questions: `yes | ./some_program_that_needs_y`
`yes` on its own will continuoulsy print `y` to stdout

we could also do this

```bash
name=$1
echo "hello $name"
```

this will use the first argument given when executing the script
so `./hello_arg calum` will print `hello calum`
$1 is the first argument, $2 is the second, $3 is the third and so on
$0 is the name/path used to invoke the script

we can loop through arguments like this

```bash
for thing in "$1" "$2" "$3"; do
  echo "thing is $thing"
done
```

this will cause issues if there are more than 3 arguments given as a 4th will be lost

```bash
for thing in "$@"; do
  echo "thing is $thing"
done
```

the `$@` expands to be an array of all the arguments given when executing the script

## Functions
Functions are basically mini programs
functions also emit an exit code similar to a program. we can use `return <id>` as the last line of a function to express success or failure
and run `$?` to check the return code

in this example we can call a script from another script. So this will call the ./hello script with each argument we give
this runs hello from the current directory './'

```bash
for name in "$@"; do
  ./hello $name
done
```

this is a function example

```bash
greet() {
  local name=$1
  echo "hello $name"
}

for name in "$@"; do
  greet "$name"
done
```

without the local keyword, `name` in the `greet` function would be a global variable. 
so the variable `name` in `greet` without the `local` keyword would overwrite the name variable in the for loop

functions emit data and print to stdout which means we can store the output in a variabe `$(..)` or redirect to a file

## Conditionals

the following snippet will print 'a and b are the same' to standard output

```bash
a=2
b=2

if [[ $a == $b ]]; then
  echo 'a and b are the same'
fi
```

we used '' (single quotes) to echo the text, something we dont have to do. It is useful if there are multiple spaces / whitespace in the text and you want to maintain the spaces
"" (double quotes) are useful if doing variable expansion for example "$a and $b are the same". This would print `2 and 2 are the same`

```bash
c=2
d=3

if [[ $c != $d ]]; then
  echo 'c and d are NOT equal'
fi
```

in this second example we are checking whether c is not equal to d. If its not equal, print `c and d are NOT the same`

```bash
if [[ -f file.txt ]]; then
  echo 'file.txt exists and is a file'
fi
```

the `[[ -f file ]]` test checks whether a file exists. `[[ .. ]]` can also be run as a command
you can determine whether this was true or false by running `$?` 0 is true, 1 is false
if the file exists the echo statement will be printed to stdout
if it does not exist, the output will not be written

`help test` in bash to show what operators are available to test with other than `-f` for zsh, `man zshall | less -p 'CONDITIONAL EXPRESSIONS'`
with zsh, we pipe to less, so we can apply a filter in this example 'CONDITIONAL EXPRESSIONS'

we could put anything after the if command. so we could do

```bash
if ls; then
  echo 'the ls command worked'
fi
```

we can also use a `while` loop checking whether a files exists and once its removed, the `while` loop ends

```bash
while [[ -f file.txt ]]; do
  echo 'file.txt exists and is a file'
  sleep 1
done
```

the sleep function/program will pause for n amount of time before checking the while condition
this will continually print 'file.txt exists and is a file' until the file is deleted with `rm`

`until` is the opposite of while
so if we do `until [[ -f file.txt ]]`, this means until the file exists echo a message

```bash
until [[ -f file.txt ]]; do
  echo 'file.txt does NOT exist'
done
```

we could also just negate a while statement `while ! [[ -f file.txt ]]`

`if true` and `if false` could also be used. `true` is `0` false is `1`

we could put something like

```bash
if apt-get update; then
  echo 'update complete'
else
  echo 'update could not complete'
fi
```

we can also chain commands so we could run `[[ -f file.txt ]] && echo it exists`
this will echo `it exists` aslong as the file also exists

you can also do `[ -f file.txt ]` with single brackets or `test -f file.txt`. while these are valid they are more for POSIX
generally use `[[..]]`

## For Loops

we can use `{1..5}` or `{a..f}` syntax within a for loop to exand them to 1 through 5 or a through f

```bash
for thing in {1..5}; do
  echo "thing is $thing"
done
```

this will print `thing is 1`, `thing is 2` etc on a new line until `thing is 5`

there is also a c style for loop for using arithmetic using `((..))`, using parenteses is for mathmatical operations
with this syntax we co not need to expand variables because math syntax is aware of variables

```bash
max=5
for ((i=0; i<max; i++)); do
  echo "thing is $i"
done
```

## Case Statements
case is a way to match on something specific and execute the commands within that case statement. After a match, it breaks out of the case.
it will only match on the first statement that matches the variable so if the case is `dave` if the first statement is `d*)` then it will match on this even if the second statement is `dave)`
this is the default behaviour when ending the statement with `;;`
when ending the statement with `;&` this means **always fallthrough**. after a match it will fallthrough to the next statement and print that whether it matches or not
when ending with `;;&` it will check all statements for a match. If there is a match execute the command in the statement. This would also match on the default `*)`
we can use a wildcard `*)`

```bash
s=$1

case "$s" in
  dave)
    echo hi dave;;
  buddy)
    echo ohh there he is;;
  guy)
    echo uh oh here comes trouble;;
esac
```

```bash
for name in "$@"; do
  case "$name" in
    d*) hello "$name";;
    b*) hello "$name";;
    *)  goodbye "$name";;
  esac
```

we could also do `d* | *b)` similar to an `OR` statement

## Indexed Arrays

we use the syntax `array=()`

`array=(foo bar baz)`, an `array` with 3 items seperated with a space
if we expand it `echo $array`, you will only get the first element of the array
we can use `[]` notation for an element like in other languages but `{}` needs to surround the variable: `echo "${array[0]}"`
with 3 elements (indexing starts at 0), if we do `echo "${array[3]}` it will echo an empty line at it just treats it like a space
we can also use negative numbers `[-1]` will echo the last element

we can put the index we want in a variable

```bash
idx=2
echo "${array[$idx]}"
```

the dollar sign is optional when indexing with a variable `[idx]` is perfectly valid in the above code

we can use the following to print all the items in an array:
`echo "*: ${array[*]}"`
`echo "@: ${array[@]}"`

if we loop over the items in the array:

```bash
for item in "${array[*]}"; do
  echo "item is: $item"
done
```

this will print one line: `item is: foo bar baz`
`*` is when we are stringifying the elements in an array (when we want a string at the end of it)

`@` is when we want to treat it like an actual array where we want to iterate over it
**the @ symbol in this format `"${array[@]}` is the correct way to loop over an array in bash**

we can explicitly create an array with `delcare`, `declare -a array=(foo bar baz)`

to copy an array, we do the following:
`second_array=("${first_array[@]}")`. this creates a new array that includes all the items in the first array
if we dont surround with `()`, `second_array` will be treated as one long string
we can also add/extend to the array like this: `second_array+=(bat cat ls)`

in the terminal we can do the following which pretty prints the array. It provides information about the type of variable and its contents

```bash
array=(foo bar baz)
declare -p array
```

this will print `declare -a array=([0]="foo" [1]="bar" [2]="baz")`
it shows how the array was actually created, which is also a valid way of creating an array in a script or the shell
we can also do `declare -a array=([45]="foo")` where we can retrieve the array with `echo "${array[45]}"`

this syntax will get the length of the array: `echo "${#array}"`
this would show you the length of a specific element `echo "${array[1]}"`

so we can also get the length of other variables with this method

```bash
var="hello world"
echo "${#var}"
```

## Associative Array
this is like a dictionary in Python. Key Value pairs

we start an array like this `declare -A`. **We Must Do This**

this was available from Bash 4.X so if we want to check in a script whether this is valid in whatever version is runnning:

```bash
if ! declare -A arr; then
  echo "incorrect bash version, unable to declare an associative array"
  exit 1
fi
```

```bash
declare -A arr

arr[foo]=1
arr[bar]=2
arr[baz]=3
```

we can echo these like `echo "${arr[foo]}"`

we can also use `"${arr[*]}"` or `"${arr[@]}"` which will just print the **values**
we can print the keys only with `echo "${!arr[*]}"`. we usually want the `*` to stringify the output

so we can do the following to loop over the array

```bash
for key in "${arr[@]}"; do
  value=${arr[$key]}
  echo "got $key=$value"
done
```

the `$` is required when getting a value from the array in a loop

## IFS Variable (Internal Field Separator)
this is built in to bash which is a special argument
by default this is set to "Space, New Line Character, Tab" (whitespace characters)

```bash
array=(foo bar baz)
echo "array is: ${array[*]}"
```

in the above, the array is stringified using the first variable of the IFS `space`

so it prints `array is foo bar baz`

```bash
array=(foo bar baz)
IFS=hello
echo "array is: ${array[*]}"
```

in this new example the script will write `array is: foohbarhbaz`
because `h` is the first character in the new IFS

we can follow this up with `unset IFS` to return this back to its default

## Command Substitution
command substitution is a way of storing the output of commands such as `whoami` into a variable

```bash
#!/usr/bin/env bash

user=`whoami`
echo "$user"
```

we can use `` to get the output of the command. in the above example "calum" would be displayed

we can also nest the commands. However, it starts to become a little messy

```bash
#!/usr/bin/env bash

whoami
echo `echo \`whoami\``
```

the first 'whoami' will print the user to the screen.
the second line will call echo from echo. we also need to escape the whoami because we are nesting

the more modern way is to use `$(..)` notation
`echo $(whoami)`

we can also nest this like before `echo $(echo $(whoami))`

```bash
#!/usr/bin/env bash

my-func() {
    echo hi
}

thing=$(my-func)
echo "thing is $thing"
```

when we use `$(..)`, the command is run in a subshell

```bash
#!/usr/bin/env bash

i=5

my-func() {
    i=6
    echo hi
}

thing=$(my-func)
echo "thing is $thing"
```

in the above, with command substitution, the global variable i is not modified. once the command substitution has ended, `i=6` is dropped
if we just ran `my-func` instead of using command substitution, the variable would be modified

in bash 5.3, we can use this syntax `thing=${ my-func; }` which will run the command in the main shell instead of a subshell

## Arithmetic Expression

`help '(('` also look at `help let`
the `(())` is for arithmetic or math expression

```bash
#!/usr/bin/env bash

thing=$(( 2 + 2 ))
echo "thing is $thing"
```

you can also use this from the terminal `echo $(( 57 * 2 ))`

```bash
#!/usr/bin/env bash

a=2
b=3

echo "$(( $a + $b ))"
```

you can also use the echo command with out expanding the variables inside the '((', such as `echo $(( a + b ))`, bash understands that a and b are variables
you could also do `(( c = a + b ))` and `echo $c`

```bash
i=2
(( i << 5))         # this == 64

(( i *= 5 ))        # this == 10
```

with `(())` we can do a ternary operation

```bash
#!/usr/bin/env bash

a=2
b=3

(( max = a > b ? a : b))
echo "i = $max"
```

bash will run the expression and after the `?`, if its true return a otherwise return b
the expression could also be a failure. Anything that == 0 in bash is considered a fail
leading 0's can also be an issue because bash would treat it as octal. `a=010; echo $(( a ))` would print 8
if we want that to be a decimal number we do `echo $(( 10#$a ))`

## Process Substitution

this is the syntax `$<(..)`

```bash
#!/usr/bin/env bash

words=$(grep d /usr/share/dict/words)

i=0
for word in $words; do
    echo "$word"
    ((i++))
done

echo "found $i words"
```

whilst the above code will work, it is not the correct way to do this
also if `""` is not surrounding the `$words` and the code still works, its technically incorrect. Adding `""` adds more resilliency
when we surround `$words` in quotes, all the words will be printed, but only 1 will be found not the 61055 that there actually would be
the above will read all words into memory

```bash
#!/usr/bin/env bash

i=0
while read -r word; do
    echo "$word"
    (( i++ ))
done < <(grep d /usr/share/dict/word)

echo "found $i words"
```
this streams it rather than reading it all into memory but we cant easily detect whether grep completed successfully
