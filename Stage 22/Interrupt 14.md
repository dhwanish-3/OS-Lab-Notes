### Interrupt 14

#### SemLock System Call
- system call number = 19
- arguments - semaphore descriptor(SEMID)
- a process locks the semaphore it is sharing using semlock
- if the semaphore is already locked by another process
    - then the process is blocked by changing state to tuple(WAIT_SEMAPHORE, SEMID)
    - invokes scheduler
    - when the semaphore is unlocked, then the process state is made READY(by other process which unlocked the semaphore)

- when the current process is scheduled & semaphore is still unlocked the current process locks by changing LOCKING PID in semaphore table entry to the PID of current process
- When the process is scheduled but finds that the semaphore is locked by some other process, current process again waits in the busy loop until the requested semaphore is unlocked

#### SemUnlock System Call
- system call number = 20
- arguments - semaphore descriptor(SEMID)
- invalidates the LOCKING PID field (= -1) in the semaphore table entry for the semapphore
- wakes up all processes waiting for the semaphore by changing their state to READY