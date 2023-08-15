## ABI and XEXE Format

#### Modifying INIT
- to comply with XEXE format
- first 8 words of the must contain header
- OS i sexpected to load file into logical pages starting from page 4
- first disk bloack of program is loaded into logical address from 2048, second (if 512 words not enough) to 2560 & so on

- first instruction in program loaded to 2056
- second instruction at 2058 & so on
- jump address in the INIT program must be designed accordingly

### Modifying OS Startup Code
```
loadi(63, 13);
loadi(64, 14);
```
- The eXpOS ABI stipulates that the code for shared library must be loaded to disk blocks 13 and 14
- During OS startup OS loads this code into memory pages 63 and 64
- **This library code must be attached to logical page 0 and logical page 1 of each process**
- this code is shared by every application program running on OS called **common shared library** or simply library.
- **The dynamic memory allocation functions of the library manage the heap memory of the application program. The ABI stipulates that each application must be provided 2 pages of memory for the heap. These two pages must be attached to logical pages 2 and 3 of the application.**
