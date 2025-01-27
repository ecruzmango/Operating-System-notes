# Chapter 6: Direct Process

### Facing Challenges:

The basic idea of `virtualization` is simple: run one process for a little while, then run another one and so forth. By time sharing the CPU in this manner,
virtualization is achieved.

However, there are some challenges that we must face. The first is __PERFORMANCE__ and the the second is maintaining __CONTROL__ over the system. 

 `Control` is particularly important to the OS, as it is in charge of
resources; without control, a process could simply run forever and take
over the machine, or access information that it should not be allowed to
access.

## 6.1 Basic Technique: Limited Direct execution

`limited direct execution` is what developers came up. The solution is simple, "__run the
program directly on the CPU__. Thus, when the OS wishes to start a program running, it creates a process entry for it in a process list, allocates
some memory for it, loads the program code into memory (from disk), locates its entry point (i.e., the main() routine or something similar), jumps to it, and starts running the user's code" [Pg.1-2 Arpaci]

__Simplified__:
| OS              | Program |
|-----------------|---------|
| Allocate Memory for program |
| load program into memory
| set up stack with argc/argv
| execute call main() 
||Run main()
|| Execute __return__ from main
| free memory of process and remove from process list

This rises a few issues. How can the OS make sure the program doesn't do anything that we don't want to do? Will it overwrite data that another process owns? How do we stop the process so we can time share the cpu?

## 6.2 Restricted Operations

* we need hardware support for a __"user mode"__ that is restricted (certain instructions that aren't allowed to be executed)
* there will also be a unrestricted mode known as the __"kernel mode"__ or __"privileged mode"__
* now that the process wont be able to break the rules, how do we allow it to read/write to the disk?

>[!NOTE]
>
> In other words,
> "THE CRUX: HOW TO PERFORM RESTRICTED OPERATIONS
A process must be able to perform I/O and some other restricted operations, but without giving the process complete control over the system.
How can the OS and hardware work together to do so" [pg.2 Arpaci]

### system calls
