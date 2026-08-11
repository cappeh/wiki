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
