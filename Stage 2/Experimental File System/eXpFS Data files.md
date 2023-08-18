### eXpFS Data files
- data file is a sequence of words
- extension ".dat"
- eXpFS treats every file other than root and executable files as data file
- __create__ system call auto sets file type in root entry as data
- allows an application program to perform,
1. create
2. delete
3. open
4. close
5. read
6. write
7. seek

- data files can be loaded into eXpFS file system using XFS-Interface
- for such a data file the owner field is set to root(value = 1) access permission is set to open access(value = 1)