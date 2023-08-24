## Dynamic Memory Routines

### Initialize
- Arguments: None
- Return Value:
    - 0 - Success
    - 1 - Failure
- Initializes heap data structure
- sets up heap area of the process
- Applications responsilbility to invoke Initialize() before the first use of Alloc()
- behaviour of Alloc() and Free() when invoked without an Initailize() operation is undefined
- any memory allocated before an Initialize() operation will be for future allocation

## Alloc
- Arguments: Size(Integer)
- Return Value:
    - Allocated address in Heap - Success
    - -1 - No allocation
- takes size as input
- currently, it allocates 8 words for any variable

## Free
- Arguments: Pointer(integer)
- Return value:
    - 0 - Success
    - -1 - Failure
- takes a pointer (an integer memory address) of a previously allocated memory block and returns it to the heap memory pool
- if invalid refernce to memory allocated => undefined behaviour