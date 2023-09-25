## Process Manager Module

### 1. Exit Process (function number = 3)

- first function invoked in exec sys call
- argument - PID of a process
- deallocates all the pages of the invoked process
    - by calling **Free Page Table** function

- deallocates the user area page by invoking **Free user Area Page**

- state of the process set to TERMINATED


### 2. Free Page Table (function number = 4)

- argument = PID of a process
- for every valid entry in the page table of the process, the pages are freed by invoking the **Release Page** function present in memory manager module

- since the library pages are shared by all the processes, do not invoke Release Page function for the library pages

- invalidates all the page table entries of the process with given PID


### 3. Free User Area Page (function number = 2)

- argument - PID of a process
- user area page number of the process is obtained from the process table entry corresponding to the PID

- user area page is freed by invoking the **Release Page**(memory manager module)

- ***the user area page holds the kernel stack of the current process. Hence, releasing this page means that the page holding the return address for the call to Free User Area Page function itself has been released***

- ***Nevertheless the return address & the saved context of the calling process will not be lost. Because Release Page function is non-blocking and hence the page will never be allocated to another process before control transfers back to caller