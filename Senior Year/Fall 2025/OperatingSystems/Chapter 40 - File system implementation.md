Operating Systems
12-01-2025


### The big picture
2 problems
1) Data structures: How do you organize all the info on the disk? Where do you actually put all of the files? Where do you store information about each file? How o you keep track of which parts of the disk are free?
2)  Access methods: When a program calls open(), read(), or write(), what actually happens behind the scenes? Which parts of the disk does the file system need to read or write to complete that operation?
---

### Dividing the disk into blocks
- First step: Dividing the storage device into fixed- size chunks called blocks.
	- i.e: 4KB blocks where each block starts from 0
- Picture the disk like a big parking lot dividing into many parking spaces which each hold 4KB of data.

### The Overall Layout: Different Regions for Different Jobs
The file system divides the disk into several regions, each with a specific purpose

#### The superblock
Block 0 contains the super block - the master directory of the file system which has crucial info like:
- How many data blocks exist on this disk
- How many inodes are available
- Where the inode table begins
- Other essential parameters
When you mount a USB drive, the OS reads the superblock first to understand how this particular file system is organized

#### The allocation bitmaps
Inode bitmap: A simple list where each bit represents an inode slot. If the bit is 0, the slot is free, if 1 then the slot is occupide

Data bitmap: A simple list where each bit represents one data block. 0 means empty, 1 means full

The inode table:
This region stores the actual inodes
From the slide example:
- Inode table occupies blocks 3-7
- Each inode is 256 bytes
- A 4 KB block can hold 16 inodes (4096 ÷ 256 = 16) 2^12/2^8 = 2^4 
- Total of 80 inodes possible (5 blocks × 16 inodes each)
This means the file system can hold 80 files and directories

The data region
The remaining blocks store the actual file contents and directory information. This is where photos, documents, and program files actually live

### Inodes: The file info cards
Each file has an inode (index node) which contains the metadata

Whats in an inode
For example 2 in the slides
**Ownership and Permissions**:
- `uid`: Which user owns this file
- `gid`: Which group the file belongs to
- `mode`: Who can read, write, or execute the file

**Size and Timing Information**:
- `size`: How many bytes the file contains
- `time`: When the file was last accessed
- `ctime`: When the file was created
- `mtime`: When the file was last modified
**Block Location Information**:
- `blocks`: How many disk blocks this file uses
- `block`: An array of 15 pointers showing where the file's data is stored

**Reference Counting**:
- `links_count`: How many directory entries point to this inode (for hard links)

How can you find an inode on disk? 
In this example we are looking for inode number 32
1. calculate byte offset: 32\*256B = 2^12
2. Add the inode table start address: 12KB + 8KB = 20KB
3. Convert to sector address for the disk: 20KB / 512B per sector = sector 40
This mathematical approach lets the file system instantly jump to any inode without searching.

---
## The Multi-Level Index: Handling Big Files

Small files can store their data block addresses directly in the inode. But what about huge files that need hundreds of blocks?

The solution is **indirect pointers** - like having a table of contents that points to other tables of contents.

### Direct Pointers

The inode contains 12 direct pointers. Each points straight to a data block. For 4 KB blocks, this handles files up to 48 KB.

### Single Indirect Pointer

When a file grows larger, the system allocates an indirect block. This block doesn't contain file data - instead, it contains 1,024 block addresses (since each 4-byte address fits 1,024 times in a 4 KB block).

Maximum file size with single indirect: (12 + 1,024) × 4 KB = 4,144 KB

### Double and Triple Indirect Pointers

**Double indirect**: Points to a block full of single indirect pointers. Each of those points to a block of data block addresses.

**Triple indirect**: Points to a block of double indirect pointers.

This nested system can handle enormous files. With 12 direct + single + double + triple indirect pointers, files can exceed 4 GB in size.

The beauty of this system is that small files use simple direct pointers (fast), while large files automatically use the more complex but scalable indirect system only when needed.

## Directories: Special Files That List Other Files

Directories are actually special files whose "data" consists of lists of filename-to-inode mappings.

### Directory Structure

From the slide example, a directory containing files "foo", "bar", and "foobar" would store:

```
inum | reclen | strlen | name
  5     4       2       .        (current directory)
  2     4       3       ..       (parent directory)  
 12     4       4       foo
 13     4       4       bar
 24     8       7       foobar
```

Each entry contains:

- `inum`: The inode number of this file
- `reclen`: How many bytes this directory entry takes
- `strlen`: Length of the filename
- `name`: The actual filename

### The Directory Tree Structure

Directories form a tree by containing entries that point to other directories. The special entries "." and ".." in every directory create this hierarchical structure:

- "." always points to the directory itself
- ".." always points to the parent directory

## Free Space Management: Keeping Track of Available Space

The file system must efficiently track which blocks are free for new files.

### Creating Files

When you create a new file:

