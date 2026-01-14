
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
You can get the inode number by doing `ls -i` or the 'stat' command
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

---

### Open file table
A system wide table that tracks every currently open file
Contains:
- Current offset
- File access permissions
- Reference count
- Pointer to the inode

### Inode Table
This table contains all of the inodes (file metadata) for the filesystem

The all the system wide tables connect is

`File descriptor -> Open file table entry -> Inode table entry -> actual file data`


---

### Reading and Writing: The offset system
Every file has a current offset
- Current offset - your position within the file (like a bookmark that moves whenever you read or write)
Sequential reading example
Starting with a 300-byte file:
1. `open("file", O_RDONLY)` → Returns file descriptor 3, offset = 0
2. `read(fd, buffer, 100)` → Reads 100 bytes, offset becomes 100
3. `read(fd, buffer, 100)` → Reads next 100 bytes, offset becomes 200
4. `read(fd, buffer, 100)` → Reads final 100 bytes, offset becomes 300
5. `read(fd, buffer, 100)` → Returns 0 (end of file), offset stays 300

### Non-Sequential Access
You can jump around in files using `lseek()`:
- `SEEK_SET`: Jump to a specific position
- `SEEK_CUR`: Move relative to current position
- `SEEK_END`: Move relative to end of file

```c 
lseek(fd, 200, SEEK_SET); // Jump to byte 200 read(fd, buffer, 50); // Read 50 bytes starting at position 200 // Now offset is at 250
```


#### Writing data immediately
If you want to write data immediately use `fsync(file descriptor)`
- Usually on write it adds to a buffer but for important things such as a database writing instantly is required

---

### Renaming files atomically

The rename() system call is **atomic**
It does this by:
1. Opening tmp file will new contant
2. copy everything over immediately using fsync
3. closing
4. renaming the tmp file 

---

### Getting file information
The stat() and fstat() system calls fill in a structure with file metadata

using stat() gives the user this metadata:
- device which has the file
- Inode number
- File permissions
- number of hard links
- user id of owner
- group id of owner
- file size in B
- last access time
- last modification time
- last status change time

---

### Directory operations
Creating directories
- mkdir() creates a new directory that starts empty except for 2 special entries
	-  "."(dot) - refers to the directory itself (current dir)
	- ".." (dot-dot) - refers to parent directory (parent of current dir)

#### Reading directory contents
Reading directories requires special functions:
```c
DIR *dp = opendir(".");              // Open current directory
struct dirent *d;
while ((d = readdir(dp)) != NULL) {  // Read each entry
    printf("%d %s\n", d->d_ino, d->d_name);  // Print inode and name
}
closedir(dp);                        // Close directory
```

The dirent structure contains:
- d_name - the filename
- d_ino - the inode number
- d_type - type of file (regular file, dir, etc.)

---

#### Removing directories
rmdir() - Removes directories, but only if they're completely empty

---
### Hard links: Multiple Names for one file
A hard link creates additional name for an existing file. Both names refer to the same inode number

---
### File System Mounting

**Making a file system**: The `mkfs` tool creates an empty file system on a storage device. It sets up the basic structure including a root directory and inode tables.

**Mounting**: The `mount` command attaches a file system to your directory tree. After mounting, you can access the files on that device through normal paths.

Example from slides:
bash

```bash
mount -t ext3 /dev/sda1 /home/users
```

This takes the ext3 file system on device /dev/sda1 and makes it accessible at /home/users. Now /home/users/ refers to the root of that mounted file system.

---
### Putting It All Together

This entire system creates a unified way to access files regardless of where they're physically stored. Whether files are on your main hard drive, a USB stick, or a network drive, they all appear as part of one big directory tree. The file system handles the complexity of tracking where everything is stored, managing permissions, and ensuring data integrity.

The layered approach (file descriptors → file table → inodes → actual data) provides flexibility and efficiency while keeping the interface simple for programmers and users.
