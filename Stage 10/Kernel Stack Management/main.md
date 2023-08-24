## Kernel Stack Management during System Calls
- program must pass parameters to the system call through user stack
- return value is passed backed to program throught user stack
- when writing code in ExpL, it will generate code for saving and restoring the context and pushing arguments into user stack
- if writing in assembly then our code must explicitly contain code to do these
- system call module must also change the stack to its kernel stack upon entry and switch back to the user stack at time of return

**Note:**
- alogrithms in design page says each system calll sets **MODE flag** to appropriate sys call no and switches to kernel stack
- interrupt handler can reset the MODE flag and switch back to user stack after return from sys call function, before returning to user mode

<h3>1. Actions done by the user process before executing an INT instruction </h3>
a. Push the registers in use to stack
b. Push the system call no into the stack
c. Push the arguments
d. Push an empty space for return value
e. Invoke the INT instruction corresponding to the system call

![stack](https://exposnitc.github.io/img/Stack_Management/Kernel_sw1.png)
```
.... 			         // Code to push registers
PUSH System_Call_Number	 // Push system call number
PUSH Argument_1		     // Push the arguments to the stack
PUSH Argument_2
PUSH Argument_3
PUSH R0			         // Push an empty space for return value
INT number		         // Invoke the corresponding INT 
			                instruction
```

<h3>2. Actions done by the System call serivice routine upon entry</h3>
a. Extract the sys call no and the arguments from the user stack
b. Set the MODE field in process table entry of the process to the sys cal no
c. Switch from the user stack to the kernel stack
d. Identify the system call using the system call no and transfer control to the system call code

![img](https://exposnitc.github.io/img/Stack_Management/Kernel_sw2.png)
```
 Pseudo code

... 
syscallNumber <- address_translate(UserSP - 5);
Argument_1    <- address_translate(UserSP - 4);
Argument_2    <- address_translate(UserSP - 3);
Argument_3    <- address_translate(UserSP - 2);

// Switching the stack

UPTR <- UserSP
SP   <- User Area Page Number * 512 - 1
...
```

<h3>Actions done by the System call service routine before returning from the system call</h3>

a. Store the return value in the user stack
b. Set the stack pointer (SP) to top of the user stack
c. Set the MODE field in process table entry of the current process to 0
d. Return to the user program using IRET machine instruction

![return](https://exposnitc.github.io/img/Stack_Management/Kernel_sw1.png)
```
Pseudo code

....		   

// store the return value in the space alloted in the user stack

Address_RetVal <- address_translate(UPTR - 1);
		
[Address_RetVal] <- return value   // move the value of User Stack Pointer TO SP
		
MOV SP, UPTR    // return to the user program

IRET
```

<h3>4. Actions done by the process after returning from a system call</h3>
a. Save the return value
b. Pop off the arguments and the system call no from the stack
c. Restore the register context and resume execution

```
 Pseudo code

....
POP R0			    // Pop and save the return value
POP Argument_3		
POP Argument_2
POP Argument_1		// Pop and discard the arguments
POP System_Call_Number	// Pop and discard the system call number 
....			        // Code to restore the register context and 
                           resume execution

```