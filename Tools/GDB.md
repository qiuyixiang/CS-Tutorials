
GDB For GNU


GDB is a powerful GNU toolchain which is used to debug Applications !
This Note is a precise Guide for GDB debugger !


#### Basic Utilities

Enter GDB
Just Enter The program's name next to the gdb command line ! You will enter the internal of gdb !
```shell
gdb exec1
```
Type `q` To quit GDB command line !


#### Layout

Using layout command you will choose the layout of the coding !
```
(gdb) layout --layout-- 
```

There are different layouts in gdb
- layout asm    apply the assembly language layout (GNU style assembly ! AT&T )
- layout regs         apply the register layout
- layout src          apply the source code layout
- layout split <>   This command can split with other layout 
eg : `layout split asm`

#### Run Programming

To commence execution, you can use the command run followed by any arguments that might be required to run the program
```shell
(gdb) [r]un args (start running program)  
```

The program will pause execution at the first breakpoint encountered. You could step through the lines in your program using next or step or you can continue running the program until the next breakpoint using continue.

```shell
(gdb) [c]ontinue (continue to the next breakpoint)  
(gdb) [n]ext (continue to next line, steps over function calls) 
(gdb) [s]tep (continue to next line, steps into function calls) 
```
#### Break Point

Breakpoints are used in gdb to stop a program whenever a certain point in the program (the breakpoint) is reached. Breakpoints are particularly useful for debugging. 


You can set breakpoints at a function, at a specific line within the file you are currently running, or even at a line within another file.
```shell
(gdb) [b] main (set breakpoint at function main)
(gdb) [b] 101 (set breakpoint at line 101 within file)
(gdb) [b] helper.c:101 (set breakpoint at file helper.c, line 101)  
```
You can use the info command to get information about the breakpoints you have set and the breakpoint numbers they correspond to. You can also delete breakpoints with the delete command.

```shell
(gdb) [i]nfo breakpoints (show all breakpoints)
```

These Commands will delete breakpoints
```
(gdb) [d]elete 1  (delete breakpoint 1)
(gdb) [d]elete  (delete all breakpoints)
```
