### Close System Call

- close open instance of a file
- open instance always closed at process termination by Exit call

- arguments
    - file descriptor

- invalidates per-process resource table entry by storing Resource identifier as -1
- *Close* function of file manager module used to
    - decrenent OPEN INSTANCE COUNT in open file table
    - update File status table