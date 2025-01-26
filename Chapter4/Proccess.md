# Chapter 4: Processes

**Process**: is a running program (described in the intro of Ch.2).  A process is a program that has state attached to it. (i.e. it's a DFA!)

* We usually want to have more processes than we have CPU's, but how do we get this to work?

>[!IMPORTANT]
> HOW TO PROVIDE THE ILLUSION OF MANY CPUs? Although there are only a few physical CPUs available, how can the OS provide the illusion of a nearly endless supply of "CPUs"

* as described in the intro, we can use `virtualization`. 
* The OS creates this illusion by virtualizing the CPU. By running one
process, then stopping it and running another, and so forth, the OS can promote the illusion that many virtual CPUs exist when in fact there is only one physical CPU (or a few). 
* This basic technique, known as `time sharing` of the CPU, it allows users to run as many concurrent processes as they would like; the potential cost is performance, as each will run more
slowly if the CPU(s) must be shared.

### Virtualization in simple terms

Imagine you're the manager of a large library. The library has only one librarian (like having one CPU), but many people want to get books at the same time. Instead of making everyone wait in a single line while the librarian helps one person at a time, you create a clever system: you set up multiple service desks, and the librarian quickly moves between them, spending a small amount of time helping each person in turn. To the library patrons, it appears as if there are multiple librarians working simultaneously, even though there's really just one moving very quickly between desks.
This is similar to how CPU virtualization works in operating systems. The operating system acts like the library manager, creating the illusion of multiple CPUs by rapidly switching the actual CPU's attention between different programs or processes. This technique is called "time-sharing" or "CPU scheduling."
Here's how it works in more detail:
The operating system divides CPU time into very small time slices (typically milliseconds). When a program starts running, it gets assigned a time slice on the CPU. Before that time slice ends, the operating system saves the current state of the program (like bookmarking where the librarian left off with each patron). Then it switches to another program, loads its saved state, and gives it a turn on the CPU.
This switching happens so quickly that to humans and programs, it creates the illusion of having multiple CPUs, each dedicated to running their specific program. Just as library patrons might believe they each have their own librarian, programs "believe" they have their own CPU.
The benefits of this virtualization are significant:
First, it allows multiple programs to run concurrently on a single CPU, making efficient use of the hardware. Without virtualization, we'd need one physical CPU for each program we want to run, which would be incredibly expensive and impractical.
Second, it provides isolation between programs. Each program operates as if it has its own dedicated CPU, memory, and resources, unaware of other programs running simultaneously. This isolation improves security and stability, as problems in one program are less likely to affect others.
Third, it simplifies program development. Programmers can write their applications as if they have exclusive access to the CPU, without worrying about coordinating with other programs. The operating system handles all the complex scheduling and resource management behind the scenes.

## 4.1 The Parts of a Process:

* To understand what counts as a process, we first have to understand its `machine state`

**machine state**: What a program can read or update when it's running a given time

* **MEMORY**: this is one of the components of the machine state. The __instructions lie in memory__. Memory information includes what parts of memory the process can access and what data is stored there. The memory that the process can address is its `address space`

| code | Heap | | Stack | 

* **REGISTERS**: 
Registers are the fastest form of computer memory, built right into the CPU. 
While main memory (RAM) might take dozens or hundreds of CPU cycles to access, registers can be accessed in a single cycle. 
This makes them crucial for performance.


* **I/O Information**:
Processes interact with persistent storage devices

## 4.2 Process API

These APIs, in some form, are available on any modern operating system.

* Create a new process (fork)
* Destroy a process (SIGKILL)
* wait for process to stop running (waitpid())
* Miscellaneous control - suspend/resume (kill(pid, SIGCONT))
* Status - give info about process like running time 

## 4.3 Process Creation
The first thing that the OS must do to run a program is to load its code
and any static data (e.g., initialized variables) into memory, into the address space of the process. Programs initially reside on disk (or, in some
modern systems, flash-based SSDs) in some kind of executable format;
thus, the process of loading a program and static data into memory requires the OS to read those bytes from disk and place them in memory
somewhere
* Early OSes loaded entire program into memory before executing 
* modern OSes does this lazily by loading the code/data only as needed 

Here's what the operating system does to prepare a program for execution:

Memory Loading and Setup:

Loads program code and static data from disk into memory
Allocates memory for the run-time stack (for local variables, function parameters, return addresses)
Initializes stack with main() arguments (argc, argv)
Creates initial heap space for dynamic memory allocation (malloc/free)


I/O Setup:

Sets up file descriptors (in UNIX: standard input, output, error)
Enables program to read input and display output


Program Execution:

After setup is complete, OS jumps to the program's main() function
Control transfers from OS to the new process


Key Concepts:

Stack: Pre-allocated space for program's immediate function-related needs
Heap: Grows dynamically as program requests memory via malloc()
File Descriptors: Enable program I/O operations
Process Initialization: Multi-step process to prepare program for execution

This illustrates how the OS creates the environment necessary for a program to run properly and independently.

## 4.4 Process States

__Running__: executing instructions
__Ready__: Ready to run but currently not running
__Blocked__: waiting for an event to happen 

