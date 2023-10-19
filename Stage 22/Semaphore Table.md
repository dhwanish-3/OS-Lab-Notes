### Semaphore Table

- eXpOS maintains a **Semaphore table** which contains the list of semaphores currently acquired by all the processes
- Semaphore table has 32(MAX_SEM_COUNT) entries
- for every semaphore entry in the per-process resource table, there is an entry in the semaphore table

- *Semid* is the index of the semaphore in the semaphore table return by *Semget*

- *Semget* - sets the process count to 1
- When forked - child inherits the semaphore table entry of the parent => process count incremented by 1

- *Semrelease* - decrements the process count
- Process count also decremented when a process terminates

- *SemLock* - if locking pid is -1, then the process locks the semaphore & sets the locking pid to its pid

- Each entry occupies 4 words & last 2 are unused
- The entries are
    1. PROCESS COUNT(1 word) - number of processes currently sharing the semaphore
    2. LOCKING PID(1 word) - pid of the process that has locked the semaphore else -1
    3. UNUSED(2 words)

- Unused entries are marked by setting the process count to 0