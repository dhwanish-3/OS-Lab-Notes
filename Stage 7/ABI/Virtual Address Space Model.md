

### Virtual Address Space Model

![Alt text](http://exposnitc.github.io/img/process_model.png)

#### Shared Library
- X_LSIZE = 1024 words
- address space in the process = 0 to 1023

#### Heap
- memory pool from which dynamic memory allocation is done by allocator routines in shared library
- when a process executes the Fork sys call, this region will be shared b/w parent and child process
- X_HSIZE = 1024 words
- address space in the process = 1024 to 2047

#### Code
- header and code part of XEXE file
- first 8 words of XEXE contains header
- rest of the code region is XSM instructions
- X_CSIZE = 2048 words
- address space in the process = 2048 to 4095

#### Stack
- space for runtime stack
- parameters and variable associated with functions in a program are allocated in the stack
- 
- X_SSIZE = 1024 words
- address space in the process = 4096 to 5119
