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

# 1/30/24
### OS Management of Application Execution
- the OS is helping to execute several applications
- resources are made available to multiple applications
    - wording note: we are not making it available to users -> users never interact with the OS -> probably only GUIs
- the CPUs are switched among multiple processes so all will appear to be progressing
    - this is called time-sharing
    - processes are added and removed onto the CPU, switched around really fast, looks to the user as if they are running in parallel
- Main goal: the processor, memory and I/O devices can be used efficiently
- requires mechanisms and policies

### policies mechanisms
- policies
    - a set of algorithms
    - make high level decisions
        - which process to run next?
        - when to stop a process?
    - a decision is based on some criteria
        - power
        - performance
        - priority
- mechanisms
    - low level protocol to implement a functionality
        - ex. how to switch one process for the other (context switching)
        - collecting processes metrics
    
### more flow
- executable file on your disk
- command is issued to OS to start executing program
- the loader, part of the oS, extracts needed info from the executable file, loads it into memory, the OS does some initialization, and teh CPU is told to start executing
- when the CPU begins to execute the program code, we refer to this executing entity as a process

### Three kinds of object files (sometimes called mmodules)
- relocatable object file (.o file)
    - contains code and data in a form that can be combined with other relocatable object files to form executable object file
        - each .o file is produced from exactly one source (.c) file
    - this is confusing ^^
    - the compiler doesnt know whether c files are part of the same program or not -> so 3 c files will give you 3 assembly files
        - until you get to the linker, but how does it know to put the code together?
    - relocatable -> need to change addresses in order to combine
    - another example -> i have written a program in C that has two files, assembler gives me two object files
        - look at one of the two object files, the assembler will not know where the program must go in memory, so maybe both files will be assigned to address 0, 
        - maybe there are jumps in both files, jump to address 10 and jump to address 15, 
        - now both files will reach the linker to combine into one executable, it will get first file and put on top and get second file code and put on the bottom
        - the first has jump to address 10, this is correct because it is on the top
        - the second has jump to address 15, this is incorrect now because it will go to the first file, it is no longer address 15 in the second file, maybe its address 115 instead
        - relocatable means the addresses need to be changed in order to be combined by the linker
- executable object file (a.out file)
    - contains code and data in a form that can be copied directly into memory and then executed
- shared object file (.so file)
    - special type of relocatable object file that can be loaded into memory and linked dynamically, at either laod time or run time
    - called Dynamic Link Libraries (DLLs) by windows
### Executable and Linkable Format (ELF)
- standard binary format for object files
    - originally proposed by AT&T system V Unix, later adopted by BSD Unix variants and Linux
- one unified format for
    - relocatable object files (.o)
    - executable object files (a.out)
    - shared object files (.so)
- generic name: ELF binaries
#### the actual format
- ELF header
    - word size, byte ordering, file type (.o,exec,.so), machine type, etc
- segment header table
    - page size, virtual addresses memory segments (sections), section sizes
- .text section
    - code
- .rodata section
    - read only data: jump tables
- .data section
    - initialized global variables
- .bss section (block started by symbol)
    - uninitialized global variables
    - only the length but no data
    - later, the program loader will allocate memory for it
### the process model
- an instance of an executing program and includes
    - program counter
    - registers
    - variables
    - code
    - ...
- thread is the code to be executed
    - multithreaded means you are writing parallel code
- if a program is running twice, is it one or two processes?
    - 2 processes!
- while the program is executing, its process can be uniquely characterized by a number of elements including:
    - identifier
    - priority
    - state
    - program counter
    - where is this info stored?
#### data structure
- the OS keeps a list of all processes in the system
    - process list (also called process table or control table)
- process control block contains the process elements
    - makes it possible to interrupt a running process and later resume execution as if the interruption had not occurred
    - created and managed by the operating system
    - key tool that allows support for multiple processes
#### multiprogramming
- one CPU and several processes
- cpu switches from process to process quickly
    - uses a scheduler to do priority
- this is the illusion -> we think its all executing at the same time, more like the light illusion where multiple things are happening in order really fast