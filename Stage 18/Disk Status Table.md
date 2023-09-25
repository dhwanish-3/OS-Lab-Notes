### Disk Status Table

- stores the status of the disk indicating whether the disk is busy or free
- has a LOAD / STORE bit indicating whether the disk operation is a load or store
- stores page no. & block no. involved in the transfer
- PID of the process that has acquired the disk

- Disk status table is present at page 56 of the memory

- spl constant **DISK_STATUS_TABLE** gives the starting address of the Disk Status table in XSM memory


| STATUS | LOAD/STORE BIT | PAGE NUMBER | BLOCK NUMBER | PID | Unused |
|--------|----------------|-------------|--------------|-----|--------|

- **STATUS** (1 word) - specifies whether disk is free(0) or busy(1)

- **LOAD/STORE BIT** (1 word) - specifies whether the operation done on the device is **load(0)** or **store(1)**

- **PAGE NUMBER** (1 word) - page no. involved in the disk transfer

- **BLOCK NUMBER** (1 word) - disk block number in the disk transfer

- **PID** (1 word) - PID of the process involved in the disk transfer
- if the disk transfer was initiated by OS during paging/swapping, the field is set to PID of IDLE process (0)
- **Unused** (2 words)
