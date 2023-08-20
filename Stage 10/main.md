## Console Output

### Modifications to user program
- write system call of eXpOS is coded inside INT 7 handler
- create and delete sys calls are coded inside INT 4 handler
- system call runs is kernel mode => access to all h/w resources
- write sys call (sys call no 5) programmed inside INT 7 handler

**A user program must execute the following steps to invoke the system call:**
1. Save the registers in use in user stack
2. Push the system call number and arguments to the stack

    a. Write sys call no = 5

    b. Argument 1 is the file descriptor (-2 for terminal)

    c. Argument 2 is the word to be written to the terminal

    d. Push R0 (by convention all sys calls have 3 arguments but not this one => this argument will be ignored by the sys call handler)

3. Push any register, say R0 to allocate space for the return value
4. Invoke the interrupt by "INT 7" instruction
5. Pop out the return value, the system call no and arguments which were pushed on the stack prior to the system call
6. Restore the register context from the stack

![before](https://exposnitc.github.io/img/system_call_stack1.png)
![after](https://exposnitc.github.io/img/system_call_stack2.png)

- MODE FLAG field in the process table is used to indicate whether the current process is executing in a sys call, exception handler or user mode
- MODE flag (word 9 in system status table)