### Per-Process Disk Map Table

- stores the disk block number corresponding the pages of each process

- has 10 entries for a single process
- when memory pages of a process are swapped out into the disk, the corresponding disk block numbers of those pages are stored in this table

- stores block numbers of the code pages of the process

| Unused | Unused |Heap 1|Heap 2|Code 1|Code 2|Code 3|Code 4|Stack 1|Stack 2|
|--------|--------|------|------|------|------|------|------|-------|-------|

- if a memory page is not stored in disk block the corresponding entry must be set to -1

- Disk Map Table is present in **page 58** of the memory
- spl constant **DISK_MAP_TABLE** points to starting address of the table
- **DISK_MAP_TABLE + PID * 10** gives beginning address of disk map table entry corresponding to the process with given PID