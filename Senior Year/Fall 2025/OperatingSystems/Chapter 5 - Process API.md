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

  

After fork is called both processes get a return value. The parent gets the ID number of its new child. The child gets a return value of zero. This is how the program can tell if it's the parent or the child. Because they are two different processes now they can run different parts of the code. This is a strong way to make a single program do multiple things at once. ![](../../../-images/Screenshot%202025-08-27%20at%202.42.57%20PM.png)

Why do we need to create a child process?

- to perform more than one function at the exact same time.

What is wait()?

- It can be useful for a parent process to wait for the child process to finish and return a value

- wait() system call wont return until the child has fully run and exited.

  

- the parent process may issue a wait() call which stops the execution of the parent operation

- essentially it is a way to ensure that the child process finished what it is doing first before finishing the parents execution

What is exec()?

- Exec is short for execute.

- The exec() family of functions replace the current process image with a new process image. It first loads the program into the current process space and runs it from the entry point. If successful, exec() will **NOT** return anything.

### UNIX shell basics

- When you press enter it passes your line of text to the shell which is then interpreted by the shell. With a command to execute one program the shell forks a child program to run the executable using exec(). Then it goes to sleep awaiting future commands. When the executable is done, it tells the kernel its done and exits. The kernel then wakes the shell back up.
$$