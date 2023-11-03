### File Creation & Deletion

- *Create* system call creates empty file with given name
- it starts disk data structurees with metadata for the file
- **Inode Table** & **root file** are data structures to maintain permanent record of files


- *Delete* system call deletes the file with given name from inode & root file
- also deletes all the disk blocks occupied by the file

- **MAX_FILE_NUM** = 60 files can be stored in disk
- For each file, inode table entry & root file entry should have same index
- disk data structures have to be loaded from the disk to the memory before accessing them
- Inode table at memory page 59 & 60
- Root file at memory page 62

![Int4](https://exposnitc.github.io/expos-docs/assets/img/roadmap/create_delete.png)

### Create System Call

- System call number = 1
- arguments
    - filename
    - permission(0 or 1)

- only data file can be created
- finds a free entry(-1 in the FILE NAME field) in the inode table to store details of new file
- user-id field in inode table is set to user-id from process table => owner of the file
- username in root file is set from username in **User table** corresponding to the user-id(index)

- during disk formatting XFS-interface creates 2 users
    - root
    - kernel


### Delete System Call

- arguments
    - filename
- can not be deleted if the file is currently opened by any process
- it first acquires the lock on the file by invoking **Acquire Inode** function
- then invalidates the record of file in entries of inode & root file
- blocks allocated to the file are freed
- finally releases the lock on the file by invoking **Release Inode** function

- *if the deleted file is in buffer cache & buffer page is marked dirty, the OS will write-back the buffer page into disk when another disk block need to be brought into the same buffer page*
- but it is unneccessary if file is deleted => *Delete system call must clear the dirty bit in buffer table of all buffered disk blocks of the file*

#### Acquire Inode (function number = 4)
- arguments
    - inode index
    - pid of the process
- to lock the inode, it waits in busy loop by changing state to **(WAIT_FILE, inode index)** until the file becomes free
- then when it is finally free & current process resumes execution, acquire inode checks whether the file is deleted
- then locks the inode by setting LOCKING PID in *FIle status table* field to pid of the process

#### Release Inode (function number = 5)
- arguments
    - inode index
    - pid of the process
- frees the inode(file) by invalidating the LOCKING PID field in *File status table*
- wakes up all the process wating for the file with given inode index by changing state to READY

### Interrupt Routine 15 (ShutDown System call)

- disk update for these data structures are done during system shutdown

![Int15](https://exposnitc.github.io/expos-docs/assets/img/roadmap/Initial_shutdown.png)


#### Disk Store(function number = 1)

- arguments
    - PID of a process
    - page number
    - block number
- to store data into the disk, first needs to lock the disk by invoking **Acquire Disk** function
- after locking the disk, Disk Store updates **Disk Status Table**
- finally it initiates the store operation for given page number & block number & waits in *WAIT_DISK* state until the store operation is complete
- when store is operation is completed, system raises the disk interrupt which makes process READY again