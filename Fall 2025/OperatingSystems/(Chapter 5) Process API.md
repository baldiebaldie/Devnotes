Operating systems
08-27-2025

The fork() system call:
- The fork call creates a brand new process that is a copy of the one which calls it
- the process that calls fork becomes the parent and the copy becomes the child process

Here's how it works:
1. A running program calls fork().
2. The operating system creates a new process. This new process is almost exactly the same as the original. It has the same memory, variables, and instructions.
3. The new process is called the **child** process. The original process is called the **parent** process.
4. Both the parent and the child continue to run at the same time.

After fork is called both processes get a return value. The parent gets the ID number of its new child. The child gets a return value of zero. This is how the program can tell if it's the parent or the child. Because they are two different processes now they can run different parts of the code. This is a strong way to make a single program do multiple things at once. ![](../../images/Screenshot%202025-08-27%20at%202.42.57%20PM.png)