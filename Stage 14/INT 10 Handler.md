### Modifications to INT 10 handler

- The ExpL compiler sets every uset program to execute the INT 10 instruction(exit sys call) at the end to terminate gracefully.

- INT 10 should halt the process which invoked it, and schedule other surviving processes

- sets dying processes state as TERMINATED

- if all processes except idle are in TERMINATED state, then INT 10 routine can halt the system