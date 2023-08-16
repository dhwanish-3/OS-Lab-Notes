## Handling Kernel Stack

When OS enters an interrupt handler that runs in kernel mode , the interrupt handler must switch to different stack to prevent user level __hacks__ through stack

- OS kernel must contain 2 stack
1. user stack
2. kernel stack

- eXpOS requires to maintain a **Process Table** where data such as value of the kernel stack pointer, user stack pointer etc. pertaining to each process is stored.

#### Process Table
- Process table starts at page number 56(address 28672)
- has space for 16 entries (each 16 words)
- user stack(word 13)
- user area page number (word 11)

#### Modification to the OS Startup code
- First available free page is 80
- User Area page is allocated at physical page number 80
- SPL constant PROCESS_TABLE = 28672
- kernel maintains a data structure called **System Status Table** where the PID of curren tuser process is stored
- System Status table is started from 29560
- SPL constant SYSTEM_STATUS_TABLE = 29560
- all interrupt handlers assume kernel stack is empty
