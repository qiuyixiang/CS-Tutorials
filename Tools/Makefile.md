
Make File Handbook !
This is a HandBook About all Rudimentary Skills About Makefile !


#### Basic Rules

A Makefile Some This Basic Properties 
```Makefile
<target>: <dependencies>
[ tab ]<shell_command>
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