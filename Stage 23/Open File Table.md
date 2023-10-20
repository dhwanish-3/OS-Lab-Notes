### Open File Table

- stores info about all the files that are open while the OS is running
- MAX_OPENFILE_NUM = 32 no. of entries
- Open system call creates an entry in the Open file table, when a process opens a file

- *if the file is opened again by some other process(or same), the open sys call creates a new entry in the open file table*
- each entry has 4 words size

- purpose
    1) To keep track of how many times each file has been opened using the​ open ​system call
    2) To provide a mechanism for processes to ​lock a file before making updates to the file’s data/metadata

| INODE INDEX | OPEN INSTANCE COUNT | LSEEK | Unused |

- **Inode index** (1 word) - specifies index of entry for the file in inode table. Special constant *INODE_ROOT* is stored in the case of root file
- **Open instance count** (1 word) - specifies the no. of processes sharing the open instance of the file represented by the open file table entry
- **Lseek** (1 word) - position from which the next word is read from or written into the file
- **Unused** (1 word)

- invalid => -1 (for all)

- Unused entry is indicated by inode index = -1

- *seek pointer is shared between all processes sharing the open instance*
- **OPEN_FILE_TABLE** is set to beginning address of open file table in memory

![files](https://exposnitc.github.io/expos-docs/assets/img/tutorials/file_system.png)