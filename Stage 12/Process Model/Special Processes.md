### Special Processes in eXpOS
- idle and init are special processes
- stored in predefined location the the disk and loaded to memory by bootstrap loader
- main purpose of idle is to run as background process in infinite loop
- demanded by the OS so that the scheduler will always have a process to schedule
- init is the first process executed by OS
- PID(idle) = 0
- PID(init) = 1

- A shell in an ExpL program which takes the name of an executable file as input and executes it
- shell proccess forks itself and the child process involes the Exec sys call with the executable file as argument
- shell runs untill the user stops the process