### XEXE Executable File Format
An XEXE executable file in eXpOS consists
1. Header
2. Code

![Alt text](http://exposnitc.github.io/img/exe_file.jpeg)

- maximum size = EXE_SIZE = 2048
- 1st 8 words are for header which describes features of a file

![Alt text](http://exposnitc.github.io/img/header.png)

**XMAGIC** - number indicating the type of executable file. All XEXE files will have magic number 0.

**Entry Point** - contains the virtual address in memory of the first instruction to be executed after OS loader has loaded it. During loading program counter(PC) must be initialized to this address.

**Text Size, Data Size, Heap Size and Stack size** indicates the their sizes to be allocated by OS loader.

**NOTE** : data and stack must be in the same memory area and must be managed by the compiler/app program (means program must contain code to initialize stack pointer). Data size field is ignored.

**Runtime Library**
- library flag is set to 1 in the XEXE file
- if the flag not set then neither memory is allocated nor the library linked to the address space of the process by the eXpOS loader at execution time

### Low Level System Call Interface
#### System Calls
For an application program, 2 stages of executing  a sys call:
1. Before the System call
- calling application must set up the arguments nth user stack
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
- INT instruction changes mode from **User** to **Kernel** mode and passes control to the **Interrupt Routine** corresponding to the sys call
- IRET instruction transfers control back to the user program to the instruction immediately following the INT instruction.
- The interrupt routine number denotes the number of the interrupt routine which handles the system call. An interrupt routine may handle more than one system call.

#### Low Level Runtime Library Interface
The library provides a uniform interface through which an application program can invoke system calls and dynamic memory allocation / deallocation routines by providing the function code and the arguments. The interface hides the details of the interrupt service routines corresponding to the system calls from the application, thereby making them architecture independent. The library also provides user level routines for dynamic memory management (allocation and de-allocation) from the heap region of the application.

![alt text](http://exposnitc.github.io/img/memory_management.png)
- library routine is linked to virtual address 0 and required 4 arguments to be passed through the stack

#### Invoking a library module
```
PUSH Function_Code      // Push Function Code
PUSH Argument_1         // Push argument 1 to the stack
PUSH Argument_2         // Push argument 2 to the stack
PUSH Argument_3         // Push argument 3 to the stack
PUSH R0                 // Push an empty space for RETURN VALUE
CALL 0   // Pass the control to virtual address 0.
    	// (eXpOS loader links the library to virtual address 0)
```
#### After return from library module
The following machine instructions are present after the CALL instruction in the ExpL compiled machine code given
```
POP Ri      // Pop and save the return value into some register Ri
POP Rj      // Pop and discard argument 3
POP Rj      // Pop and discard argument 2
POP Rj      // Pop and discard argument 1
POP Rj      // Pop and discard the function code
            // Now the stack is popped back to the state before call
```