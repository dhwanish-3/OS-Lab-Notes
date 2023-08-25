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

