## XSM Interrupts and Exception Handling

XSM Machine supports 3 hardware interrupts
1. Timer interrupt
2. Disk interrupt
3. Console interrupt

- Each of these have handle routines with names = name + handler
- ROM locaions are 493, 494 and 495
- Preset to values 2048, 3072 and 4096
- OS bootstrap code must load timer interrupt handler into memory from 2048
- and disk interrupt handler from 3072
- and console interrupt handler from 4096
