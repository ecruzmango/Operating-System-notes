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

