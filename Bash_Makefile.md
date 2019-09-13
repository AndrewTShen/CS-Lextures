# Bash Scripts and Makefiles
**Written by Andrew Shen**  
**Last Updated: 09/12/2019**

### Table of Contents
1. [Introduction](#introduction)
2. [Bash Scripts](#bash-scripts)
3. [Simple Makefile Usage](#simple-makefile-usage)

### Introduction
##### Compiling code manually sucks
As suggested, manually compiling code through the terminal/command prompt is annoying and can take a bit of work as manually typing commands one by one is pretty ridicuously. There are some tricks that we can use to search for commands in our recent history or use the last used command, but these also require some effort. So, in this lecture we will explore some ways that we can more easily compile and run our code without all the difficulties and work of manually typing out each part of the process.

##### Bash Scripts
We will be working in macOS for this section as using Terminal to run bash scripts. What is a bash script? It's a way that we can define sequences of bash commands under a single function, which, among other functions, is placed in a folder at the root of the directory. To this we will `cd ~` to the root of our directory and then `vim .custom_commands.sh`. The `.` prefix makes it so that the file is hidden from normal display methods (finder or `ls`) and the `.sh` extension means that it is a bash file. In this file, we can just start writing functions. Let's start off with a basic example:

```bash
#!/bin/bash

function print_my_input() {
    echo 'Your input: ' $1
}
```
Breaking this down a little bit, the first line specifies tat the script should always be run by bash and not another shell (you only need one of these at the top of a `.sh` file). The `function` keyworks allows us to declare our function, `print_my_input`, and inside this function is our command, which functions exactly as it would if typed into a terminal session.

After placing this into the `.custom_commands` file we can go ahead and exit the file and we will need to "refresh and execute" the contents of the file by using `source .custom_commands` or we can go ahead and restart terminal. This reads and executes the contents of the file and allows them to be used in the current terminal session. Now if we type `print_my_input hello` into our session, we can have it return `Your input: hello`. You can put pretty much any terminal command you want into the `function` so we free to make all sorts of combinations to maximize your performance. We won't spend to much time on this since it's fairly straight forwards, but there are plenty of cool things that you can do with bash scripts. If you have any questions, feel free to search the internet for more extensive guides or message me for questions. Lastly, here's an example I used to build all of my Java labs from AP Computer Science Labs.

```bash
#!/bin/bash

# just defining a few terminal colors
RED='\033[1;31m'
GREEN='\033[1;32m'
BLUE='1;34'
PURPLE='1;35'
CYAN='1;36'
NC='\033[0m' # No Color

function javara() {
    printf "${GREEN}Compiling "$1"...${NC}\n"
    javac $1'.java'
    if [ $? -eq 0 ]; then
        printf "${GREEN}Running "$1"...${NC}\n" 
        java $1
    else
        printf "${RED}Compile Failed.${NC}\n"
    fi
}
```

## Simple Makefile Usage
##### What is the conceptual idea behind using a Makefile?
Makefiles are used for compiled languages, languages that compile code down to a binary in assembly for execution. Perhaps the most popular being C or C++. The premise behind using Makefiles is that they are a way to define "formulas" for compiling and building certain objects. This is helpful in situations where one type of file always builds into another in the same fashion everytime. An example would be that all `*.c` files compile into `*.o` files. Therefore, we can define variables and substitute them into formulas to build them. 

##### Set Up
To create a Makefile, enter into the directory that holds your project. This is a file called `Makefile`. At the top, we put

```make
MAKEFLAGS += -r
MAKEFLAGS += -R
```

This is pretty much magic and you don't need to know what this does. If you'd like to know you can just search it up and this is all the setup that you'll need to start using the `Makefile`.

##### Writing the Formulas

By convention, we will use all capital variable names to hold reference to important variables. We can reference them later to get back the actual alue. For example, we might use single variable assignments such as 

```make
CC=gcc
CC_FLAGS=-Wall
PROGRAM=test
``` 

in order to set out compiler, compiler flags, and program name respectively, or even set variables to lists as

```make
PROGRAM_OBJFILES := \
    test.o \
    ... \
    ... \
```
in the case of object files, or even using wildcards to autofill based on what is in the build directory as

```make
HEADERFILES := $(wildcard *.h)
```

With all of our variables set up we can finally start writing our build formulas. In the conventions of writing `Makefile`s, we put the build target on the left side of a colon, followed by the dependencies on the right side of the right side of the colon, and the commands required to convert the dependencies on the right side into the right side. This looks like

```make
[build target]: [build dependencies]
    [command to build target from dependencies]
```

There are two build steps, converting `test.c` into `test.o`, then converting `test.o` to `test.bin`. So we can write this as 

```make
$(PROGRAM).o: $(PROGRAM).c
	$(CC) -c $(CC_FLAGS) $(PROGRAM).c

$(PROGRAM).bin: $(HEADERFILES) $(PROGRAM).o
	$(CC) -o $@ $(PROGRAM_OBJFILES)
```

The first of the two formulae builds the `test.c` into `test.o`. Notice that the `$(..)` is how we reference the value of the variable. The second of the two formulae compiles the binary from the object file we give it (usually it requires all of the object files, but since we only have one it is sufficient to use this formula for now). We are almost ready to run our program, all we need is a `make` command that we can call. We use the following notation to describe a `make` command that we can call from the command line.

```make
.PHONY: run
run: $(PROGRAM).bin
	./$(PROGRAM).bin
```

This allows us run our program from the command line as `make run`. However, using `make` actually just runs the **first** `make` command in the makefile, so we could also just call `make` instead. Note that this make follows the formula description we provided above: the `make` command run depends on the `test.bin` and we execute it by using `./test.bin`. Now, if we go back to the command line we can use `make` or `make run` to build and run our `test.c` file. What is exciting about `Makefile`s is that they autoregenerate, meaning that if a dependency of a formula is edited, it will automatically rebuild after you run make again; it knows that it outdated. If you don't remove the intermediate builds, it will not rebuild something that has not changed.

##### How to make Makefiles Powerful
So far we have done a barebones example of a `Makefile`. You might have noticed that this isn't much different that building using a bash script. Well, it turns out that there are certain optimizations to make your `Makefile`s much cleaner and more versitile. Take for example the lines below,

```make
%.o: %.c
	$(CC) -c $(CC_FLAGS) $^
```

Breaking this down, we have substituted `$(PROGRAM).o` for `%.o` and `$(PROGRAM).c` with `%.c`. This actually will autofill in this formula with any file name that matches the description. For every `.c` file in the build directory, this formula will offer a **generic** method of building that `.c` into a `.o`. This is not to say that it will build **every** `.c` into a `.o`, but rather, only the ones that are dependencies of something higher up in the dependency structure. Notice that we also replaced the `$(PROGRAM).c` in the command part of the formula with a `$^`. We are allowed to do this as there are a number of special characters we can use to make our code shorter and more readible.

You can read more, about this and `Makefile`s in general at this [link](https://github.com/amjadmajid/Makefile).

## Conclusion
Hopefully, this helps you to understand the power that `Makefiles` provide for making compiling much simpler and less time consuming. There are plenty of references for Makefile and I would strongly suggest that you go check some out if this sort of thing interests you. Both Bash and Make are both languages that each have their own specialties and can take a while to get used to.




