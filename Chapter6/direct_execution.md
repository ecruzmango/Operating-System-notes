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

