## Experimental File System (eXpFS)
- eXpOS assumes that disk is sequence of blocks
- block can store sequence of words
- number of words in a block is h/w dependant
- file abstraction allows app programs to think of each data or executable file stored in disk as continuous stream of data
- details of physical storage hidden from app programs
- external interface through which executable and data files can be loaded into the file system externally - **XFS-Interface**