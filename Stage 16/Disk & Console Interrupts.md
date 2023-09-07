### Disk and Console Interrupts

- load(page_num, block_num)
- store(disk_num, page_num)

- actual data transfer involves time delays as disk access is slow

- On load/Store instruction, XSM machine will
    - start a disk transfer
    - increment IP by 2
    - fetch next instruction without waiting for data transfer to be completed
    - when disk-memory data transfer is completed, disk controller will raise the disk interrupt

- Similarly **IN** instruction
    - initaites console input
    - will not suspend machine execution till some input is read
    - machine execution proceeds to next instruction
    - when the user enters data it is transferred to port P0
    - console interrupt is raised

- After execution of each instruction in unprivileged mode, the machine checks whether a pending disk/console/timer interrupt. If so
    1. Push the IP value into top of the stack
    2. Set IP to value stored in interrupt vector table entry for timer interrupt handler
        - this entry is located at physical address 493 in page 0(ROM) of XSM 
        - value is 2048 in this location
        - machine switches mode (address translation off)
        - next instruction will be fetched from physical address 2048

- **The XSM machine enables interrupts only when the machine is executing in unprivileged mode**
- If a previously initiated load/store/IN operation is completed while XSM is running in privileged mode, the machine waits for next transition to unprivileged mode before processing the interrupt