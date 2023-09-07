## Console Interrupt

- IN (must use to read data from console to input port P0)

- IN is a privileged mode instruction
- executed only inside a sys call/module

- read sys call invokes **Terminal Read** function in **Device Manager Module**(module 4)
- IN instruction will be executed within this function

- IN will not wait for the data to arrive in P0
    -> need for mechanism to detect arrival of data to P0
    -> when string/number is entered
    -> ENTER is pressed

- then XSM machine will raise the console interrupt

- Since it is not useful for the process that invoked the Terminal Read function to continue execution till data arrives in P0, a **process executing the IN instruction will sets its state to WAIT_TERMINAL and invoke the scheduler**

- It is the **responsiblity of the console interrupt handler to transfer the data arrived in port P0 to the process which is waiting for the data**

- done by copying the value present in port P0 into the **input buffer** field of the process table entry of the process which has requested for the input

- **Console interrupt handler also wakes up the processes in WAIT_TERMINAL by setting its state to READY**

- Each process maintains an input buffer which stores the last data read by the console

- On the occurance of a terminal interrupt
    - the interrupt handler, uses PID field of terminal status table to identify the correct process that had acquired the terminal for a read operation
    - the handler transfer data from the input port to the input buffer of the process

- Arguments for Read system call
    1. file descriptor = -1 (Terminal input)
    2. variable to store number/string

- Read sys call - **Interrupt 6** invokes **Terminal Read** in Device Manager Module

- Terminal read function must perform
    1. Acquire terminal
    2. Issue an IN instruction
    3. Set process state as WAIT_TERMINAL
    4. Invoke scheduler
    5. After console interrupt wakes up this process transfer data present in input buffer of process table into word address

![Control flow of Read](http://exposnitc.github.io/img/roadmap/read.png)
