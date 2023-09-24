### Disk Status Table

- stores the status of the disk indicating whether the disk is busy or free
- has a LOAD / STORE bit indicating whether the disk operation is a load or store
- stores page no. & block no. involved in the transfer
- PID of the process that has acquired the disk

- spl constant **DISK_STATUS_TABLE** gives the starting address of the Disk Status table in XSM memory
