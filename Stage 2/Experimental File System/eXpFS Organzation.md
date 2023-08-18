### eXpFS File System Organization
- eXpFS logical file system comprises of files organised ina single directory called root
- root is also treated conceptually as a file
- each file has 3 attributes each 1 word long
1. name
2. size
3. type
- each file must have unique name
- size of a file is the total number of words in the file
- in extended eXpOS file has 2 more attributes
1. username
2. permission

- 3 types of files
1. root file
2. data file
3. executable file

- each file in eXpFS has an entry in the root called **root entry**