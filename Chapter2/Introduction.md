## Introduction to Operating Systems

* The `processor` fetches an instruction from memory, decodes it (i.e. it figures out which instruction) and executes is (like at 2 numbers together, access memory, check a condition, jump to a function, etc.). After it is done with that instruction, it moves onto the next one, and so on, until the program finally completes.



* These are the basics of the `Von Neumann` model of computing. This sounds simple until we consider all the other things going into the system that makes it easy to use. 

There is actually a body of software that is actually making it easy to run programs known as the `operating system (OS)`. 
The primary way for the OS to do this through a general technique that we call a    `visualization`. This consist of a physical resource (such as a processor, a disk, etc.) and transforms it into a more general easy to use virtual form of itself. We sometimes refer it as a `virtual machine`.
A typical OS exports a few hundred system calls that are available to applications. 


## 2.1 Virtualizing The CPU 

>[!NOTE]
>Take a look at figure 2.1

```C
#include <stdio.h>
#include <stdlib.h>
#include <sys/time.h>
#include <asset.h>
#include "common.h"

int main(int argc, int argv[]){

    if (argc != 2){
        fprintf(stderr,"usage: cpu <sting>\n");
        exit(1);
    }
    char *str = argv[1];
    while(1){
        Spin(1);
        printf("%s\n", str);
    }
    return 0;
}
```

The program does not do much as it only calls the function <ins>Spin()</ins>, which repeatedly checks the time and returns once it has run for a second. Afterwards, it prints out the string.

__EXAMPLE__
```
    prompt> gcc -o cpu cpu.c -Wall
    prompt> ./cpu "A"
    A
    A
    A
```
This time let's run many different instances of this same program. The results will be a bit more complex:

__EXAMPLE__
```
    prompt> gcc -o cpu cpu.c -Wall
    prompt> ./cpu A & ./cpu B & ./cpu C & ./cpu D &
    [1] 7353
    [2] 7354
    [3] 7355
    [4] 7356
    A
    B
    D
    C
    B
    D
    C
    A
```

Even though we only have one processor, we are able to have all 4 programs running at the same time. How is that possible?
As it turns out the system has a very large number of virtual CPUs. Turning a single CPU (or a small set of them) into a seemingly infinite number of CPUs and thus allowing many programs to seemingly run at once is what we call virtualizing the CPU.

As the book states, the ability to run multiple programs at once reaches all sorts of new questions. __For Example__,  if two programs want to run at a particular time, which should run? This question is answered by a policy of the OS.

## 2.2 Virtualizing Memory (simplified)

Basic Memory Model:

Memory is fundamentally an array of bytes
*    To read: you need an address
*    To write: you need both an address and data
*   Both program instructions and data are stored in memory


The Example Program:

It allocates memory using malloc()
Stores a value at memory address 0x200000
Increments this value in a loop
Each running instance is identified by a unique Process ID (PID)


The Key Observation:

When multiple instances of the same program run simultaneously
Each instance shows it's using the same memory address (0x200000)
Yet they don't interfere with each other
Each instance updates its "own" memory independently


The Explanation - Virtual Memory:

This is possible because the OS provides memory virtualization
Each process gets its own "virtual address space"
The address 0x200000 in each process maps to different physical memory locations
Programs think they have their own private memory
The OS manages the translation between virtual and physical addresses behind the scenes


Why This Matters:

Programs can be written as if they have the entire memory to themselves
They don't need to worry about interfering with other programs
The OS handles the complexity of sharing physical memory
This is a key example of how operating systems provide abstraction and isolation



This is a fundamental concept in operating systems - the virtualization of resources (in this case, memory) to provide each process with the illusion of having its own private memory space, while efficiently managing the actual physical memory underneath

## 2.3 Concurrency

[!NOTE]
Take a look at the program below:

```c



```
