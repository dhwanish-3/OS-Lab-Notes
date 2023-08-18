### The eXpFS Root File
- root file has name root
- contains meta-data about files in the file system
- For each file, root file stores 3 words of info
1. file name
2. file size
3. file-type
- this triple is called **root entry** of the file
- first root entry is root itself

- operations on the root files are
1. open
2. close
3. read
4. seek