
GNU Toolchain HandBook 

This is a Hand Book For GUN GCC compiler And GNU Tool Chains! 

You can follow each Parts in the GCC Compiler !


### GCC

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

4. -v (--verbose)
- display all compilation details

#### Debugger Flags 
----------
##### Sanitizers

Some Debugging Sanitizers

1. -fsanitizer = address
- This flag enables the AddressSanitizer program, which is a memory error detector developed by Google. This can detect bugs such as out-of-bounds access to heap / stack, global variables, and dangling pointers (using a pointer after the object being pointed to is freed). In practice, this flag also adds another sanitizer, the LeakSanitizer, which detects memory leaks (also available via `-fsanitize=leak`)

2. -fsanitizer = undefined
- -This flag enables the UndefinedBehaviorSanitizer program. It can detect and catch various kinds of undefined behavior during program execution, such as using null pointers, or signed integer overflow.

3. -g    (mainly for GDB)
- This flag requests the compiler to generate and embed debugging information in the executable, especially the source code. This provides more specific debugging information when you’re running your executable with gdb or address sanitizers

##### Common AddressSanitizer Errors

Here is a list of common ASAN errors:

1. **heap-buffer-overflow/underflow**

	_Reason:_ Your code accessed memory outside a valid heap (dynamic lifetime) allocation.
	
	_Typical bugs that cause this:_ Off-by-one error on size passed to malloc(); incorrect index calculation when accessing heap memory via array subscript or pointer arithmetic; copied data larger than allocation into an allocation.
	
	_Next steps:_ Check which allocation is affected (ASAN tells you this), and by how much. Investigate if you allocated too little memory or wrote too much data.

2. **stack-buffer-overflow/underflow**

	_Reason:_ Your code accessed memory outside a valid memory region on the stack (automatic lifetime). Often involves arrays.
	
	_Typical bugs that cause this:_ Allocated a fixed-size array on the stack, but tried to write more data into it (e.g., 1005-character name into 1000-character array); off by one error on indexing; incorrect index calculation; writing larger type into variable of smaller type (e.g., char* into int, or int into char); incorrect pointer arithmetic with stack addresses.
	
	_Next steps:_ Check the backtrace to see what functions and variable’s space you exceeded. Depending on the amount of overflow, ASAN may not be perfect at reporting the source, but it does usually correctly report the place where the memory access happens.

3. **dynamic-stack-buffer-overflow/underflow**

	Same as above, but for a dynamically-sized array in the stack segment.

4. **global-buffer-overflow/underflow**

	Same as above, but for global (static lifetime) variables.

5. **SEGV on unknown address**

	_Reason:_ Your code dereferenced something as an address that isn’t a valid address in this program. Commonly involves address 0x0, the NULL pointer.
	
	_Typical bugs that cause this:_ You didn’t initialize a pointer variable, and ended up dereferencing some garbage left over in memory as an address; a pointer was NULL at runtime but got dereferenced; your code accidentally overwrote a pointer variable with data and you’re dereferencing that data (e.g., a deref of a small integer like 0x1).
	
	_Next steps:_ Find the pointer in question from ASAN’s back trace, and figure out where its value comes from. This may involve checking where it gets assigned, and debugging with GDB or print statements where the value of the pointer variable changes (this could happen due to a seemingly unrelated assignment if that assignment corrupts the pointer).

6. **heap-use-after-free**

	_Reason:_ Your code accessed memory in a heap allocation after it was already freed.
	
	_Typical bugs that cause this:_ You left a pointer to a free’d heap allocation in a data structure and later dereferenced it; you passed a pointer to already freed heap memory to somewhere that ended up dereferencing the pointer.
	
	_Next steps:_ Look at where the pointer is dereferenced (ASAN’s backtrace tells you), and then see where the allocation got freed (also in the back trace). Try to figure out what happened in between, and how the pointer to the now-dead dynamic lifetime memory continued to exist.

7. **double-free**

	_Reason:_ Your code calls free() twice with the same address as an argument, and the dynamic lifetime memory is dead on the second call. (Note that calling free() multiple times with the same address is fine if you called malloc() in between and it gave you the memory at this address again).
	
	_Typical bugs that cause this:_ Multiple cleanup code paths that clear up the same resources; pointers stored in data structures that already got freed elsewhere.
	
	_Next steps:_ Figure out where the offending free() happens, and where the prior free() call happened (both are in the ASAN backtraces). Then understand your logic and why both calls happened on the same pointer; change it to avoid this.

8. **stack-use-after-return**

	_Reason:_ You returned a pointer to a stack (automatic lifetime) variable, whose lifetime has ended by the time the calling function gets to run again.
	
	_Typical bugs that cause this:_ Returning a pointer to a local variable, or into a local array.
	
	_Next steps:_ Check what you’re returning in the location indicated by the backtrace and trace it back to where it comes from.

9. **LeakSanitizer error**
	
	_Reason:_ Your program did not call free() for a heap-allocated memory region before exiting.
	
	_Typical bugs that cause this:_ You forgot a free() call; or you lost track of a pointer that you needed to free, either by overwriting it or by not storing/passing it for another part of your code to free it.
	
	_Next steps:_ Find out which allocation is affected, and track where the pointer returned from malloc() gets passed or stored. Check that all code paths to the exit of the program end up calling free() on this pointer.


#### Optimization Flags

