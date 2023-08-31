### Operations on Processes
- Most fundamenal operations are
    - Fork
    - Exec
- remaining operations are
    - Exit
    - Wait
    - Signal
    - Getpid
    - Getppid
#### Semantics of Exec operation:
1. OS closes all files and semaphores opened by the process.  A new address space is created replacing the existing one.  The new process inherits the process id of the calling process.

2. The code (and static data ) of the exec file are loaded into the code (and stack) regions of the new address space.  The system library is mapped to the library region and stack is initialized to empty.

3. machine IP is set to location specified in XEXE header.  The machine SP is initialized to beginning of the stack. 
- From here execution continues with the newly loaded program.

#### Semantics of the Fork operation:
1. new child process with new pid and address space is created which is an exact replica of the original process with same library, code, stack and heap regions.
- fork call returns the pid of the child to parent
- the heap, code and library regions of the parent are shard by the child
    => modification of contents of these regions by one process during subsequent execution will change the other as well
    - both process are in concurrent execution subsequent to the fork operation
    - Stack is seperate for the child and is not shared

2. All open file handles and semaphores are shared by the parent and child
    - file handles (or semid) of files (or semaphores) opened or created after (subsequent) to fork operation by parent or child will be exclusive to particular process and not shared
- Parent and child continue execution from here on

#### Exit sys call
- terminates a process after closing all files and semaphores

#### Wait sys call
- suspends the execution of a process till another process exits or executes a **Signal** sys call

#### Signal sys call
- resumes the execution of a process that was suspended by wait