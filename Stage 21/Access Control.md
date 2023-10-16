### Access Control

- when multiple processes access the same data (in memory or files), => requires some locking mechanism
- to ensure data integrity & issue called **critical section problem**

- eXpOS Provides (binary) **Semaphore** to allow programs to handle the critical section problem
- binary-valued variable whose values can be set or reset through system calls

- Process can acquire a semaphore using *Semget* system call, which returns *semid* for the semaphore

- any of the processes sharing a semaphore identifier can set the semaphore using *SemLock* system call giving semid as argument 
- *SemUnlock* will reset / release the semaphore waking up all other process which went to sleep trying to lock the semaphore