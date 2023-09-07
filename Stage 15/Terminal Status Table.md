## Terminal Status Table

- keeps track of the Read/Write operations done on the terminal
- PID of the process that invoked the system call for Read/Write is stored
- contains info on status of terminal(free or busy)
- size = 4 words (last 2 unused)
- occupies page 57 of the memory
- SPL constant **TERMINAL STATUS TABLE** points to starting address of the table

- **Status** (1 word)
    - specifies whether terminal free or busy
    - initially set to 0
    - set to 1 when busy
    - **Terminal Interrupt Handler** sets back the status to 0 upon completion of Terminal Read
    - **Write system call** sets back the status to 0 upon completion of Terminal Write

- **PID** (1 word) 
    - specifies the PID of the process which is currently using terminal
    - invalid when STATUS is 0

- **Unused** (2 words)
