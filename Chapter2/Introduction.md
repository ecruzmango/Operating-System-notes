## Introduction to Operating Systems

* The `processor` fetches an instruction from memory, decodes it (i.e. it figures out which instruction) and executes is (like at 2 numbers together, access memory, check a condition, jump to a function, etc.). After it is done with that instruction, it moves onto the next one, and so on, until the program finally completes.



* These are the basics of the `Von Neumann` model of computing. This sounds simple until we consider all the other things going into the system that makes it easy to use. 

There is actually a body of software that is actually making it easy to run programs known as the `operating system (OS)`. 
The primary way for the OS to do this through a general technique that we call a    `visualization`. This consist of a physical resource (such as a processor, a disk, etc.) and transforms it into a more general easy to use virtual form of itself. We sometimes refer it as a `virtual machine`.
A typical OS exports a few hundred system calls that are available to applications. 


## 2.1 Virtualizing The CPU 

[!NOTE]
Take a look at figure 2.1

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
As it turns out the system has a very large number of virtual CPUs.
