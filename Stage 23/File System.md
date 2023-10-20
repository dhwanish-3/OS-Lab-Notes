## File System

### Disk Data Structures

#### 1. Disk blocks & Disk Free List

- data of a file is stored in disk blocks
- file may have up to 4 blocks of data
- interface where the user feels like the file is stored in sequentially although the actual allocation could be in non-contiguous blocks

- inode table entry for a file store the block number of the disk blocks which contain the file data

- *disk free list* is global data structure that indicates which blocks are allocated and which disk blocks are free
- disk blocks are allocated for a file by *write system call*(uses *get free block* to allocate disk block)
- disk blocks are freed by *delete system call*(uses *release block* to free disk block)

- we have to update disk free list when a disk block is allocated or freed
- the following disk data structures contain *meta-data* corresponding to each file in the system (inode table & root file)

#### 2. Inode Table

- global data structure that contains an entry for each file stored in the file system
- a new entry is created when
    1. new file created using write sys call
    2. file loaded into disk the disk using XFS-interface

- inode entry of a file stores
    1. file name
    2. file size
    3. user-id of owner
    4. file type(data/executable/root)
    5. file access permission
    6. the block numbers of the disk blocks allocated to a file(max 4)

- *when a file is created by the Create system call, no disk blocks are allocated for the file, only inode entry is created* => file size will be set to 0 initially

- file name & access permission are supplied as arguments to Create system call & set acccordingly
- only data files can be created using Create system call => file type is set to DATA
- executable files can only be loaded into the file system using xfs-interface
- user-id of the process executing create system call is set as the own of the file
- eXpOS is a single terminal system
- as data is written into the file by write system call, new disk blocks may be allocated, whenever a block is allocated for a file, the block number is recorded in the inode table

- file can be created with *Exclusice access/open access* permission
- access permission is given as argument to create system call
- if a file is created with exclusive access permisstion, the Delete and Write sys calls must fail if executed by any process whose user-id is not equal to root or owner of the file
- when a file has open access permission, all users are allowed to perform any operation on the file


#### 3. Root File

- stores human readable information about each file in the file system
- eXpOS does not support directory structure all files listed at single level
- each file has an entry in the root file
- *kth entry in the root file corresponds to the file whose inode index is k*
- entry contains
    1. filename
    2. file size
    3. file type
    4. username
    5. access permissions

- part of the data in the inode table is duplicated in the root file
- because the root file is designed to be readable by user programs using read system call 
- inode table is accessable only through OS routines only
- *Write and Delete system calls are not permitted on the root file*

- the only data in the root file that is not present in the inode table is the username of the owner of the file => inode table contains user-id of the owner
- user-id value can be used to index into the user table to find the username corresponding to the user-id

- when a file is created, the Create system call must initialize the root file entries of the file along with the inode table entries
- when the file size is changed by write system call, the root file & inode table entry must be updated

#### 4. User Table

- contains the names of each user who has an account in the system
- not a data structure assocaiated with the file system
- an entry consists of,
    1. usernane
    2. encrypted password

- *user-id* of a user is the index of the user table entry corresponding to the user
- first two entries of the table are
    - 0 - kernel
    - 1 - root


### Transient data structures

#### 1. File(Inode) Status Table

#### 2. Open File Table

#### 3. Per-Process Resource Table

- the Open system call returns index of a entry in the resource table as **file descriptor** to the user
- any Read/Write/Seek/Close system call on open isntance must be given file descriptor as argument

#### 4. Memory buffer Cache

- when a process tries to Read/Write a file, the relevent block of the file is first brought into a disk buffer in memory & the read/write is performed on the copy of the block stored in the buffer

- OS maintains 4 memory buffer pages as cache(0 , 1, 2, 3)
- buffers are in memory pages 71, 72, 73, 74
- buffering scheme
    - when there is a request for ith disk block, (i mod 4) buffer is used
    - if thte buffer is already in use, then the OS must check whether the disk block needs a write-back(dirty) before loading the new block into the buffer

#### 5. Buffer Table

- manages buffer cache
- contains one entry per each buffer page => 4
- each entry contains
    1. block number of the disk block stored in the buffer or -1
    2. dirty bit - whether the buffer was modified after loading
    3. locking PID - PID of process which locked the buffer or -1

- when a process tries to read/write into certain data block of file using sys call, the sys call must first determine the buffer number to which block must be loaded & lock the buffer before loading disk to buffer data transfer
- locking PID is set to prevent other processes from concurrently trying to load other blocks into the same buffer