### Process Abstraction

At the end of bootstrap, eXpOS loads and creates first process - INIT process

- a process spawns new processes using *fork* system call
    - caller = parent process
    - new process = child process

- process can terminate itself using *exit* system call

- each process has its own (virtual) address space
- 4 regions in the above address space
    1. (shared) library
    2. code
    3. stack
    4. heap
- address space (0 - maximum limit)
- memory mapping (virtual to physical) by paging/segmentation

- **when a new process is created using fork , the child process shares library, code and heap with th e parent**
    => any modifications to memory words in these regions by one process will result in modification of the contents for both processes

- stack region is seperate
- concurrent exection after fork

- stack region stores
    - *static variables*
    - stack frames

- since eXpOS does not explicitly provide an area for for storingg static data => stored in stack

- Dynamic memory allocation is normally done from heap

- variables to be shared between different processes could also be allocated in heap

- library code mapped to library region

- process can load XEXE file from file system into the virtual address space using *exec* system call

- during loading, the original code and stack regions are overlayed by those newly loaded program
- if the original process had shared its heap with its parent process(or any process), OS ensures that other processes do not lose their shared heap data

- XEXE file must have header which specifies how much size must be allocated by the OS for each region when program is loaded for execution

**Q2** Where does ExpL allocate memory for variables of user defined data type?

- *Variables of user defined data type are allocated memory in stack, same as any variable of primitive data type. Every variable in ExpL is allocated one word memory in stack. Variable of primitive data type saves actual data in the word that is allocated to it, but variables of user defined data type stores the starting address of the object. Alloc() library function allocates memory for an object in heap and returns the starting address. This return address is stored in the stack for corresponding variable.*