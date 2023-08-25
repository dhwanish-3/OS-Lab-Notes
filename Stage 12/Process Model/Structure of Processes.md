### The Structure of Processes
- eXpOS associates a virtual memory address space for each process
- contiguous memory address, starting from zero
- max limit of address space = 5120 (10 pages)
- address space partitioned into 4 regions

    - Shared Library
    - Heap
    - Text/Code
    - Stack

- these regions are mapped into physical memory using h/w mechanisms like **paging / segmentation**
- every process corresponds to a XEXE file stored in XFS file system
- XEXE header of the file contains info about space allocation for each region
- **OS loader**
    - reades the header & sets up the regions of virtual address space
    - maps virual address space into physical memory
- eXpOS loader is the interrupt service routine corresponding to **Exec** system call

- process may open files or semaphores
- OS asociates a file handle with each openn instance of a file
- OS assigns a semaphore identifier (semid) for each semaphore acquired by the process
- the above 2 are also attributes of the process
- in addition process has **context**
- context refers to
    - contents of the registers
    - instruction pointer
    - contents of memory

- a process can get its id using **Getpid** system call
- pid of the parent process get by **Getppid** system call