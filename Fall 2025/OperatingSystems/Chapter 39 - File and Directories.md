
Operating Systems
11-19-2025

### What is persistent storage?
- Persistent storage keeps data safe even when the power goes out:
i.e.
- Hard disk drives
- Solid state drives (SSDs)
Two main abstractions
- Files - Just containers for data
- Directories - containers for organizing files
---
### What is a file?
A file is a linear array of persistent bytes that can be:
- Created
- Read
- Written
- Deleted
Key concepts:
- Every file has a secret low-level name called the **inode** number
- Users never see this number - they use human names like "hw.txt"
- The file system manages both the human name and inode numbers**Example from slides:**

**Example from slides**
```
ls -lt foo
442383 -rw-rw-r-- 1 vboxuser vboxuser 6 Jul 2 22:29 foo

stat foo
Inode: 442383    (This is the secret low-level name!)
```

---
### inodes - the File's secret identity
What inodes do
- They store metadata (info about the file)
- Each file gets a unique inode number (index)
- The inode number points to a table that contains:
	- File size
	- Location on disk
	- Permissions (who can read/write)
	- Timestamps
**Think of it like**
- inode number = student ID number
- inode table = student database with all the details
- human filename = My actual name
---
### Directories
A directory is like a file but it contains a list:
- Pairs of (human name, inode number)
- Each entry points to either a file or another directory
i.e: If a directory has entry ("foo", "10"), this means:
- There's something called foo
- It has inode number 10
#### Directory trees (file system hierarchy)

```
		(root)
		   | 
         /  \
       foo   bar
       |     / | \
    bar.txt bar foo
              |
           bar.txt
```
Two types of paths:
Absolute paths (which start with /)
- i.e
	- /foo/bar.txt
	- /bar/foo/bar.txt
Relative paths (starts from current location)
- boo/foo/bar.txt (considering you are in the root)
---
### File descriptors - How programs access files
The procces
1. Program calls `open("filename")`
2. OS returns a file descriptor (just a number)
3. Program uses this number to read/write the file
4. Program calls close() when done

Key system calls:

``` c
int fd = open("foo.txt", flags);     // Returns file descriptor number
read(fd, buffer, size);              // Read data using the ticket
write(fd, buffer, size);             // Write data using the ticket  
close(fd);                           // Return the ticket
```

Special file descriptors
- 0 = standard input (keyboard)
- 1 = standard output (screen)
- 2 = standard error (error message)

NOTE: File descriptors are process to process (They are stored in the PCB)

--- 
### File descriptors tables - How the OS manages Open Files
Every process has its own file descriptor table:
- It is like a personal phonebook
- Each entry point to information about an open file
**Three level of tables**
1. Pre-process file descriptor table
	- Your personal list of open files
	- Maps numbers like 3, 4, 5 to file information
2. System-wide open file table
	- Shared by all processes
	- Contains current position in file, permissions
	- multiple processes can share entries
3. inode table
	- Contains file metadata
	- shared by all processes
Example from slides:
```c
int fd1 = open("file.txt");    // returns 3
read(fd1, buf, 12);            // reads 12 bytes, offset now 12

int fd2 = open("file.txt");    // returns 4  
// fd1 and fd2 have separate offsets!
```

---
### fork() and Shared File descriptors
Special case: When a process forks, parent and child share the same open file entries.
Example:

```c
int fd = open("file.txt", O_RDONLY);
int rc = fork();
if (rc == 0) {
    // Child process
    lseek(fd, 10, SEEK_SET);    // Child moves to position 10
} else {
    // Parent process  
    wait(NULL);
    // Parent sees offset is also 10!
}
```
Result: Child and parent share the same offset because they share the open file table entry.

---
### dup() System call - copying file descriptors
dup() creates a copy of a file descriptor:
- Both file descriptors point to the same open file table entry 
- They share the same offset
Example:
``` c
int fd1 = open("file.txt");    // returns 3, offset = 0
int fd2 = open("file.txt");    // returns 4, offset = 0 (separate entry)
int fd3 = dup(fd2);            // returns 5, shares with fd2

read(fd2, buf, 5);             // reads bytes 0-4, both fd2 and fd3 now at offset 5
read(fd3, buf, 3);             // reads bytes 5-7, both fd2 and fd3 now at offset 8
read(fd1, buf, 2);             // reads bytes 0-1, fd1 independent at offset 2
```