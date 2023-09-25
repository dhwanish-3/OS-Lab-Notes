### Per-Process Resource Table

- store the information about the files and semaphores which a process is currently using
- For each process, this table is stored in user area page of the process

- table ahs 8 entries with 2 words each
- total 16 words

- last 16 words of the User Area page to store this table

- In exec after reacquiring the user area page for new process, per-process table should be inititalized in this user area page

- each entry in this data structure is initalized to -1