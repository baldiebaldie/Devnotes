Computer Architecture
11-05-2025

### What is Memory Hierarchy?
Memory hierarchy is a system with different levels of memory. Each level has different speeds, sizes, and costs. The goal is to make memory feel both fast and large to the user.

---
### Principle of Locality
Programs don't access all of their memory randomly. They follow two patterns

**Temporal Locality**: If you use something recently, you'll probably see it again soon
- Good for loops
Spatial Locality: If you use something, you'll probably use nearby things again soon
- Good for reading something like an array

---
### Memory hierarchy levels
From fastest to slowest
1. CPU Registers - fastest, smallest, but most expensive
2. Cache (SRAM) - Very fast, small, but expensive
3. Main Memory (DRAM) - Medium speed, medium size, medium cost
4. Storage (Disk/SSD) - Slow, large, cheap

Hit - data found in current memory level
Miss - data not found, must go grab from slower level
Block - The unit of data that gets moved between levels
Miss Penalty - Time it takes to get data from slower level