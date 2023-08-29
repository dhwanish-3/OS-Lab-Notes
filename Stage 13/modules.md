## Boot Module

- perform certain logical tasks
- run in kernel mode
- invoked from kernel mode
- can be invoked from interrupt rountines, other modules or the OS startup code
- uses the kernel stack of currently scheduled process
- XSM supports 8 modules
    - MOD_0 to MOD_7
    - invoked using CALL MOD_n / CALL <MODULE_NAME>


- while switching to module, the CALL instruction pushes the IP of next instruction after CALL to top of the kernel stack then starts execution

- module returns to called using RET (not IRET) instruction
- RET restores the IP value on top of kernel stack
- IRET changes mode from kernel to user as it assumes that SP contains a logical address
- RET returns to the called in kernel mode, using IP value pointed by SP

- A module may implement several functions
- each function is given a function number to distinguish them within the module
- this function no should be passed as argument in reg R1 along with arguments in R2, R3 etc. 
- Register R0 is reserved for return value

- OS startup code can have only 1 memory page(1)
- but it may exceed one page due to initialization of several OS data structures.
- need to design a module for OS initialization
- called **Boot Module**(module 7)
- invoked from OS startup code
- OS startup code
    - hand-created the idle process
    - initializes Sp register to kernel stack of idle process
    - loads module 7 in memory
    - then invokes boot module (using stack of IDLE process)
    - upon return from boot module, OS startup code initiates user mode execution of idle process

- we initiate idle process first before init from this stage onwards
- Boot module is responsible for initialization of all eXpOS data structures, user processes and also loading interrupt routines and modules