### Open System Call

- arguments
    - filename
- to read/write a file, we need to open it first
- creates new open instance of the file
- return **file descriptor** - index of the new per-process resource table entry
- a process can open a file multiple times => multiple file descriptors for same file
- creates entry in 
    - **per-process resource table**
    - **open file table**
- when a file is opened, the OPEN INSTANCE COUNT in th open file table is set to 1 & *seek* position is set to start of file (0)
- each time when a file is opend, the FILE OPEN COUNT in the file status table is incremented by 1

- Open system call invokes *Open* function of *file manager module*
    - to deal with file status table & open file table

- when a process *forks* the open instance of files & semaphores are shared
    - OPEN INSTANCE COUNT in open file table incremented

- *FILE OPEN COUNT* (in file status table) - global count of how many times Open system call has been invokes with each file in system
    - number of instances  of a file at given time
    - incremented each time a file is opened
    - decremented each time a file is closed
    - if count is 0, file is deleted from file status table **(doubt)**

- *OPEN INSTANCE COUNT* - each open instance can be shared between multiple processes
    - share count
