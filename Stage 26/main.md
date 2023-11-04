## User Management

- init
    - login process (pid = 1)
    - userid = 0

- shell = shell process (pid = 2)

### Interrupt 17 - login
- arguments
    - username
    - unencryted password

- if user found
    - sets state of shell process to CREATED
    - state of login process to (WAIT_PROCESS, PID of shell)
    - invoke scheduler

### Interrupt 12 - Logout
![logout](https://exposnitc.github.io/expos-docs/assets/img/roadmap/logout.png)
- arguments
    - none

- only shell process can invoke logout
- invokes *Kill All* function
- sets state of shell process to TERMINATED
- *starting IP of shell process to first word of user stack of shell*
- login process is set to READY state
- invokes scheduler