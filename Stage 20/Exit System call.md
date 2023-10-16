### Exit System call

- *system call number = 10* & *int 10*
- arguments - PID
- has to update/clear OS data structures of the terminating process
- detach memory pages allocated from its address space
- page table entries are invalidated

- there may be other processes waiting for this to end
=> Exit system call wakes up those processes

- Exit has to close the files and release the semaphores acquired by the process
- *Per-process table* has to be invalidated

- Set state to *TERMINATED*

- these tasks are done by **Exit process** in process manager module

- finally calls *scheduler*

![Exit](https://exposnitc.github.io/img/roadmap/exit.png)