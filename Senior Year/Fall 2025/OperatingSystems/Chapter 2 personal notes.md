Operating systems

08-27-2025

Von Neumann model

- Computers are made up of...

    - CPU (Arithmetic logic (ALU) and the Control unit (CU))

    - Memory

    - Input Output (I/O)

Virtualization

- When an operating system takes a physical resource and transforms it into a more general and powerful to use version of itself. It can take most physical parts of hardware and do this. Note the new smaller virtualized part is private. Each program then in turn, thinks it has its own private CPU and private memory. This allows for many programs to run at the same time without each other getting in its way.

    - Example questions: When running multiple CPU processes on a single-core system, what is the illusion that they run simultaneously is achieved through?

        - CPU virtualization via time-sharing

    Another way to think about this is imagine a baker with one stove trying to make 10 different cakes at the same time. The baker will multitask switching between say mixing batter, and putting on frosting. Even though the baker is stopping one task and starting another quickly to "make" all 10 at the same time the baker is not actually doing all of this work simultaneously.

- Concurrency is about making sure processes in an operating system work when running at the same time. An example can be when multiple processes in the same thread attempt to compute and push a number the same chunk of memory at the same time. If this happens you can get an unexpected value due to multiple processes getting different results with the same chunk of memory. This is due in part to steps to change the number can be interrupted. Therefore, the OS must find ways to prevent mistakes thus, concurrency.

    - Example question: **Which of the following is the primary reason why the shared counter in threads script does not always reach the expected value with high loop counts?** Race conditions due to non-atomic increment operations.

        - Another way to think about this is two people trying to empty a bucket of water. One person scoops and the other scoops. If they both start at the same time one might get to the empty bucket leaving the other with an empty scoop. This is the same when threads race to access a shared variable where the memory is already in use or full.

    - Another question is: **The concept of multiprogramming was introduced mainly to Increase CPU utilization by overlapping computation with I/O operations.

  

        - You can think of this as if a chef is waiting for a pot of water to boil (a slow I/O operation), they don't just stand there. They use that time to chop vegetables or wash dishes. Multiprogramming is the same idea. The computer doesn't sit idle when a program is waiting for something slow; it runs a different program.

Persistence

- How computers remember things. Saving data in such a way where it remains even after the computer is shut off. The file system is a large part of this. That is the area that the OS uses to organize and store files on the hard drive. Note the hard drive is not virtualized. Each process understands it shares a piece of the same pie to try and prevent potential issues.

    - Question: **Which of the following is the main role of the file system in an operating system?** Manage files on persistent storage reliably and efficiently.

  

        - Think of the file system as a librarian for your computer. The librarian organizes books (your files) on shelves (your disk) and makes sure you can find them later.

    - Another question:**Which of the following best explains why file systems may delay writes to disk?** To batch multiple small writes into fewer, larger, more efficient operations.

  

        - You can think of this as instead of sending a single text message every time you think of something, you wait and send one big message with all your thoughts. This is more efficient. The file system does the same thing with saving data to a disk.

System Calls and Resource Management

- A system call is a special request a program can make to the OS. It can ask it to do something important like open a file or create a new process. When a system call is made, the keys are handed over to the OS which now has full control. The CPU then switches to a "***Kernel Mode***" through a procedure called a ***"Trap instruction"***. The OS is the boss and a resource manager who gets to use whatever and for however long. As a resource manager the OS controls CPU amounts, memory, and the input output of a process. There is also a thing called a procedure call which is roughly the same as a system call barring the OS getting control of the process. Some examples of system calls is open(), close(), kill(), fork(), etc.

    - Example problem: **Which of the following statements best describes the relationship among OS, system calls, and device drivers?** The operating system exposes functionality through system calls, which may rely on device drivers to interact with hardware.

  

        - Think of it as how a restaurant kitchen is like the OS. The menu is like the list of system calls. When a customer orders a pizza from the menu (a system call), the chef (the OS) might use a pizza oven (a hardware device) that has its own special instructions (the device driver).

Other core concepts through example questions:

- **Which of the following best distinguishes an operating system from a file system?** The operating system manages hardware and resources, while the file system specifically manages persistent data storage.

    - Think of the operating system as a country's government. It manages everything: the army (CPU), the roads (networks), and the power grid (memory). The file system is a specific government agency, like the Department of Records, that only deals with saving and organizing documents.

- **Which of the following is not a main theme of an operating systems course, according to the introduction?** Network packet routing.

    - While networking is a part of computers, a basic operating systems course usually focuses on the core tasks of managing the computer itself, not how data travels over the internet.