## Process Synchronization

- using *wait* & *signal* system call
- implement *Getpid* & *Getppid* also

- When a process executes *Wait* system call
    - its execution is suspended till the the process it waits for, executes *signal* or terminates
    - waiting process sets its state to *WAIT_PROCESS*
    - then invokes scheduler

- When a process executes signal sys call it wakes up all process waiting for it
- if a process terminates without calling signal, then *Exit* sys call wakes up all the processes waiting for it

- wait & signal help to achieve process synchronization
- In general, synchronization primitives help two co-operating processes to ensure that one process stops execution at certain program point, and waits for the other to issue a signal, before continuing execution

- suppose process B had finished using the shared resource and had executed Signal system call before process A executed Wait system call, then process A will wait for process B to issue another signal. Hence if process B does not issue another signal, then process A will resume execution only after process B terminates. The issue here is that, although the OS acts on the occurance of a signal immediately, it never records the occurance of the signal for the future.In other words, **Signals are memoryless**

- Exit system call has to be modified to wake up all processes waiting for it
- but one special case : *Exit called by Exec*
- Process waiting for the current process must not be waken up

- when a process exits child => *orphans* & pid set to -1 (not if called from Exec)


### Shell Program

- user program that implements interactive user interface for the OS
- string entered in shell - command
- *Shutdown* command to halt the system
- If program present in disk => forks & child executes => shell waits
- if not present => child prints "BAD COMMAND" & exits
