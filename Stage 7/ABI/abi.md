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

### XEXE Executable File Format
An XEXE executable file in eXpOS consists
1. Header
2. Code
![Alt text](http://exposnitc.github.io/img/exe_file.jpeg)

- maximum size = EXE_SIZE = 2048
- 1st 8 words are for header which desccribes features of a file
![Alt text](http://exposnitc.github.io/img/header.png)

**XMAGIC** - number indicating the type of executable file. All XEXE files will have magic number 0.

**Entry Point** - contains the virtual address in memory of the first instruction to be executed after OS loader has leoaded it. During loading program counter(PC) must be initialized to this address.

**Text Size, Data Size, Heap Size and Stack size** indicates the their sizes to be allocated by OS loader.

**NOTE** : data and stack must be in the same memory area and must be managed by the compiler/app program (means program must contain code to initialize stack pointer). Data size field is ignored.

**Runtime Library**
- library flag is set to 1 i the XEXE file
- if the flag not sset then neither memory is allocated nor the library linked to the address space of the process by the eXpOS loader at execution time

### Low Level System Call Interface
#### Synstem Calls
For an application program, 2 stages of executing  a sys call:
1. Before the System call
- calling application must set up the arguments nthe user stack
2. After the system call
- return value of the system call must be extracted from stack

#### Invoking a system call
A user program invokes a system call by first pushing the sys call no. and then arguments to stack and then invoke INT machine instruction
- eXpOS ABI stipulates that the no. of arguments push into stack is fixed at 3.
```
PUSH System_Call_Number // Push system call no.
PUSH Argument_1         // Push first argument to stack
PUSH Argument_2         // Push second argument to stack
PUSH Argument_3         // Push third argument to stack
PUSH R0                 // Push an empty space for RETURN VALUE
INT number              // Inovke corresponding INT instruction 4 - 18
```

![Alt text](http://exposnitc.github.io/img/system_call_stack1.png)
![Alt text](http://exposnitc.github.io/img/system_call_stack2.png)

- The INT instruction in XSM will push the value of IP + 2 on tp the stack.
- INT instruction changes mode from **User** to **Kernel** mode and passes control to the **Interrupt Ruotine** corresponding to the sys call
- IRET instruction transfers control back to the user program to the instruction immediately following the INT instruction.
- The interrupt routine number denotes the number of the interrupt routine which handles the system call. An interrupt routine may handle more than one system call.

#### Low Level Runtime Library Interface
The library provides a uniform interface through which an application program can invoke system calls and dynamic memory allocation / deallocation routines by providing the function code and the arguments. The interface hides the details of the interrupt service routines corresponding to the system calls from the application, thereby making them architecture independent. The library also provides user level routines for dynamic memory management (allocation and de-allocation) from the heap region of the application.

![alt text](http://exposnitc.github.io/img/memory_management.png)
- library routine is linked to virtual address 0 and required 4 arguments to be passwd through the stack

#### Invoking a library module
```
PUSH Function_Code          // Push Function Code
PUSH Argument_1             // Push argument 1 to the stack
PUSH Argument_2             // Push argument 2 to the stack
PUSH Argument_3             // Push argument 3 to the stack
PUSH R0                     // Push an empty space for RETURN VALUE
CALL 0                  	// Pass the control to virtual address 0.
    		                // (eXpOS loader links the library to virtual address 0)
```
#### After return from library module
The following machine instructions are present after the CALL instruction in the ExpL compiled machine code given
```
POP Ri           // Pop and save the return value into some register Ri
POP Rj           // Pop and discard argument 3
POP Rj           // Pop and discard argument 2
POP Rj           // Pop and discard argument 1
POP Rj           // Pop and discard the function code
                 // Now the stack is popped back to the state before call
```