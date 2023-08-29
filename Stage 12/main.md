## Introduction to Multiprogrammning

### Idle Program
- loaded for execution during OS bootstrap
- stored at disk blocks 11 and 12
- OS bootstrap loader must load this program to memory pages 69 & 70
- page table and process table for the idle process must be set up by bootstrap loader
- PIP = 0

### Modificaton to OS Startup code
- PID(idle) = 0
- PID(init) = 1
- Process table entry
    - process with ID = 0 in the 16 words starting at memory address PROCESS_TABLE
    - PID = 1 in 16 words starting at address PROCESS_TABLE + 16 & so on
- page table entry
    - PID = 0 in 20 words starting from PAGE_TABLE_BASE
    - PID = 1 will start at address PAGE_TABLE_BASE + 20
- max 16 processes concurrently

- process being currently scheduled is said to in RUNNING state
- bootstrap loader will load INIT first
- Initially INIT will be in RUNNING State
- and IDLE in CREATED state
- CREATED state indicates that the process had never been scheduled for execution previously
- STATE field occupies 2 words
- in case of RUNNING and CREATED states, 2nd word is not required

- when process in user mode active stack = user stack (logical page 8-9 of process)
- when process switches to kernel mode, kernele code save SP value to UPTR and set the SP register to physical address of top of kernel stack
- when it switches back to user mode, kernel stack is always empty
- Hence, SP must be set to value (User Area Page Number * 512 - 1) when kernel mode is entered

### Modification to timer interrupt handler
- A process in CREATED state had never been scheduled earlier => no history to backup
- A process in READY state had been in RUNNING state in the past => will have saved user context to backup