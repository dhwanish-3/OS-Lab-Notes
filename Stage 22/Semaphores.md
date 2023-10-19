### Semaphores

- Primitives that allow cuncurrent processes to handle *critical section* problem for handling data consistency
- eXpOS provides binary semaphore which can be used for user programs to synchronize the access to shared resources

1. Acquiring semaphore - Semget
1. Releasing semaphore - Semrelease
1. Locking semaphore - SemLock
1. Unlockinging semaphore - SemUnlock

- *When a process forks, the semaphore currently acquired by the process is shared between child & parent*

- a process can lock/unlock a semaphore only after acquiring
- when entering critical section, process can lock the semaphore
- unlocks after exiting critical section => allowing other processes to lock the semaphore
- after the use of semaphore is finished, a process can detach by releasing the semaphore

![Semaphores](https://exposnitc.github.io/expos-docs/assets/img/roadmap/sem.png)

#### Per-Process Resource Table

- A process mantains record of the semaphores/files acquired by it in its **Per-Process resource table**
- Per-process resource table can store at most 8 entries
- each entry contains 2 words
    - Resource Identifier(1 word) - *FILE* (0) or *SEMAPHORE* (1)
    - Index of open file table/semaphore table entry (1 word)

- free entry is indicated by -1 in the Resources Identifier field


#### Semget System Call
- used to acquire a new semaphore
- finds a free entry in the *per-process resource table*
- then creates new entry in the semaphore table by invoking *Acquire Semaphore* function
- the index of the semaphore table entry returned by acquire semaphore is stored in the *per-process resource table*
- finally returns the index of new entry in the *per-process resource table* as **semaphore descriptor**(SEMID)

#### Semrelease System Call
- argument - semaphore descriptor(SEMID)
- used to detach a semaphore from the process
- releases the acquired semaphore & wakes up all processes waiting for the semaphore by invoking *Release Semaphore* function
- also invalidates the per-process resource table entry corresponding to the SEMID given as argument

#### Acquire Semaphore (function number = 6)
- argument - pid of the process
- finds a free entry in the semaphore table
- then sets PROCESS COUNT to 1 in that entry
- returns index of that free entry of semaphore table

#### Release Semaphore (function number = 7)
- argument - semaphore index(SEMID) & PID of the process
- if the semaphore to be released is locked by the current process, then
    - unlocks the semaphore
    - wakes up all processes waiting for the semaphore
- finally decrements PROCESS COUNT in the semaphore table entry corresponding to SEMID
