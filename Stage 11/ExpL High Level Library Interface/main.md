## High Level Library Interface (API)

- ExpL language allows applications to access the OS routines through the library interface
- syntax for the call to the library function in ExpL is:
```
t = exposcall(fun_code, arg1, arg2, arg3);
```

Note:
- according to syntax of exposcall(), it needs 4 arguments
- func_code is necessary for every  exposcall() to recognize the Library function/System call
- the remaining number of arguments varies according to Library interface spec of the corresponding func_code
- even if exposcall() is invoked with more number of arguments than required, they are ignored

Examples:
1. To open a file named example.dat,
```
fd = exposcall("Open", "example.dat");
```
    - return value is file descriptor for file example.dat on success

2. to write the value stored in variable "num" to this file,
```
temp = exposcall("Write", fd, num);  // return value stored in temp
```
    - to write to terminal use -2 as first argument
```
temp = exposcall("Write", fd, num);  // Invalid call```

3. Alloc is a library function
    - it is a dynamic memeory management routine
    - it allocates memory for variables of user defined type in ExpL
ExpL call to allocate memory for variable 'data', which requires 3 words of memory is
```
data = exposcall("Alloc", 3);
```

- **The present library routine alloc allocates 8 words for any variable irrespective of the size mentioned in its alloc exposcall(). So, do not define a user defined type having more than 8 fields. Remember to call library function Intialize using exposcall() once before invoking first alloc in any ExpL program.**