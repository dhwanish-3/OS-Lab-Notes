### Interrupt 16

#### 1. Newusr system call = 22
- arguments
    - username
    - unencrypted password

- only invoked from shell of root

#### 2. Remusr system call = 23
- arguments
    - username
- only invoked from shell of root

- *user can not be removed if the user is owner of any file*

#### 3. Setpwd system call = 24
- arguments
    - username
    - unencrypted password

- only invoked from shell
- only changes password of current user
- root can change password of any user

#### 4. Getuname system call = 25
- arguments
    - userid
- returns username of the user with given userid
- can be invoked from any process of any user

#### 5. Getuid system call = 26
- arguments
    - username
- returns userid of the user with given username
- can be invoked from any process of any user