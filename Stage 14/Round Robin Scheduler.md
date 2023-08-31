## Round Robin Scheduler

- using the --exec option of XFS interface, we can load executable files into XFS disk
- XFS interface will load the executable into disk
- and create inode table entry for file
- also creates a root entry
- From inode table entry we can find out disk blocks where the file is loaded by XFS interface

- scheduler is implemented as MODULE_5
- loaded to disk blocks 63 and 64 in XFS disk
- boot module must load this from disk to memory pages 50 & 51

- in the current implementation (in this stage)
    - first 3 process table entries are occupied
    - initialize STATE field of all other process table entries to TERMINATED
    - useful while finding next process to schedule with round robin algorithm
    - when STATE field is TERMINATED, this means process table entry is free for allocation to new processes