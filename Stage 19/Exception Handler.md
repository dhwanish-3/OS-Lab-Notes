## Exception Handler

- 4 events result in generation of exception in XSM
    1. Illegal memory access
    2. Illegal instruction
    3. Arithmetic exception
    4. Page fault

- one of these leads to transfer of of control to exception handler

- occupies page 2 & 3 in memory
- block 15 & 16 in disk

- 4 registers in XSM which stores cause & info of the exception
    - **EC, EIP, EPN, EMA**

- EC - stores cause of exception

For events
    1. Illegal memory access(EC = 2)
    2. Illegal instruction (EC = 1)
    3. Arithmetic exception (EC = 3)
the exception handler *just prints the cause*

- As the OS is not responsible for these 3 errors ex. handler 
    - *halts the process gracefully*
    - *invokes scheduler*

- **Page fault exception** (EC = 0) occurs when the last instruction in the currently running application tried to either
a. Access / modify data from a legal address within its address space, but the page was set invalid in the page table
b. fetch an instruction from a legal address within its address space, whose page table entry is invalid

- in either case error was from side of OS (did not load the page & set the page tables)

In such case
    - *ex. handler resumes the execution of the process after allocating the required pages for the process & attaching the pages to the process
    - if the faulted page is a code page the OS needs to load the page from the disk to newly allocated memory*

- **lazy allocation**
    - start executing a process with just one page of code & 2 pages of stack allocated initially
    - when the process during execution, tries to access a page that was not loaded, an ex. generated & exhandler will allocate the required page
    - if the required page is a code page it will be transferred from disk to allocated memory
    - since page allocation only on demand => memory utilization is better(on avg.)

- In previous stage, we allocated 2 pages for each heap and stack 
- we modify to allocate only allocate pages for stack (nothing for heap)

- *For code blocks, only 1 page is allocated & first code block is loaded into that memory page

- we use **Get Code Page** in memory manager module for doing both allocating memory page & loading one code block


- Each process has a data structure called **Per-process Disk map table**
    - stores disk block numbers corresponding to the memory pages used by the process
    - *each disk map table has 10 words*
        - 1 user area page
        - 2 for heap
        - 4 for code
        - 2 for stack
        - 1 unused
    - helps to keep track of disk copy of memory pages
    - **DISK_MAP_TABLE** gives start address for PID = 0
    - **DISK_MAP_TABLE + PID * 10** for any process


### Get Code Page
- arguments - block number of a single code block
- loads this block into a memory page

- code pages are shared by the process running the same program

- purpose of this function is to find out if the current code block is already in use by some other process
    - done by going through disk map table entries of all the processes checking for the code block
        - if found, then function checks if the code block is loaded into a memory page
        - if code block is already present in some memory page, then just returns that memory page number
        - if not, a new memory page is allocated by invoking *Get Free Page* function  in memory manager module
        - followed by loading code block using *Disk Load* function device mamger module

- return memory page number

### Exception Handling
- first switches to kernel stack & backsup register context
- using *EC* finds out the cause of exception
- if other than page fault
    - print the error message to notify the user about the termination of the process
    - terminate the process by invoking **Exit Process** function in process manager module
    - invoke scheduler

- *eXpOS is designed such that, page fault can only occur for heap and code pages*

- using EPN register, the exhanlder find out whether page fault has caused for heap or code page
- **for heap EPN = 2 or 3** exhanlder allocates 2 new memory pages by invoking *Get Free Page* function in memory manager module

- for code page, exhandler invokes **Get Code Page** in memory manager module & page table is updated

- finally restores register context
- switches to user stack and to user mode

- An OS can implement **Demand Paging**, as we will be doing here, only if the underlying machine hardware supports re-execution of the instruction that caused a page fault.

![Exec](https://exposnitc.github.io/img/roadmap/exec3.png)
