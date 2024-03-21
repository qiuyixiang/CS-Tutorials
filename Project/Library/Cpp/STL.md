
### GNU  libstdc++

 Intro To Referenced Library

This is a Simplified implemented C++ Standard Library ! Based On GNU C++ Standard Library ! All the interfaces are based ISO C++ 11 Standard !

The Standard Library Main Path  In mac OS (Homebrew)
>  /opt/homebrew/Cellar/gcc@11/11.4.0

Basic Information About Compiler  : 

The C compiler identification is GNU 11.4.0
The CXX compiler identification is GNU 11.4.0

- Cpp Header File : 
  /opt/homebrew/Cellar/gcc@11/11.4.0/include/c++/11/string

- C Header File : /opt/homebrew/Cellar/gcc@11/11.4.0/lib/gcc/11/gcc/aarch64-apple-darwin23/11/include-fixed/stdio.h

Based Computer Architecture : Mac AArch64

Reference :  GNU libstdc++ Documentation
Book Reference : The C++ Standard Library :  A Tutorial and Reference Second Edition



### QSTL

qstl is a self-developed Standard Library Based on GNU libstdc++ !
Here is some declaration for its namespace and how to use or test its test units !


Main Folder : 

- include : All sources code from qstl project

- test : This is a folder that include all sorts of test units

- build : This folder include all sorts of .o and middle-process file, and the test unit also will be here, you can clean them all using `make clean` !

- Makefile : Using this to Construct Your cloning Project and test its functionality !


### Implementation 

#### Container


#### Algorithm


#### Iterator


#### Allocator