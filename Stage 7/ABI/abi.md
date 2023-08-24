## Aplication Binary Interface (ABI)

An application binary interface (ABI) is the interface between a user program and the kernel.

eXpOS ABI defines:

1. Machine Model
    - architecture specific

2. Memory division into Regions 
    - text, stack, heap and library
    - dependant on both OS and architecture

3. File Format
    - XEXE format
    - OS specific, architecture independant
    - compilers must generate execultable files adhering to this format

4. Low Level System Call Interface
    - gives specs about software interrupt instruction to each system call and
    - order in which arguments must be passed to the system call through program's stack
    - architecture dependant

5. Low Level Runtime Library Interface
    - contains user level routines
    - provide layer of abstraction
    - app level dynamic memory alloc and deallocator are also part of eXpOS library.


### XSM User Level Instruction Set
- describes target language in which a compiler must generate an executable file
- instructions classified into priviliged and unprivileged
