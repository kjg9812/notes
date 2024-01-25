# 1/23/24
### What is an OS?
- manage hardware with software
- applications (gui) built on top of OS
### Why do we need an OS?
- only piece of software that accesses the hardware
- it needs to be efficient
- if media player, jdlsakf,etc all access hardware it would be chaos

#### applications developed by programmers
- media player, communication, games, word procesing
- does a programmer need to understand all the hardware to write these programs?
    - no this would be overkill
    - so the OS gives you a nice interface to the hardware

#### ex. opening a file
- tell operating system you want to ask the disk
    - OS makes sure you have access etc then lets you access the disk/file

### two main tasks of OS
- provide programmers and programs a clean abstract set of resources
    - abstract commands so that managing hardware is more simple, ex. fclose, fopen, fwrite...
- manage the hardware resources

#### how does the OS perform these two main tasks?
1. virtualization
- put an abstract layer on top of hardware -> make the hardware look different than what it actually is
- because hardware is really hard to look at
- we want to virtualize the CPU and the memory mainly
- every program must think that it has the whole CPU and whole memory to itself
2. concurrency
- parallel programs
3. persistence
- the filesytem on the disk
- make sure the data manages through faults
- becasue the OS does not virtualize the disk

# 1/25/24
### The 3 ways to achieve the main tasks
virtualization of the cpu and memory
- put a layer between the program and the real thing, which makes the program see the real thing differently
- why dont we make a virtualization of the disk
    - if programs are both trying to read the same file its fine, but writing at the same time becomes the problem
    - if theres an outage, there may not be enough time to write to the actual disk if youre constantly trying to update the disk with your virtualized layer
    - so the persistence of the filesystem is the problem
concurrency and parallel programs
persistence of the filesystem on the disk
### Main Goals of an OS
- convenience: set of standard libraries (APIs)
    - as a programmer, if you need to access hardware, you need to tell OS to do things -> which means you need to call APIs
- abstraction
- performance
- energy efficient
    - if it is not, people won't use the OS
- isolation and protection
    - isolate programs from each other
    - media browser cant know its coexisting and running at the same time as other programs
    - if one of the programs is broken, it wont affect the others
- reliability
    - system that does not crash
- ability to evolve
    - drivers are clips to OS to make OS to deal with a new device
#### Terms
- Core = CPU
- if it is single core it can only execute 1 program at a time
### OS Topics
- multiple types
    - ex. supercomputer OS, server OS, PC OS, etc.
- concepts
    -  processes, threads, memory management, file system, IO
- different structures
    - coding strategies for OS
    - monolithic, layered systems, microkernels, virtual machines, etc

### flow
- you are the user
    - you run programs (processes)
        - which activates OS
            - which talks to hardware
        - which sends back to OS
    - which sends back to programs
- which is displayed back to you

#### Process
Definition: a process is a program in execution
- each process thinks
    - it has the whole CPU for itself and the whole memory for itself
    - this is what we call virtualization
- the code to be executed is a thread
    - if we have one sequential program, there is one thread
    - if there are programs running in parallel, there are multiple threads
- process is associated with an address space
- also associated with a set of resources
- process is a black box, a container
    - it holds all information needed to run a program

#### Source code to execution
- GCC stands for GNU Compiler Collection, which refers to compiler assembler and linker
- C source code -> compiler -> assembly
- Assembly -> assembler -> object file (binary files)
- object file and library -> Linker -> executable
    - linker grabs the libraries and takes your files to put in one executable file
- executable and DLL -> loader -> put it in memory
    - loader is part of the OS
    - if you want just some things, an entire library is unnecessary
    - so linker puts a mark to put it in memory later
    - libraries put into memory only when you execute the problem
    - DLL is dynamically linked libraries

