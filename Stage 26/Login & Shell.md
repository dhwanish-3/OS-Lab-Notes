## Login & Shell Processes

- first user to login after running FDISK must be the root user with the password "root"

- if username & password match, login process creates a shell program
- for shell program
    - shell code is pre-loaded into memory
    - stack pages are allocated & page table is set during boot time

- *shell process is assigned user-id of the logged in user and its Parent PID is set to the PID of the login process. Hence, the shell process runs the shell in the context of the logged in user*

- login process waits till the *logout* system call is invoked by the shell process
- login runs in infinte loop & never terminates

- shell pid = 2
- logout system call terminates all processes of the current user except the shell process
- logout process can executed only by shell process

### Built in Shell Commands

1. Newusr - to create new user
2. Remusr - to remove a user
3. Setowd - set the password of a user
4. Getuid - to get user-id of currently logged in user
5. Getuname - to get username of currently logged in user
6. Logout - to logout of the current user
7. Shutdown - to shutdown the system