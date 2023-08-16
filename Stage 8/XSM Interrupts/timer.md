### Timer Interrupt
XSM timer device which can be set to interrupt the machine at regular intervals of time.
```sh
xsm --timer 10
```
Then, every 10 instructions completed in unprevileged mode,
1. Push the IP value to top of the stack
2. Set IP to value store in the interrupt vector table for timer interrupt handler. This entry is at physical address 493 in page 0 (ROM) of XSM and value 2048 is present at this location
=> IP register gets value 2048
=> Machine switches to previleged mode and address translation is disabled.
=> next instruction fetched from physical address 2048

**Interrupts are disabled when machine runs in the privileged mode so that there are no race conditions.**

In a time-sharing environment, **the timer handler invokes the scheduler of the OS to do a context switch** to a different process when one process has completed its time quantum.