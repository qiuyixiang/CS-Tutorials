
GCC HandBook 

This is a Hand Book For GUN GCC compiler And GNU Tool Chains! 


You can follow each Parts in the GCC Compiler !


### Basic Rules

The gcc compiler supports the use of hundreds of different flags, which we can use to customize the compilation process. Flags, typically prefixed by a dash or two (`-<flag>` or `--<flag>`), can help us in many ways from warning us about programming errors to optimizing our code so that it runs faster.


General Type
```shell
$ gcc <flags> <c-files> -o <executable-name>
```

1. -Wall  
- One of the most common flags is the `-Wall` flag. It will cause the compiler to warn you about technically legal but potentially problematic syntax

2. -Werror
- The `-Werror` flag forces the compiler to treat all compiler warnings as errors, meaning that your code won’t be compiled until you fix the errors. This may seem annoying at first, but in the long run, it can save you lots of time by forcing you to take a look at potentially problematic code

3. -Weatra
- This flag adds a few more warnings (which will appear as errors thanks to `-Werror`, but are not covered by -Wall. Some problems that `-Wextra` will warn you about

Some Debugging Sanitizers

1. -fsanitizer = address
- This flag enables the AddressSanitizer program, which is a memory error detector developed by Google. This can detect bugs such as out-of-bounds access to heap / stack, global variables, and dangling pointers (using a pointer after the object being pointed to is freed). In practice, this flag also adds another sanitizer, the LeakSanitizer, which detects memory leaks (also available via `-fsanitize=leak`)

2. -fsanitizer = undefined
- -This flag enables the UndefinedBehaviorSanitizer program. It can detect and catch various kinds of undefined behavior during program execution, such as using null pointers, or signed integer overflow.

3. -g    (mainly for GDB)
- This flag requests the compiler to generate and embed debugging information in the executable, especially the source code. This provides more specific debugging information when you’re running your executable with gdb or address sanitizers

Optimization

In addition to flags that let you know about problems in your code, there are also optimization flags that will speed up the runtime of your code at the cost of longer compilation times. Higher optimization levels will optimize by running analyses on your program to determine if the compiler can make certain changes that improve its speed. The higher the optimization level, the longer the compiler will take to compile the program, because it performs more sophisticated analyses on your program. These are the capital `O` flags, which include `-O0`, `-O1`, `-O2`, `-O3`, and `-Os`


### Objdump




### Readelf



### Linker

