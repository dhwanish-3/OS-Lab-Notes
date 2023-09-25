### Implementation of Exec system call

- System call Number(Exec) = 9
- Interrupt Routine 9

### Modifications to Boot Module
- eXpOS memory layout for the XSM machine sets pages 76 - 127 as free 
- remaining are reserved for OS modules, OS data structures, interrupts, system calls, code region of special processes (INIT, shell, etc.), OS library etc.

- 76 - 127 pages should be marked free at initialization of memory free list

- memory manager shall be doing allocation and deallocation only from the free pool(76 - 127)

- The boot module/OS startup code further allocates space for the user-area page and stack pages for INIT and IDLE processes from this free pool. The INIT process require two additional pages for the heap. The pages allocated to INIT process are- stack-76, 77, heap-78, 79, user area page-80 and for IDLE process- stack-81, user area page-82

- Finally effective free pool at the end of OS initialization process starts from memory page 83

***Q1. Why does exec reclaim the same user area page for the new process? (As done in step 7 of exec system call implementation.)***

- Since the page storing the kernel context has been de-allocated, before making any function call, a stack page has to be allocated to store parameters, return address etc. It is unsafe to invoke the Get Free Page function of the memory manager module before allocating a stack page

***Q2. Why should the OS set the WRITE PERMISSION BIT for library and code pages in each page table entry to 0, denying permission for the process to write to these pages?***
- ExpOS does not expect processess to modify the code page during execution. Hence, during a fork() system call (to be seen in later stages), the code pages are shared between several processes. Similarly, the library pages are shared by all processes. If a process is allowed to write into a code/library page, the shared program/library will get modified and will alter the execution behaviour of other processes, which violates the basic virtual address space model offered by the OS.