
C Programming Language

This note is about C Programming Language, but don't focus on general grammar syntax, The note Mainly Focused on some low-level features in C programming language and how to apply them in low-level programming, especially when implement Some OS Kernel !

This Note also will contain some implementation of GNU glibc standard library ! And also some details in POSIX and C ISO Standard !


### Low Level Features


#### Bit Field 

Bit Field Is a powerful tool in low-level programming especially you need access some memory and data by bits, it allow programmers to define a data structure which based on bit

using the flowing syntax to construct a bit field
```c
struct __bit_field{
u_int32_t _tal : 3;
u_int32_t _base : 5;
u_int32_t _header : 16;
u_int32_t _tail : 2;
};
```

The Memory or Hardware aspect of this is just only one unsigned int which size is 32 bits, it choose the biggest data type in the struct, and only hold one of that type in the memory,  unless the bits of the struct exceeds the size of that type (unsigned int ) it will add another one in this struct !

After define this structure you can access the data of this data structure by bits ! And you also can assign value to the whole structure !

The size of this structure is $(3 + 5 + 16 + 2) = 26$ bits -> $4$ bytes because of alignment !



### C with GNU