In addition to flags that let you know about problems in your code, there are also optimization flags that will speed up the runtime of your code at the cost of longer compilation times. Higher optimization levels will optimize by running analyses on your program to determine if the compiler can make certain changes that improve its speed. The higher the optimization level, the longer the compiler will take to compile the program, because it performs more sophisticated analyses on your program. These are the capital `O` flags, which include `-O0`, `-O1`, `-O2`, `-O3`, and `-Os`


### AS

As is a assembler for GNU ! It generate Object File ! It also have some special features and flags !

Input :                 Assembly Code File
Output :              Binary Object File 
#### Format 

There are some different Flags for output format !

Notice : If you want to generate Intel style Assembly, you can use `gcc -masn=intel` flag, but `as` don't support this command !

- --32/--64/--x32         generate 32bit/64bit/x32 object
- -march=CPU generate code for CPU 
	generate code for CPU and EXTENSION, CPU is one of:
   default, push, pop, generic32, generic64, i386, i486,
   i586, i686, pentium, pentiumpro, pentiumii,
   pentiumiii, pentium4, prescott, nocona, core, core2,
   corei7, iamcu, k6, k6_2, athlon, opteron, k8,
   amdfam10, bdver1, bdver2, bdver3, bdver4, znver1,
   znver2, znver3, znver4, btver1, btver2
   
### LD


### Objdump

------------
#### Basic Rules 

Objdump is a disassembler Tool, It will transfer binary file to assembly code . Note : The file must be obj format or elf format !

```shell
objdump -d hello
```

-d option only displays assembled contents of executable sections. A section is a block of memory that contains either program code or data.

If we want to Save the output of an objdump file we can use this command 
```shell
objdump -d hello | less.hex
objdump -d hello > dump.hex
```


If We want to Combine source code with assembly code, we must add `-g` flag when compile source code !
```shell
gcc -g -O0 -c hello.c -o hello.o
objdump -S hello.o
```

The default syntax used by objdump is AT&T syntax. To change it to the familiar Intel syntax:
```shell
objdump -M intel -D hello | less
```

#### Flags 
There are some useful flags in Objdump : 

- -d option only displays assembled contents of executable sections, disassemble of text section

- -D option displays assembly contents of all sections.

- -z display all zero bytes

- -S option display both source code and assembly code, but must be compiled with `-g` flag 

- -j --section=NAME             Only display information for section NAME

- -M option change the output format file of assembly code 

	The following i386/x86-64 specific disassembler options are supported for use
	with the -M switch (multiple options should be separated by commas):
	
	x86-64                   Disassemble in 64bit mode
	i386                       Disassemble in 32bit mode
	i8086                     Disassemble in 16bit mode
	
	att                         Display instruction in AT&T syntax
	intel                       Display instruction in Intel syntax
	
	att-mnemonic           Display instruction in AT&T mnemonic
	intel-mnemonic        Display instruction in Intel mnemonic
	
	addr64                 Assume 64bit address size
	addr32                 Assume 32bit address size
	addr16                 Assume 16bit address size
	
	data32                 Assume 32bit data size
	data16                 Assume 16bit data size
	
	suffix                   Always display instruction suffix in AT&T syntax
	
	amd64                Display instruction in AMD64 ISA
	intel64                 Display instruction in Intel64 ISA

### Readelf

Readelf Can Print Some Information about ELF Format File ! The Functionality of this function is similar with Objdump. But they also have some difference !


#### Flags
There are some common Flags in Readelf

- -x  (section name)  Dump the contents of section <number|name> as bytes

### Linker


### NASM 

NASM is a excellent Assembler Mainly used for x86 assembly language ! It is not belong to GNU but is a useful tool for compile your Assembly File !

#### Basic Rules

```shell
nasm -f bin test.asm -o test
```
-f option specifies the file format, e.g. ELF, of the final output file. But in this case, the format is bin, which means this file is just a flat binary output without any extra information. That is, the written assembly code is translated to machine code as is, without the overhead of the metadata from file format like ELF. Indeed, after compiling, we can examine the output using this command:

```shell
hd test
```
hd (short for hexdump) is a program that displays the content of a file in hex format. And get the following output

> Nots : That Nasm Generate 16 bits code by default, if you want to generate 32-bit code you must add some macro instructions 
```shell
bits 32
```


#### Flags
Some Flags In NASM

- -f option specifies the output file format 
	bin                  Flat raw binary (MS-DOS, embedded, ...) default 16-bits
	
	ith                  Intel Hex encoded flat binary
	
	elf32                ELF32 (i386) (Linux, most Unix variants)
	elf64                ELF64 (x86-64) (Linux, most Unix variants)
	elfx32               ELFx32 (ELF32 for x86-64) (Linux)
	
	win32                Microsoft extended COFF for Win32 (i386)
	win64                Microsoft extended COFF for Win64 (x86-64)
	
	macho32              Mach-O i386 (Mach, including MacOS X and variants)
	macho64              Mach-O x86-64 (Mach, including MacOS X and variants)


### Other Tools

#### File Content

To check the contents of a binary (non-text) file, we recommend a _hexdump_ tool like xxd
```shell
xxd <file name>
```
The above command will “dump” the contents of the file in hexadecimal format, so you can see what it contains even if the data does not consist of printable characters.

To efficiently compare and contrast the contents of two text files we recommend diff
```shell
diff -u <file1 name> <file2 name>
```
If there exists a difference between the files, the above command will produce the location of the difference, and the information that is different in the two files.

However, `diff` does not work well on non-text files. But you can combine it with a hexdump tool to compare those!

```shell
xxd <file 1 name> > file1.hex 
xxd <file 2 name> > file2.hex 
diff -u file1.hex file2.hex
```