1. The system searches the inode bitmap for a free slot (bit = 0)
2. It marks that slot as used (bit = 1) in the inode bitmap
3. It writes the bitmap back to disk
4. It initializes the new inode with default values

### Allocating Data Blocks

When writing data to a file:

1. The system searches the data bitmap for free blocks
2. It marks those blocks as used in the data bitmap
3. It updates the file's inode to point to the new blocks
4. It writes the actual data to those blocks

### Pre-allocation Strategy

Many file systems use **pre-allocation** - when allocating one block for a file, they try to reserve several consecutive blocks. This improves performance because reading consecutive blocks is much faster than jumping around the disk.

## Access Paths: What Really Happens During File Operations

Let's trace through what actually occurs when you perform common file operations:

### Opening a File: open("/foo/bar", O_RDONLY)

The system must traverse the directory path to find the target file:

**Step 1-2**: Read root directory

- Read inode 2 (the root directory inode)
- Read the root directory's data blocks
- Search for entry "foo", find its inode number

**Step 3-4**: Read foo directory

- Read the foo directory's inode
- Read foo's data blocks
- Search for entry "bar", find its inode number

**Step 5**: Access the target file

- Read bar's inode
- Check permissions
- Allocate a file descriptor
- Return file descriptor to the program

The slide timeline shows this requires 5 disk reads just to open the file. **The amount of I/O is proportional to the path length** - deeper directory structures require more reads.

### Reading from a File

Once a file is open, each `read()` operation:

1. **Read the inode** to find which data block contains the requested bytes
2. **Read the data block** containing the actual file content
3. **Write the inode** back with an updated "last accessed" timestamp
4. Update the in-memory file offset

**Each read operation requires 2 reads + 1 write**, even just to read data that's already on disk.

### Writing to a File

Writing new data to a file is more complex because it might require allocating new blocks:

1. **Read the inode** to check permissions and find existing blocks
2. **Read the data bitmap** to find a free block for the new data
3. **Write the data bitmap** to mark the new block as used
4. **Write the actual data** to the newly allocated block
5. **Write the inode** to record the new block address and update timestamps

**Each write that allocates a new block requires 2 reads + 3 writes** (5 I/O operations total).

### Creating a File

File creation involves even more work:

**Path traversal** (4-6 reads to find the parent directory) **Inode allocation** (1 read + 1 write to the inode bitmap)  
**Directory update** (1 write to add the filename entry) **Inode initialization** (1 read + 1 write to set up the new inode) **Directory inode update** (1 write to update parent directory timestamps)

Creating a single file can easily require 10+ disk operations.

## Caching and Buffering: Making It Fast Enough to Use

All these disk operations would make file systems impossibly slow without clever caching strategies.

### Read Caching

The file system aggressively caches frequently accessed blocks in system memory (RAM). From the slides:

- The first time you open "/foo/bar" might require 5 disk reads
- The second time, most of those blocks are already cached in memory
- Opening the same file again requires zero disk I/O

This is why the second time you open a program, it starts much faster than the first time.

### Write Buffering

Reading can be completely eliminated with caching, but writes must eventually reach the disk for persistence. File systems use **write buffering** to improve write performance:

**Delaying writes**: Instead of writing to disk immediately, the system holds writes in memory for 5-30 seconds

**Batching writes**: Multiple separate write operations can be combined into fewer, more efficient disk operations

**Scheduling writes**: The system can reorder writes to minimize disk head movement

**Avoiding writes**: If a program writes to the same block multiple times quickly, only the final version needs to reach disk

### The Tradeoff

Write buffering creates a tradeoff: better performance versus potential data loss. If the computer crashes, you might lose the last 5-30 seconds of changes. Critical applications can force immediate writes using `fsync()` when they need to guarantee data is safely stored.

### Unified Page Cache

Modern systems use a **unified page cache** that dynamically adjusts how much memory is used for file caching versus other system needs. If you're doing lots of file operations, more memory gets used for file caches. If you're running memory-hungry programs, some file cache memory gets reclaimed.

## Putting It All Together: Why File Systems Are Complex

This detailed view reveals why file systems are among the most complex parts of an operating system:

**Every simple operation triggers multiple disk accesses**. Opening a file requires reading several directories and inodes. Writing data requires updating bitmaps, inodes, and data blocks.

**Performance depends heavily on caching**. Without aggressive caching, even basic operations would be painfully slow.

**Consistency is crucial**. If the system crashes at the wrong moment, the file system could be left in an inconsistent state with inodes pointing to freed blocks or bitmaps that don't match reality.

**Layout decisions have long-term consequences**. Choices like block size, inode size, and the number of direct pointers affect both performance and the maximum file sizes the system can handle.

The multi-layered design (applications → file system interface → block allocation → physical storage) provides a clean abstraction that hides all this complexity from normal programs, while still allowing the file system to efficiently manage the underlying storage hardware.

Retry