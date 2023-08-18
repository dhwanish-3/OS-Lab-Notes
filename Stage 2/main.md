## Understanding the Filesystem
- XSM - **eXperimental String Machine**
- XSM consists of a processor, memory and disk
- no software except for a boot ROM
- XSM machine's disk contains 512 blocks, each storing 512 words
- XSM disk is formatted to what is known as eXpFS file system format
- eXpFS format specifies that each data/executable file can span across at most 4 data blocks
- index to these blocks along with anme and size of file must be stored in a pre-define area of disk called **Inode table**
- Inode table at disk blocks 3 and 4
- UNIX file named __disk.xfs__ simulates the hard disk of the XSM Machine

![alt text](https://exposnitc.github.io/img/xfs-interface.png)