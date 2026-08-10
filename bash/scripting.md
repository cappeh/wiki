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
