### Modifications to Timer Interrupt Routine

ISR - Interrupt Service Routine

- scheduler first saves the values of registers SP, PTBR and PTLR to the process table entry of current process

- next process to run is determined round robin algorithm

- if the state of new process is READY, then scheduler changes the state to RUNNING then returns

- although the scheduler was called by one process (timer ISR), since the stack was changed inside the scheduler, the **return is to a program instruction in some other process! (determined by the value on top of the kernel stack of the newly scheduled process - IP to the instruction immediately after call scheduler**)
- an exception to this rule is when the newly selected process to scheduled is in CREATED state
    - Here, the process was never run and hence there is no return address in the kernel stack

- The round robin scheduling algorithm generally schedules the "next process" in the process table that is in CREATED/READY state

- timer will restore the user context of the new process from the stack and return to user mode, results in new process being executed