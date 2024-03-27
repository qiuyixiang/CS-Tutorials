
Make File Handbook !
This is a HandBook About all Rudimentary Skills About Makefile !


### Basic Rules

A Makefile Have Some Basic Properties 
```Makefile
<target>: <dependencies>
[ tab ]<shell_command>
```


This is a usually Used Template for multi-file compilation
```Makefile
GCC = gcc
FLAGS = -Wall -Werror -Wextra -O0

PROGRAM = task1 task2
ALLPROGRAM = $(PROGRAM)
all : $(ALLPROGRAM)

%.o : %.c
	$(GCC) $(FLAGS) -c $< -o $@

task1 : task1.o
	$(GCC) $(FLAGS) $< -o $@

  
task2 : task2.o
	$(GCC) $(FLAGS) $< -o $@

clean :
	rm -f *.o
.PYONY : all clean
```

Automatic Deduction

Makefile have a powerful utility that it can automatically deduct what operation the target need !
For example : 
```Makefile
display.o : defs.h buffer.h
```
When you declare this line , Makefile can automatically deducted with
```shell
cc     hello.c   -o  hello
```


#### Variables

Makefiles support defining variables, so that you can reuse flags and names you commonly use. `MY_VAR = "something"` will define a variable that can be used as `$(MY_VAR) or ${MY_VAR}` in your rules. A common way to define flags for C program compilation is to have a `CFLAGS` variable that you include whenever you run gcc. For example, you can then rewrite your target like this:

```Makefile
CFLAGS = -Wall -Werror

reverse_test: test_reverse.c reverse.c
    gcc $(CFLAGS) test_reverse.c reverse.c -o reverse_test
```

**Automatic Variables** are special variables called automatic variables that can have a different value for each rule in a Makefile and are designed to make writing rules simpler. They can only be used in the ***command portion*** of a rule!

Here are some common automatic variables:
- `$@` represents the name of the current rule’s target.  
- `$^` represents the names of all of the current rule’s dependencies, with spaces in between.  
- `$<` represents the name of the current rule’s first dependency.

So The Symbolic Picture : 
```Makefile
$@ : $^  # ($< means the first depedency)
```

Pattern Rules

The last Makefile technique we’ll discuss are pattern rules. These are very commonly used in Makefiles. A pattern rule uses the `%` character in the target to create a general rule. As an example:

```
file_%: %.c
    gcc $< -o $@
```

The `%` will match any non empty substring in the target, and the `%` used in dependencies will substitute the target’s matched string. In this case, this will specify how to make any `file_<name>` executable with another file called `<name>.c` as a dependency. If `<name>.c` doesn’t exist or can’t be made, this will throw an error.

#### Phony Targets

There are also targets known as ‘phony’ targets. These are targets that themselves create no files, but rather exist to provide shortcuts for doing other common operations, like making all the targets in our Makefile or getting rid of all the executables that we made.
```Makefile
.PHONY: target1 target2 etc.
```

Usually use of Phony Target

`all: target1 target2 target3`
As you can see, there are no shell commands associated with the all target. In fact, we don’t need to include shell commands for all, because by including each target (target1, target2, target3) as dependencies for the all target, the Makefile will automatically build those targets in order to fulfill the requirements of all.

**`clean` target**  
We also have a target for getting rid of all the executables (and other files we created with make) in our project. This is the clean target.

The clean target generally looks like this:

```Makefile
.PHONY : clean

clean:
    rm -f exec1 exec2 obj1.o obj2.o
```


#### Assignment

There are different types of assignment in Makefile

- = Define a variable the value of this variable often changeable which means the value of this variable may change during the process of the Makefile 

- := Define and assign when fist encounter this variable, usually use to define some constant value ! 


### Function 

There are some build-in functions in Makefile, using them can boots your work productivity !

##### Basic Rules

```Makefile
$(<function> <arguments>)
```


##### String Function

##### patsubst
```Makefile
$(patsubst <pattern>,<replacement>,<text>)
```
utility : search words in text, judge whether conform with the pattern , if conform it will be replaced by the replacement !

return : return every string that be replaced

example : 
```Makefile
$(patsubst %.c,%.o,x.c.c bar.c)
```
return value : x.c.o bar.o


Some Practical Coding Using This Technique !
```shell
# Source Code Dir
SOURCE_DIR := ./Source
# Target Output file Dir
BUILD_DIR := ./Build

# Source : A List of All Source Code File Name
SOURCE := $(wildcard $(SOURCE_DIR)/*.c)

# Output which needs to transfer to target file name .o
BUILD_LIST := $(patsubst $(SOURCE_DIR)/%.c, $(BUILD_DIR)/%.o, $(SOURCE))

CC := gcc

all :
	make build

# All *.o file which in BUILD_DIR will compile in this process
$(BUILD_DIR)/%.o : $(SOURCE_DIR)/%.c
	$(CC) -c -O0 $< -o $@


BUILD_DEPEDENCY := $(BUILD_LIST)
build : $(BUILD_DEPEDENCY)

.PHONY : all clean
```
##### File Function

###### Wildcard

The `wildcard` function in Makefile is used to match file name patterns, allowing you to obtain a list of files that match a specified pattern. Typically, the `wildcard` function is used to find files that match a certain pattern and then assign these files as the value of a variable for use in Makefile rules.
The syntax is as follows:
```Makefile
$(wildcard pattern)
```
Here, `pattern` is a file name pattern that can include wildcards such as `*` and `?`.
For example, if you have some C source files (`.c` files), you can use the `wildcard` function to obtain a list of these files:
```Makefile
SOURCES := $(wildcard *.c)
```
In this example, `$(wildcard *.c)` will match all files with a `.c` extension in the current directory and assign the list of matched files to the variable `SOURCES`.

You can also use the `wildcard` function in a specific directory:

```Makefile
SOURCES := $(wildcard src/*.c)
```
This will match all files with a `.c` extension in the `src` directory and assign the list of matched files to the variable `SOURCES`.
Once you have obtained the file list, you can use these files in Makefile rules, such as compiling them into object files, linking them into executable files, and so on.


### Instruction

#### Include 

One Makefile also can include other Makefile file just like C programming '#include' Instructions
The Include Instructions looks like this style : 
```Makefile
include <filenames>...
```
Other Makefile usually have suffix with .mk ! means this is also a Makefile file !