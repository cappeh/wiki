# Environment and Shell Variables
the shell can store temporary variables known as `shell variables` where the values are text strings.
they are useful to keep track of values in scripts and can also control the way the shell behaves
    - for example, the shell reads the `PS1` variable before displaying the prompt

**DONT PUT SPACES BETWEEN AROUND THE `=` WHEN ASSIGNING A VARIABLE**

an environment variable is like a shell variable but it is not specific to the shell
all processes have environment variable storage.
the OS will pass all the environment variables to programs that the shell runs, shell variables cannot be accessed in the commands that are run

we assign environment variables with the `export` command

```bash
SOMETHING=hello
export SOMETHING
```

child processes inherit environment variables from their parent. many programs read them for configuration and options
we could put our favourite `less` command-line options in the `LESS` environment variable where it will use these any time we use `less`

## PATH
this is a special environment variable that contains the `command path` which is a list of directories separated by `:` that the shell searches through when locating a command
when we run the `ls` command, the shell searches the directories listed in PATH for the `ls` program
if a program with the same name exists in multiple directories, the shell runs the first matching program

```bash
echo $PATH
# prints for example    /usr/local/bin:/usr/bin:/bin
```

the paths looked in for the above example is `/usr/local/bin/`, `/usr/bin/`, `/bin` 

```bash
# add to the PATH to search through more directories for programs
# first option, adding to the beginning of the path - looked in first
PATH=dir:$PATH

# second option, appending to the end
PATH=$PATH:dir

# these are only temporary, we can start a new shell for the initial PATH variable
```
