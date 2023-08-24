## Introduction to ExpL

- ExpL permits application programs to call the function exposcall() that implements the high level library interface to the OS

- Application programs must use this library interface to invoke eXpOS system calls

- ExpL library file library.lib contains assembly language implementation of the library
    - occupies 2 pages of memory
    - library code must be pre-loaded to the XSM disk blocks 13 and 14
- OS start up code is supposed to load this code into memory pages 63 and 64 from disk blocks 13 and 14
- The compiler expects that the library will be loaded to the logical address 0 of the address space of the program
- library linkage must be done correctly for ExpL program to run properly