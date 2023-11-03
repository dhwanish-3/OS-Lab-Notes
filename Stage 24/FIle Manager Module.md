### File Manager Module

#### Open (function number = 3)

- arguments
    - filename

- locates inode index for the file in *inode table*
    - *locks the inode* using *Acquire Inode* function
    - necessary as to make sure no other process tries to delete the file

- creates a new in the open file table
    - returns index of this entry to the caller

- all the fields of open file table are initialized
    - FILE OPEN COUNT = 1
    - OPEN INSTANCE COUNT = 1
    - INODE INDEX = inode index of the file
    - SEEK POSITION = 0

- in case the file is **root** file, INODE INDEX field is initialized to *INODE_ROOT*(0)

- increments FILE OPEN COUNT field in file status table for the file (except for root file)

- FILE OPEN COUNT for root file is irrelevent
    - root file is pre-loaded into memory at boot time & can not be deleted

- lock on the file is released using *Release Inode* function

#### Close (function number = 4)

- arguments
    - open file table index

- decrements share count(OPEN INSTANCE COUNT) in open file table for the file
    - when the share count reaches 0 => open instance of the fie is invalidated by setting INODE INDEX field to -1
    - FILE OPEN COUNT in file status table is decremented by 1

#### Buffered Read (function number = 2)

- arguments
    - disk block number
    - offset value
    - physical memory address

- reads a word at position specified by the offset within the disk block
    - store it into given physical memory
- (disk block number % 4) block is loaded into buffer page

- to use a buffer it has to be locked by *Acquire Buffer*
- invokes *Disk Load* to load disk block to memory buffer page
- then, word in buffer is copied into address given as argument

- the buffer page to which disk block has to loaded may contain some other disk block
    - if the page has been modifiead( **dirty bit** is buffer table is set)
        - it has to be written back ot disk => *Disk Store* is invoked

- after reading, Buffered Read unlocks buffer page by invoking *Release Buffer* 

#### Acquire Buffer (function number = 1)

- arguments
    - buffer number
    - PID

- invoked from Buffered Read & Buffered Write
- locks a buffer by storing the given PID in the LOCKING PID of buffer table entry corresponing to buffer name

- if required buffer is locked by some other process
    - process with given PID is blocked (WAIT_BUFFER, buffer number)


#### Release Buffer (function number = 2)

- arguments
    - buffer page
    - PID of process

- invalidates the LOCKING PID(store -1) in buffer table entry for given buffer number
- also wakes up all the processes waiting for the buffer with given buffer number to READY