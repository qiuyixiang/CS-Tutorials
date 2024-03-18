
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
- layout regs    apply the register layout
- layout src      apply the source code layout


#### Run Programming

Use `r` command the gdb will execute through until it meet the next break point or it will ends the program 


#### Break Point

Breakpoints are used in gdb to stop a program whenever a certain point in the program (the breakpoint) is reached. Breakpoints are particularly useful for debugging. 


You can set breakpoints at a function, at a specific line within the file you are currently running, or even at a line within another file.
```shell
(gdb) [b] main (set breakpoint at function main)
(gdb) [b] 101 (set breakpoint at line 101 within file)
(gdb) [b] helper.c:101 (set breakpoint at file helper.c, line 101)  
```
You can use the info command to get information about the breakpoints you have set and the breakpoint numbers they correspond to. You can also delete breakpoints with the delete command.

![[Pasted image 20240318155655.png]]
```shell
(gdb) [i]nfo breakpoints (show all breakpoints)
```

These Commands will delete breakpoints
```
(gdb) [d]elete 1  (delete breakpoint 1)
(gdb) [d]elete  (delete all breakpoints)
```
