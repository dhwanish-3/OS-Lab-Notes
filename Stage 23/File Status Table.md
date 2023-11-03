### File (Inode) Status Table

- contains details about lock status & file open count of each file in the inode table
- MAX_FILE_NUM = 60 no. of entries
- for each file, *index of its entry in inode table must match with index in file status table*
- each entry has 4 words size

| LOCKING PID | FILE OPEN COUNT | Unused |

- **LOCK** (1 word) - if the file is locked by a process inside a system call, this field specifies the PID of the process or -1
- **FILE OPEN COUNT** (1 word) - specifies the no. of open instances of the file or -1
- **Unused** (2 word)


- *every time a file is opened by any process using open system call, the file open count field in corresponding file status table entry is incremented* => table gives global count of no. of open open instances of the file

- when a process enters file system call & tries to access a file, first have to lock access to the file => no other process is allowed to execute any file system call on the file's data/metadata

- **Acquire Inode & Release Inode** are functions designed to handle file access regulation(locking)

- **FILE_STATUS_TABLE** - starting address of the file status table in memory