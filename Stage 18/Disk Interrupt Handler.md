## Disk Interrupt Handler

- When the **loadi** statement (immediate load) is used fot loading, the machine will execute the next instruction only after block transfer is complete by the disk controller

- **load** statement in SPL translates to **LOAD** instruction in XSM

- LOAD takes 2 arguments
    - page number
    - block number

- XSM machine does not wait for the block transfer to complete
- instead XSM provides hardware mechanism to detect the completion of data transfer
- it raises disk interrupt when operation is complete

---
- In real OS, it maintains a software module called the disk device driver module for handling disk access
    - responsible for programming the disk controller h/w for handling disk operations

- in this implementation **device manager module** integrates a common "driver software" for all devices of XSM

- the **loadi** instruction abstracts disk I/O using the method of **polling**
- **load** instruction abstracts **interrupt based** disk I/O
---

### Disk tranfer using load statement

- first process has to acquire the disk

- OS maintains data structure called **Disk Status table** to keep track of these disk-memory transfers

- After acquiring the disk, it initializes the disk status table 

- the process then issues the load statement to initiate the loading of the disk block to the memory page
- XSM machine does not wait for the transfer to complete

- However, virtually in any situation in eXpOS, the process has to wait till the data transfer is complete before proceeding
- Hence the process suspends its execution by changing its state by **WAIT_DISK** & invokes the scheduler, allowing other concurrent processes to run


- When the load/store transfer is complete, XSM machine raises the h/w interrupt called the **disk interrupt**
- XSM machine stops the execution of the currently running process
- the disk interrupt handler releases the disk by changing the STATUS field in Disk Status table to 0
- then wakes up all the processes waiting for the disk(by changing STATE from WAIT_DISK to READY) (this also includes the process whichis waiting for the disk transfer to complete)
- then returns to the process which was interrupted by disk controller

---
XSM machine disables interrupts when executing in kernel mode => disk interrupt onnly in user mode. Hence OS has to schedule some process even if all processes are waiting for disk/terminal interrupt.
The IDLE process is precisely designed to take care of this and other similar situations
---

![Exec](https://exposnitc.github.io/img/roadmap/exec2.png)


### 1. Disk Load (Device Manager Module, function number = 2)
- arguments
    - PID of a process
    - page number
    - block number

- Acquires disk by calling Acquire disk function (resource manager module) (module 0)
- Set disk status table entries
- Issue load statement to initiate DMA transfer
- set the state of the process to WAIT_DISK
- invoke the scheduler


###  2. Acquired Disk (Resource Manager Module, function number = 3)
- argument - PID of a process

- while the disk is busy set the state of the process to WAIT_DISK & invoke the scheduler
- when the disk is finally free the process is woken up by the disk interrupt handler
- lock the disk by setting the status & PID fields in the Disk status table to 1 & PID of the process respectively


**Q1 Can we use the load statement in the boot module code instead of the loadi statement? Why?**
- No. The modules needed for the execution of load, need to be present in the memory first. And even if they are present, at the time of execution of the boot module, no process or data structures are initialized (like Disk Status Table).

**Q2. Why does the disk interrupt handler has to backup the register context?**
- Disk interrupt handler is a hardware interrupt. When disk interrupt occurs, the XSM machine just pushes IP+2 value on stack and transfers control to disk interrupt. Occurance of a hardware interrupt is unexpected. When the disk interrupt is raised, the process will not have control over it so the process (curently running) cannot backup the registers. That's why interrupt handler must back up the context of the process (currently running) before modifying the machine registers. The interrupt handler also needs to restore the context before returning to user mode.

**Q3. Why doesn't system calls backup the register context?**
- The process currently running is in full control over calling the interrupt (software interrupt) corresponding to a system call. This allows a process to back up the registers used till that point (not all registers). Note that instead of process, the software interrupt can also back up the registers. But, the software interrupt will not know how many registers are used by the process so it has to back up all the registers. Backing up the registers by a process saves space and time.

**Q4. Does the XSM terminal input provide polling based input?**
- Yes, readi statement provided in SPL gives polling based terminal I/O. But readi statement only works in debug mode. Write operation is always asynchronous.