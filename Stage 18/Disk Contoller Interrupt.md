### Disk Controller Interrupt

- allows imitating asynchrounous disk access by the  OS
- whenever the machine executes the LOAD / STORE instruction the Disk controller initiates a counter that ticks with each unproviliged instruction executed after it
- when counter reaches threshold value (specified during boot time (--disk option in XSM simulator))
    - counter is reset
    - **disk interrupt (INT 2)** is raised

- At this point machine executes
    1. Increment value of SP & push IP into the memory address pointed by SP
    2. Since machine in unprivileged mode, address in SP is treated as logical address & address translation is done
    3. Set IP to value stored in **interrupt vector table** entry for the disk interrupt handler (physical address = 494 at page 0(ROM) **value here = 3072**) i.e. IP = 3072 & machine switches to kernel mode

- The machine will re-execute these steps immediately after return to unprivileged mode, before executing any other instruction in unpriviliged mode
- thus the occurence of disk interrupt results in machine execution to transferred to the disk interrupt handler(INT 2) stored in page 6 - 7

- disk controller does not support parallel input requests
- OS programmers duty to ensure multiple disk requests are not raised
    - after a LOAD / STORE instuction is executed, another LOAD/STORE instruction is executed only after device controller has raised the device interrupt -> marking completion of first request