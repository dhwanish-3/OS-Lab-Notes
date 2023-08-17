## Setting up the System

![alt text](https://exposnitc.github.io/expos-docs/assets/img/xsm_folders.jpg)

### $HOME/myexpos/expl
This directory contains the ExpL (Experimental Language) compiler required to compile user programs to XSM machine instructions.

### $HOME/myexpos/spl
This directory contains the SPL (System Programmer's Language) Compiler required to compile system programs (i.e. operating system routines) to XSM machine instructions.

### $HOME/myexpos/xfs-interface
This directory contains an interface (XFS Interface or eXperimental File System Interface) through which files from your UNIX machine can be loaded into the File System of XSM. The interface can format the disk, dump the disk data structures, load/remove files, list files, transfer data and executable files between eXpFS filesystem and the host (UNIX) file system and copy specified blocks of the XFS disk to a UNIX file.

### $HOME/myexpos/xsm
This directory contains the machine simulator (XSM or eXperimental String Machine).

### $HOME/myexpos/test
This directory contains the test scripts for eXpOS