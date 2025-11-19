Operating Systems
11-17-2025

### What is RAID

RAID = multiple hard drives working together as one big drive
- This is like a team of workers instead of one person doing all of the work.
- This is useful because of:
	- Speed: Multiple drives can work at the same time = faster
	- Capacity: Combine small drives to make on huge drive
	- Reliability: If one drive breaks, your data is still safe
FAULT model
- RAID uses a fail-stop model
	- Disks are either working (can read/write everything) or failed (completely broken)
	- The RAID controller can immediately detect when a disk fails
	- No partial failures - its all or nothing
---
### Performance calculation
Example disk specs:
- Sequential transfer : 10 MB takes 200 ms to transfer
- Random transfer : 10 KB takes 0.195 ms to transfer
- Seek time : 7 ms
- Rotation delay : 3 ms
- Transfer rate : 50 MB /s
Sequential performance calculation:
- Total time = 7 + 3 + 200 = 210 ms
- Speed = 19 MB / 210 ms = 47.62 MB/s
Random performance calculation
- Total time = 7 + 3 + 0.195 = 10.195 ms
- Speed = 10 KB / 10.195 ms = 0.981MB/s
key insight: Sequential is about 48 times faster than random access

![](../../Pasted%20image%2020251119141304.png)

![](../../Pasted%20image%2020251119141247.png)
- ---
### Raid Levels

RAID 0 (striping)
- What it does
	- Splits your data across all of your drives
- For example: 
	- Save a photo - One piece (A) goes to drive A and the other piece (B) goes to drive B
- Pro: Very fast
- Con: If ANY drive breaks, you lose everything
- Best for: Speed when you do not care about back up
 ![](../../Pasted%20image%2020251119141350.png)
RAID 1 (Mirroring)
- What it does: Makes exact copies on different tracks
- For example: Save a photo - Identical copy goes to drive A and drive B
- Pro: If one drive dies, you have a copy
- Con: Uses twice as much space
- Best for: Important data that needs a backup
- The capacity size is n/2 for sequential write
![](../../Pasted%20image%2020251119141415.png)
RAID 4 (Parity)
- What it does: Uses XOR to create backup info on one special drive
- For Example: Drives 1, 2, 3 have your data. Drive 4 has "Parity" that can rebuild any lost drive
- The issue is that the parity drive gets overworked
- Capacity: If you have 5 drives, you get 4 drives worth of space
- It can survive exactly 1 disk failure
- The issue is parity disks become a bottleneck for writes

**Write process (subtractive parity):**
1. Read old data block
2. Read old parity block
3. Calculate: New Parity = Old Data ⊕ New Data ⊕ Old Parity
4. Write new data and new parity

![](../../Pasted%20image%2020251119141912.png)
RAID 5 (Rotating Parity)
- What it does: Like RAID 4, but spreads the parity across all drives
- For example: Row 1 parity on drive 5, row 2 parity on drive 4, etc.
- Pro: No single drive gets overworked
- Best for: Good balance speed, space, and safety

---
### Raid Hardware
A Raid system is basically a small computer. It contains:
- CPU/Micro-controller: The brain that decides where data goes
- RAM: For temporarily holding data as it moves around
- Firmware: Software that runs the RAID operations

Memory Types in RAID
  Volatile Memory (Regular RAM)
  - Purpose: Buffer/Cache for data being read and written
  - For example: When you save a file, it sits in RAM while the RAID figures out which disk to write to 
  - The issue is if the power goes out, anything in the RAM disappears
Non-Volatile Memory (Battery-Backed RAM)
- Purpose: Safely store write operations even if the power fails
- How it works: Has a small battery that keeps the memory alive during power outages
- Why it is needed: Prevents the "oops", I was writing to 3 disks but only have 2 finished" issue
---
### Key ideas

Striping
- like dealing cards - give one card to each player in turn
- Block 0 -> Drive 1, Block 1 -> Drive 2, Block 2 -> Drive 3, then repeat

Chunk size
- How much data goes to each drive before moving to the next
- Small chunks = more drives working together but more seeking
- Big chunks = less seeking but fewer drives working together
---
### Performance Comparison
- **RAID 0**: Fastest everything, but zero protection
- **RAID 1**: Fast reads, slower writes, best protection
- **RAID 5**: Good balance, but slow on small writes
---
### ![](../../Pasted%20image%2020251119142029.png)

---
### Real-World Analogy

Think of RAID like different ways to organize a group project:
- **RAID 0**: Everyone works on different parts super fast, but if anyone quits, the whole project fails
- **RAID 1**: Two people do identical work, so if one gets sick, you're still covered
- **RAID 5**: Everyone does different parts, plus they keep notes so if someone leaves, the group can figure out their part

---

### Key RAID takeaways (exam help)

**Choose RAID based on needs:**
- **RAID 0:** Best performance and capacity, but no protection
- **RAID 1:** Best for reliability and random reads, but expensive storage
- **RAID 5:** Best balance of capacity and reliability, but poor small write performance

**Remember the math:**
- RAID 0: Everything scales with N
- RAID 1: Cut capacity and write performance in half
- RAID 4/5: Lose one disk worth of capacity, parity math creates overhead

**Small write problem:** RAID 4/5 require 4 operations (2 reads + 2 writes) for every small write, making them slow for random write workloads.

**Chunk size matters:** Smaller chunks = more parallelism but more seeking. Most systems use large chunks (64 KB).