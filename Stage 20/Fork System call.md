## Fork System Call

- system call number = 8
- interrupt routine number = 8 (int8)
- no arguments

- spawns new process - child
- new pid(child) returned to parent
- parent and child will share code and heap regions
- 2 new stack pages & a new user area page

- process table of child is initialized with the same values of parent except for values
    - *TICK, PID, PPID, USER AREA PAGE NUMBER, KERNEL STACK POINTER, INPUT BUFFER, MODE FLAG, PTBR & PTLR*

- contents of stack of parent are copied into the new stack pages (child)

- contents of per process resource table in user area page of parent is copied to child

- kernel stack of parent is not copied (set to empty in child i.e KPTR = 0)

#### After completion of fork
- child process will be ready(in CREATED state)
- when child is scheduled to run, it starts execution from immediate instruction from call to fork
- fork returns 0 to child

- ExpL compiler allocates these in the stack
    1. local variables
    2. global variables
    3. arrays of primitive datatypes(int, string)

- **Since the parent and child processes have different memory pages for the user stack, they resume after fork with seperate private copies of these variables**

![Fork](https://exposnitc.github.io/img/roadmap/fork.png)

### Alloc System call

- allocates memory from heap region of a process
- Alloc() to store objects referenced by *variables of user defined types* in ExpL program in heap

- Alloc() in parent before fork()=> the copies of the variables in both the parent and the child store the address of the same shared memory

- **Since the Parent and child processes can concurrently access/modify the heap pages, they need support from the OS to synchronize access to the shared heap memory**

- above synchronization through system calls for *semaphores* and *signal handling*

- synchronization not required for code  & library pages as their access is read only

- **the OS needs to ensure during Fork that the parent is allocated its heap pages and these pages are shared with the child**

### Implementation
- *Get Pcb Entry* from Process manager - return pid
- if heap pages are not allocated (for parent) - allocate *Get Free Page* in memory manager
- initialize process table -copy &  modify
- intitialize per process resource table - copy
- disk map table - copy for code and heap & -1 for stack, user area page
- initialize page table - copy code, heap, library
- userstack - copy full
- store BP in kernel stack of child
- set return values

**Q3.** Upon completion of the fork system call, the parent and the child will contain the same return IP address on the top of the user stack. The value of the user stack pointer (UPTR) will also be the same for both the processes. When the fork sytem call returns to user mode (using the IRET instruction) which process executes first - parent or child? why?

- *Fork system call returns to the parent process. IRET sets the value of IP register to the return address at the top of the stack, pointed to by the SP register. The machine translates the logical SP to physical SP using the page table pointed to by the PTBR register, which points to the page table of the parent. Subsequent instruction fetch cycles continue to proceed by translating the value of IP using the PTBR value, which points to the page table of the parent process. The parent process continues execution till a context switch occurs.*