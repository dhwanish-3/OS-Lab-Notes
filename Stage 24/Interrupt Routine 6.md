### Interrupt Rountine 6 (Read System Call)

![Read](https://exposnitc.github.io/expos-docs/assets/img/roadmap/FileRead.png)

- arguments
    - file discriptor
    - address of a word where data should be read

- locks the inode(*Acquire inode*) & at last releases inode(*Release inode*)

- reads th word at the position pointed by *LSEEK* in open file table entry
    - then stores into the address provided as input
    - after reading *LSEEK is incremented by 1*

- to read from data pointed by LSEEK, the disk block has to loaded into memory
- **Buffer cache** can store 4 disk blocks in memory simultaneously
- cache pages are numbered
    - 0 - 71
    - 1 - 72
    - 2 - 73
    - 3 - 74

- caching scheme is
    - for disk block N => cache page N mod 4 will be used

- *Buffered Read* & *Buffered Write* of file manager module => buffer management

- read invokes Buffered Read function to bring the required block into memory buffer
- then read the word present ar position LSEEK

- *Reading from the root file does not require a buffer, as root file is already loaded into memory at boot-time

- memory address provided is logical address => translation required

- locks the resources inorder of
    1. Inode
    2. buffer
    3. disk
- released in reverse order
- to avoid **circular wait**