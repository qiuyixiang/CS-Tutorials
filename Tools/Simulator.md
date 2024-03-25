

Simulator 

This Notes is about some hardware simulators For Developing your own Operating System and Developing Environment


### Bochs

#### basic Configuration

Using command `bochs` to save configuration file into your dictionary ! And you also can use `bximage` to create your Virtual Disk Image file !

follow the each step
```shell
==========================================================
1. Create new floppy or hard disk image
2. Convert hard disk image to other format (mode)
3. Resize hard disk image
4. Commit 'undoable' redolog to base image
5. Disk image info
0. Quit

Please choose one [0] 1  
Create image
Do you want to create a floppy disk image or a hard disk image?
Please type hd or fd. [hd] 

  
What kind of image should I create?
Please type flat, sparse, growing, vpc or vmware4. [flat] 
 
Choose the size of hard disk sectors.
Please type 512, 1024 or 4096. [512] 

Enter the hard disk size in megabytes, between 10 and 8257535
[10] 10

What should be the name of the image?
[c.img] bochs_vhd.img

Creating hard disk image 'bochs_vhd.img' with CHS=20/16/63 (sector size = 512)

The following line should appear in your bochsrc:
ata0-master: type=disk, path="bochs_vhd.img", mode=flat
```


#### Display Library 

##### Mac OS
In Mac OS Environment your can use `sdl2` graphical library !
```c
config_interface: textconfig
display_library: sdl2
```


#### Example

> Warning : Because different Bits-Mode will generate different binary code So When Using bochs Magic Break Point Make Sure That you are using `xchg bx, bx `rather than 32-bit `xchg ebx, ebx`In this special case bochs's Magic Point will not work appropriately !!!


This is the easiest test sample to test your configuration of bochs

This Example Only For Intel style (NASM) ! And Using NASM Assembler !
```asm
; (Intel nasm)
XCHG BX, BX
MOV AX, 0x7c00
JMP $

db "Hello World !"

times 510 - ($ - $$) db 0x00
db 0x55, 0xAA
```
This Example is For GNU AT&T Style And Using GCC Compiler (as assembler !) To compile This File 

```
# (GNU AT&T)
.code16

start:
XCHG %BX, %BX
MOV $0x7c00, %AX
MOV $0xb800, %BX
MOV $0x7c00, %DX

loop:
JMP loop

.ascii "This is a Basic Example of GNU AS !"

end:
.fill 510 - (end - start), 1, 0
.byte 0x55, 0xAA
```

To compile and write to your virtual hard disk just follow the steps

compile:
```shell
nasm -f bin task.asm -o target.bin
```
write:
```shell
$(DD) if=target.bin of=boch.hd bs=512 count=1 iseek=0 conv=notrunc
```

### Qemu