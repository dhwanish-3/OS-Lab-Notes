## Program Loader

- Exec is the "Program loader" of eXpOS
- first checks if the file is a valid eXpOS executable stored in XSM disk
- if so, Exec
    - destroys the invoking process
    - loads the executable file from disk
    - sets up program for execution as a process
- A successful exec operation results in the termination of the invoking process & hence never returns to it

- file should be present in the disk before starting the machine
- inode index of the file can be obtained by going through the memory copy of the inode table
- inode table is loaded from disk to memory in Boot module

- Exec first deallocates all pages the invoking process is using, includes
    - 2 heap pages 
    - 2 user stack pages
    - code pages
    - user area page
    - also invalidates the entries of the page table of invoking process

- newly schedules process will have the same PID as the invoking process
    => same process table entry & page table of the invoking process will be used

- Exec calls the **Exit Process** function in the process manager module(module 1) to deallocate the pages & to terminate the current process

- Since Exec sys call runs in kernel mode and needs a kernel stack for its own execution, after coming back from Exit process function
    => Exec reclaims the same user area page for the new process


- Further, exec acquires 2 heap, 2 stack & required number of code pages (number of disk blocks in the inode entry of the file in the inode table)

- new pages will be acquired by invoking the **Get Free Page** function present in memory manager module (Module 2)
    - page table is updated according to the new pages acquired

- for loading blocks into the memeory pages, we use immediate load (**loadi**)

- finally exec initializes the IP value of new process on top of its user stack and initiates execution of the newly loaded process in the user mode

- eXpOS maintains a data structure called **memory free list** in page 57 of the memory
- Each page can be shared by 0 or more processes
- there are 128 entries in the memory free list corresponding to each page of memory 
    - for each page the corresponding entry in the list stores number of process sharing the page

- the constand **MEMORY_FREE_LIST** gives the starting address of the memory free list

![Exec](https://exposnitc.github.io/img/roadmap/exec1.png)