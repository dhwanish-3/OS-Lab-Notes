## Multi-user Implementation

- max 16 users
- 2 default users: kernel(0) & root(1)

- user-table
    - username
    - encrypted password
    - 32 words of disk block 4 in disk at then end of inodetable

- **ENCRYPT** - to convert text password to encrrypted form (in spl *encrypt*)

- *kernel is non-loginable user* => no password set by **FDSIK**

- root
    - password = root

- OS startup hand creates
    - 3 processs
    - idle, init, shell (0, 1, 2)
    - init => login process

- userid of idle & login process = 0 (*kernel processes*)
- userid of shell = id of user who has logged in
