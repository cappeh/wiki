## User Input
we use the `read` bash command
we use the `-p` option to provide a prompt to the user followed by a name for the variable to store the input in

`read -p 'Enter your name: ' name`

```
./hello_input_name
Enter your name: Calum
Hello Calum
```
we can also pipe the standard input if you know what is being asked

```
echo Calum | ./hello_input_name
Hello Calum
```

in this example, we bypassed the prompt by providing the input directly in the pipeline

the `yes` program is also useful if you want to answer y to a bunch of questions: `yes | ./some_program_that_needs_y`
`yes` on its own will continuoulsy print `y` to stdout

we could also do this

```
name=$1
echo "hello $name"
```

this will use the first argument given when executing the script
so `./hello_arg calum` will print `hello calum`
$1 is the first argument, $2 is the second, $3 is the third and so on
$0 is the name/path used to invoke the script

we can loop through arguments like this
```
for thing in "$1" "$2" "$3"; do
  echo "thing is $thing"
done
```
this will cause issues if there are more than 3 arguments given as a 4th will be lost

```
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
```
for name in "$@"; do
  ./hello $name
done
```

this is a function example
```
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

```
a=2
b=2

if [[ $a == $b ]]; then
  echo 'a and b are the same'
fi
```

we used '' (single quotes) to echo the text, something we dont have to do. It is useful if there are multiple spaces / whitespace in the text and you want to maintain the spaces
"" (double quotes) are useful if doing variable expansion for example "$a and $b are the same". This would print `2 and 2 are the same`

```
c=2
d=3

if [[ $c != $d ]]; then
  echo 'c and d are NOT equal'
fi
```

in this second example we are checking whether c is not equal to d. If its not equal, print `c and d are NOT the same`

```
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

```
if ls; then
  echo 'the ls command worked'
fi
```

we can also use a `while` loop checking whether a files exists and once its removed, the `while` loop ends

```
while [[ -f file.txt ]]; do
  echo 'file.txt exists and is a file'
  sleep 1
done
```
the sleep function/program will pause for n amount of time before checking the while condition
this will continually print 'file.txt exists and is a file' until the file is deleted with `rm`

`until` is the opposite of while
so if we do `until [[ -f file.txt ]]`, this means until the file exists echo a message

```
until [[ -f file.txt ]]; do
  echo 'file.txt does NOT exist'
done
```

we could also just negate a while statement `while ! [[ -f file.txt ]]`

`if true` and `if false` could also be used. `true` is `0` false is `1`

we could put something like

```
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

```
for thing in {1..5}; do
  echo "thing is $thing"
done
```
this will print `thing is 1`, `thing is 2` etc on a new line until `thing is 5`

there is also a c style for loop for using arithmetic using `((..))`, using parenteses is for mathmatical operations
with this syntax we co not need to expand variables because math syntax is aware of variables

```
max=5
for ((i=0; i<max; i++)); do
  echo "thing is $i"
done
```
