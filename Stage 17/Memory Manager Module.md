## Memory Manager Module

### 1. Release Page (function number = 2)

- argument - page number to be released

- first decrements the value in memory free list corresponding to the page number given

- the **System status table** keeps track of number of free memory pages available to use in the **MEM_FREE_COUNT** field

- when the memory free list entry of the page becomes 0, no process is currently using the page
    - in this case., increment **MEM_FREE_COUNT** in **System status table
    - indicates that page has become free for fresh allocation

- Finally this function must check whether there are processes in WAIT_MEM state(processes wating for memory to become available)
    - If so these processes have to be set to READY state as memory has become available


### 2. Get Free Page (function number = 1)

- no arguments except the function number
- returns the page number of the first free page in R0

- Fundamentally, Get free page searches through the memory free list to find a free page for the allocation
- if a free page is found, memory free list entry corresponding to that page is incremented and number of page found is returned

- if no memory page is free (i.e. MEM_FREE_COUNT in System status table = 0) then,
    - state of the process is changed to WAIT_MEM
    - scheduler is invoked
    - this process is scheduled again when the memory is available (state of this process is set to READY by some other process)
    - the field **WAIT_MEM_COUNT** in the System status table stores the number of processes waiting to acquire a memory page
    - this function increments the WAIT_MEM_COUNT before changing state to WAIT_MEM
    - the process waits in WAIT_MEM state until any memory page is free
    - upon waking up this function allocates the free memory page and updates the WAIT_MEM_COUNT and MEM_FREE_COUNT in the System status table