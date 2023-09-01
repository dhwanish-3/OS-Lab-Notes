## Resource Manager Module

- handles acquisition and release of system resources
- A process must invoke the resource manager to acquire or release any system resource
- implements two functions for each resource 
    1. acquire the resource
    2. release the resource by a process

- Before the use of a resource, a process has to first acquire the required resource by invoking resource manager
- A process can acquire a resource if the resource is not already acquired by some other process
- if resource not available, then that process has to be blocked unti resource becomes free.

- A blocked process must wake up when the requested resource is released by the process which had acquired the resource
- For this, **when a process releases a resource, the state of other processes waiting for the resource must be set to READY**

- function number(Acquire Terminal) = 8
- function number(Release Terminal) = 9

- function number is stored in R1 and passed as argument to the module

- PID of currently running process must be passed as an argument through register R2

- Acquire & release Terminal are not directly innvoked from the write sys call
- Write system call invokes a function called **Terminal Write** present in **Device manager module (Module 4)**

- function number(Terminal Write) = 3
- Terminal Write function call
    - R1 -> function number = 3
    - R2 -> PID of current process
    - R3 -> Word to be printed
    - acquires terminal by calling Acquire Terminal
    - prints the word in R3
    - releases the terminal using Release Terminal

- invoker must save the registers in use to th kernel stack od the process invoking the module

- R0 -> used for return value

- Acquire Terminal function
    - waits in a loop
    - repeatedly invokes the scheduler if the terminal is not fre
    - this kind of  waiting loop is called **Busy loop or Busy wait**

#### Need for a Busy Loop or Busy wait
- when a process invokes the scheduler waiting for a resource, the scheduler runs the process again only after the resource becomes free.
- However, the process may find the resource locked again when it tries to acquire the resource when it resumes execution
- Reason
    - When a resource is released, all processes waiting for the resource are woken up by the OS.
    Only the one tha get scheduled first will be able to lock the resource successfully
    - other processes will have to wait for the resource repeatedly.


![Control flow for Write sys call](https://exposnitc.github.io/img/roadmap/write.png)