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

# 2/1/24
### three state model
- process is running
- becomes blocked because its waiting for IO
- scheduler picks another process that is ready
- when it detects IO then it becomes ready
- then scheduler can pick this process

how can you ask the IO if you are not running
- this is why theres no arrow from ready to blocked

### five state model
- added "new" and "exit"
    - exit, when a process finishes it finishes, why should we keep track of it?
        - 
    - new, to start, need to ensure that you have security clearance and resources to use things -> then you can be admitted to be ready

# 2/6/23
- interrupt -> OS has to take over
- user mode -> exdcuting any program
- kernel and OS mean the same

### simple modeling of multiprogramming
- process spends fraction p of its lifetime waiting for IO
1-p -> it is not waiting for IO
- assme n processes in memory at once
- probability that all processes are waiting for IO at once is p^n
1-p^n -> at least one process not waiting for io
- so CPU utilization is 1-p^n

- multiprogramming lets processes use CPU when CPU becomes idle, we want 100% CPU utilization

### executing the OS itself
- 3 options
1. separate kernel
2. OS functions execute within user processes
3. OS functions execute as separate processes
which is the best for a single core CPU?
- a or b, since c adds more processes
- in b, if part of OS is in the process/code, we reduce system calls and take less time
- we want to decrease the number of processes -> it decreases if the processes are IO bound -> we'll also have more context switching

### two main challenges again
- how can OS implement virtualization of the CPU without adding excessive overhead to the system
    - we want to make sure OS is less heavy in terms of performance
- how can the OS run processes efficiently while retaining control over the CPU
    - how can we guarantee the OS runs when it needs to?

### straightforward solution: direct execution
- OS 
    - create entry for process list
    - allocate memory for program
    - load program into memory
    - set up stack with argc/argv
    - clear registers
    - execute call main()
- program
    - run main()
    - execute return from main
- OS
    - free memory of process
    - remove from process list
- overview:
    - no context switches
    - issues:
        - Q1: but how does the OS guarantee that main() program won't do anything nasty?
        - Q2: also, how can the OS stop this process from running and switch to another process if main never executes return and goes into infinite loop?
### Q1: how to make sure program won't do anything bad?
- answer: introducing kernel mode and user mode
    - when process is running in user mode lots of things it cant do
        - cant read and write in some parts of memory
#### flow of kernel mode and user mode
- OS at boot initializes trap table that contains the address of the different trap handles (aka Interrupt service routines)
    - hardware remembers address of syscall handler
- OS in kernel mode creates entry for process list, allocates memory, loads program into memory, sets up user stack, fills kern stack, then return from trap (go to user mode)
    - hardware restores regs, ..., move to user mode, jump to main,
- program in user mode runs main, make system call, need to trap back into OS
    - hardware saves regs to kernel stack, move to kernel mode, jump to trap handler
- OS handles trap, does work of syscall, return from trap

- note: moving from kernel mode to user mode or other way around is purely in hardware
    
#### what happend here?
- processes run at user mode and cannot access the hardware
- if a process needs something from the hardware, it makes a system call with a specific system call number
- this switches the machine to kernel mode and the OS starts executing 

### Q2: how do we stop a process from infinite loop
- if a process is using the CPU, this means the OS is not running (you can scale that to multi processes with multicore)
- if so, how can OS stop a process to switch to another process?
- solution: timer interrupts

#### timer interrput
- a device programmed to raise and interrupt every x milliseconds
     - is set by the OS
     - OS starts the timer at boot time
- kernel mode activated every x ms
- when the interrupt occurrs, the machine switches to kernel and the OS takes control
    - the scheduler part of the OS
    - the scheduler decides: continue running the current process or context switch to another one

### APIs for dealing with processes in C
- need to include header files
    - header files -> contains function definitions, has some predefined functions and variables without a main
    - a library is already precompiled
```C
#include<unistd.h>
#include<sys/types.h>
#include<sys/wait.h>
```
### basic syscalls for managing processes
- `fork` spawns new process
    - called once, returns twice
- `exit` terminates own process
    - puts it into zombie status until its parent reaps
- `wait` and `waitpid` wait for and reap terminated children
- `execve` runs new program in existing process
    - called once, never returns

#### fork: creating new processes
```c
int fork(void)
```
- creates new process (child process) that is identical to the calling process (parent process)
- fork is called once but returns twice
    - return once to me with process id, and return 0 to the child process
    - this is so you can make parent or child do something different
```c
pid_t pid = fork();
if (pid == 0){
    printf("hello from child\n");
} else {
    printf("hello from parent: child pid is %d\n", pid);
}
```

#### ex 1 on slides
x = 2
x = 0
last statement executed twice
Bye from process pid with x = 2
Bye from process pid with x = 0

- two things going on here, child and parent executions
- new child has its own address space and everything
- new variables and stuff too

# 2/9/24
- fork
    - parent and child both run same code
    - start with same state, but each has private copy of memory
        - including shared output file descriptor
        - relative ordering of their print statements undefined
    - pid_t is same as int
    - pid for child is 0, pid for parent is the id of the child process

#### example 2 on slides
L0
L1
L1
Bye
Bye
Bye
Bye
- so fork, after you call it, everything after will be executed by the parents and the children

#### example 4
- you can set conditionals, so only if fork() does not return the child
    - if the fork is the parent

### exit: ending a process
- void exit(int status)
    - exits a process
        - normally return with status 0
    - atexit(function_name) make function_name execute upon exit
```C
void cleanup(void) {
    printf("cleaning up\n")
}
void fork6() {
    atexit(cleanup);
    fork();
    exit(0);
}
```

### zombies!
- when process terminates, still consumes system resources (ex. an entry in process table)
    - why? so that parents can learn of children's exit status
- called a "zombie"
- reaping
    - performed by parent on terminated child
    - parent is given exit status information
    - OS discards process
- what if parent doesn't reap?
    - if parent has terminated, then child will be reaped by init process
- in linux, ps shows processes with their PID
    - kill (pid) stops the process

### wait: synchronizing with children
- int wait (int *child_status)
    - blocks until some child exits, return value is the pid of terminated child
    - if multiple children completed, will take in arbitrary order(use waitpid to wait for a specific child) 

### execve
- int execve(char *fname, char *argc[], char *envp[])
    - executes program named by fname
    - name of executable and the arguments of that executable
    - execute code ps, first it will fork, that child will do the same thing
    - so if pid = fork() = 0 -> child
        - then replace process with new process loaded from the file
    - basically, if you have a child, your child will execute code from a different program rather than the parent

# 2/13/24
### scheduling
- given a group of ready processes, which process to run next?
#### when to schedule
- when a process is created
    - new process might have higher priority
- when a process exits
    - that means process is done
- when a process blocks
    - need new process
- when an io interrupt occurs
    - blocked to ready process
### definition
- preemptive scheduling: a process can be interrupted, and another process scheduled to execute
- non premptive scheduling: the current running process must finish/exit first before another process is scheduled to execute

# 2/15/24
### scheduling strategies
- first come first serve
- shortest job first
    - once process is on CPU it stays until it finishes
- shortest time completion first
    - when we have a batch system (users submit jobs to the system, jobs collected in groups or batches processed without user interaction)
        - most systems are interactive now
    - when we know the total runtime of a process beforehand
        - not realistic
### metric
- response time
    - in interactive systems
    - depends on arrival time and the first time a process is scheduled to run
    - Tresponse = Tfirstrun - Tarrival
    - this is basically begin minus end
### scheduling in interactive systems: round robin
- each process is assigned a time interval (quantum or time slice) to stay on the CPU
    - ex. process is on cpu for 10 cycles
- after this quantam, the CPU is given to another process
- what is the length of this quantum?
    - too short -> too many context switches -> lower CPU efficiency
        - need to take time to switch
    - too long -> poor response to short interactive

### example
- processes a b and c arrive at same time 0, each needs to run for 5 seconds
- is response time or turnaround time better?
    - the question is not complete, is it an interactive system or batch system?
    - in an interactive system: you need the user to feel something happening(they provide input to the system), its not important when it finally completes, so response time is better

### scheduling in interactive systems: priority scheduling
- each process is assigned a priority
- ready process with the highest priority is allowed to run
- priorities are assigned statically or dynamically
    - dynamically: one process has some priority and as long as buffer from ethernet is empty -> then priority drops
- if you are designer of OS will you allow user to assign priority of processes?
    - no
    - assigned by OS depending on how crucial process is
        - ex. IO handling, ethernet data buffer pipeline because it will kill internet, etc
- must not allow a process to run forever
    - can decrease the priority of the currently running process
    - use time quantam for each process

### desires
- optimize turnaround time
- reduce response time for interactive users
- without knowing the total runtime of a process a priori
- Multi level feedback queue (MLFQ)
- side note: can we use deep learning for the scheduler?
    - in theory yes
    - but its a heavy piece of software and takes too much time

### MLFQ
- we have several distinct queues
- each queue is assigned a different priority level
- a process that is ready to run is on a single queue
- a process priority may change overtime
    - that is, assigned to a different queue
- rules:
    - rule 1: if priority (A) > Priority(B), then A runs and B doesnt
    - rule 2: if priority A = priority B, a case where A and B are in the same queues, A & B run in RR (round robin)

# 2/20/24
### memory hierarchy
- as you go up, speed increases and cost increases
- as you go down, density and capacity increases
- cache is made of SRAM
    - static ram, only using transistors
    - every bit stored on a buffer, and density is much lower
    - much faster than DRAM
    - usually managed by the hardware
- main memory is made of DRAM
    - charge is on capacitors
    - managed by the OS
- disk storage can have lots and lots of storage
    - managed by OS
    
### moores law
- speed of CPU keeps going up exponentially
- memory performance does not
- this is bad because the memory will become a bottleneck for the cpu
    - increasing cpu performance wont matter

### memory abstraction
- the hardware and OS memory manager make you see the memory as a single contiguous entity
- how do they do that -> abstraction

### no memory abstraction
- several setups
- user program and the OS in RAM
- OS in ROM and user program
- device drivers in ROM and user program/os in DRAM
- only one process at a time can be running here
- so if we want to run multiple programs
    - os saves entire memory on disk
    - os brings next program
    - OS runs next program
    - this is swapping
- we can use swapping to run multiple programs concurrently
    - memory divided into blocks
    - each process assigned to a block
    - example: IBM 360

### issue arises
- when we put two blocks of memory on top of each other and our code says to jump to a certain line with no abstraction, since programmers are using absolute address, the addresses change
- we can use static relocartion at program load time
    - at load time, go through instructions and change the jumps/addresses to ensure the correct execution
    - but this is a bad idea because it will be very slow! and will require extra info from the program
    - so we need memory abstraction

### memory abstraction
- to allow several programs to co exist in memory we need
    - protection
        - make processes unware that there are other processes
    - relocation
        - need to make sure jumps are executed correctly
    - sharing
        - need to let processes share things without knowing that they're sharing
    - logical organization
    - physical organization

### protection
- processes need to acquire permission to reference memory locations for ready or writing purposes
- location of a program in main memory is unpredictable
- memory references generated by a process must be checked at run time

### relocation
- programmers typically do not know in advance which other programs will be resident in main memory at the time of execution of their program
- active processes need to be able to be swapped in and out of main memory in order to maximize processor utilization
- specifiying that a process must be placed in the same memory region when it is swapped back in would be limiting
    - we may need to relocate the process to a different area of memory

### sharing
- it is advantagous to allow each process access to the same copy of the program/libary/ rather than have their own separate copy
- memory management (OS), must allow controlled accesss to shared areas of memory without compromising protection
    - ex. cannot write to a library only read

### logical organization
- we see memory as linear one dimensional address space
- a program = code + data + ... = modules
- those modules must be organized in that logical address space
- so this is what we think is happening

### physical organization
- what is really happening
- memory is really a hierarchy
- several levels of caches, main memory, disk
- managing the different modules of different programs in such a way as:
    - to give illusion of logical organization
    - to make the best use of the above hierarchy

### all of this must be done while ensuring
- transparency: the running programs must not know that all of this is happening
- efficiency: both in terms of time (speed) and space (not wasting lots of memory)
- protection: as we saw, protecting processes frome ach other

### address space: base and limit
- map each process address space onto a different part of physical memory
- two registers: base and limit (only accessed by OS)
    - base: start address of a program in physical memory
    - limit (sometimes called bound): length of the program
- for every memory access
    - base is added to the address
    - result compared to limit
    - who is doing this? a piece of hardware inside the processor called the memory mangaement unit (MMU)
- only OS can modify base and limit

main drawbacks:
- need to add and compare for each memory address
- what if memory space is not enough to all programs?
    - then we may need to swap some programs out of the memory

### swapping
- programs move in and out of memory
- holes are created in the memory
    - check that visual
    - holes cam be combined -> memory compaction
- processes can take up variable space so its like tetris at that point
- what if a process needs more memory?
    - scenario 1: if a hole is adjacent to the process, it is allocated to it
    - scenario 2: process must be moved to a bigger hole
    - scenario 3: process suspended till enough memory is there

### managing free memory
- bitmap:
    - memory is divided into allocation units of equal size
    - each unit has a corresponding bit in the bitmap
    - 0 = unit is free, 1 = unit is occupied
    - bitmap is slow: to find k-consecutive 0s for a new process
- linked list: of allocated and free memory segements
    - segments are of different sizes

### linked list
- more widely used
- linked list of allocated and free memory segments
- usually doubly linked list in case you need to combine process and hole segments
- ex. a node is either a process with its base and limit or a hole with its base and limit

# 2/22/24
### managing free memory
- again, how are we keeping track of what's used and where there are holes in memory
- bit map
- linked list
- why do we need to compact slots in memory?
    - if we know theres a hole from 5-10 and 10-15, these sizes are too small for something thats length 10, so combine the holes because they are actually big enough

### how do you allocate memory?
- different strategies
    - first fit
        - scan list until you find first hole that is big enough
        - the fastest, but not the best probably
    - best fit
        - find the best memory usage
    - next fit
        - use first fit then keep a pointer pointing to where you stopped
        - when you get another process continue from that pointer
    - worst fit
        - take the biggest hole that is available, use part of it for processs
        - the rest will still be useful for another process
        - maybe if you take the best fit will create a hole that is so small that it's useless
### memory management techniques
- memory management brings processes into main memory for execution by the processor
    - involves virtual memory
    - based on
        - segmentation (variable size parts) or
        - paging (fixed size parts)
- processor is not connected to the disk, only the memory
    - OS needs to move things from memory and disk
- processor doesnt know how big physical memory is

### conclusions
- process is cpu abstraction
- address space is memory abstraction
    - OS memory manager and the hardware helps provide this abstraction
- two main tasks needed form OS regarding memory management
    - managing free space
    - making best use of the memory hierarchy

### what is the issue with memory
- theres not enough
- having enough memory is not possible
    - how do you determine enough?
    - programs keep growing in size as years go by
- processor does not execute anything that is not in the memory

### hints
- why the text segment of any process starts at the same address?
    - same address on all processes? hmmmm
- why dont you run out of memory even if the sum of every memory requirement of all your programs is more than what you have
- the address is 64 bit in 64 bit machines
    - so memory addresses will be from 0 to 2^64 - 1 which is way more than the amount of memory you have on your machine

### hint intuitions
- all memory references are logical (virtual) addresses that are dynamically translated into physical addresses at run time
    - so all addresses are not the real addresses -> they need to be translated
    - so the cpu knows nothing about the actual setup of memory
- a process may be broken up into several pieces that don't need to be contiguosuly located in main memory during execution
    - so addresses can be anywhere in actual physical memory
    - this way we can use memory effectively
- so its not necessary that all pieces of a process be in main memory during execution
    - maybe a bunch of stuff is in the disk until its needed

### definition of virtual memory
- mapping from logical (virtual) address space to physical address space

### implementation of virtual memory
- paging!
    - when we have whole memory we divide it into pieces of equal size
    - i have 16 gb of ram and virtual memory 2^64
        - take 16 gb of ram and divide it into 4000
            - each 4000 bytes is a page
    - which will have more pages?
        - virtual memory
    
### the story/flow
1. OS brings into main memeory a few pieces of the process
    - what is meant by pieces?
2. an interrupt is generated when an address is needed that is not in main memory
    - how do you know it isnt in main memory?
3. OS places the process in a blocking state
4. OS issues a disk I/O read request
    - why?
    - please put this stuff into memory
5. another process is dispatched to run while the disk IO takes place
6. an interrupt is issued when disk IO is complete, which causes the OS to place the affected process in the ready state
    - interrupt is a signal sent to CPU from hardware
7. piece of process that contains the requested logical address is brought into main memory
    - what if memory is full?

### virtual memory
- each program has its own address space
- this address is divided into pages
    - so a page is a block of used memory that the program used
- pages are mapped into physical memory

# 2/27/24
- every process thinks it has 2^64 addresses in memory
- use part of the disk as an extension to the memory
- CPU has no bus to connect to the disk, only to memory
### the flow
- CPU gives a virtual address to the MMU(memory management unit) which translates it to the physical address in main memory
- thought exercise?
    - assume that page size is 4k bytes -> 4k on each page until you get to 16gb of memory
    - in virtual memory same thing -> 4k on each page until you reach the end
        - this will have more pages since the end is 2^64
### virtual memory
- advantages:
    - illusion of having more physical memory
    - program relocation
        - page may be kicked out to the disk and brough back when its needed, since physical memory can get full
        - it will make the best use of the physical memory
    - protection
        - protect processes from each other

### addresses/offset
- since virtual is much bigger, the number of bits you need in physical is much less than virtual for the page
- the address will be the page number and the page offset for the specific line in the page
- need 10 bits(2^10) to specify the offset if the page is 1k

### page table
- CPU, memory, disk are hardware
- MMU is hardware in the CPU
- page table is a data structure used by the operating system
    - created per process
    - so 10 processes means 10 page tables
- virtual page number is an index in a table that contains the physical page number
    - also has a valid bit to say that the page is in memory (1 it exists 0 else)
- the offset will be the same in virtual memory page and the physical memory page (its an offset within a page) -> basically our groups are just in different spots between virtual and physical (groups being pages)
- how does the OS tell the hardware where to find the page table
    - there is a page table base register (PTBR)
    - MMU just caares about which page table to use for the translation

### page table base register
- the OS maintains information about each process in a process control block
- the page table base address for the process is stored there
- the OS loads this address into the PTBR whenever a process is scheduler for execution
- when the scheduler decides a process is running
    - one step is to put the address of the page table into the PTBR
- only the kernel (the OS) can access PTBR

### address translation: page hit
- CPU sends virtual address to MMU
- where is the page table? in memory
- where is the process table stored? in memory
1. processor sends virtual address to MMU
2-3. MMU fetches page table entry from page table in memory
    - get page table entry addreess, then add the offset
4. MMU sends physical address to cache/memory
5. cache/memory sends data word to processor

### address translation: page fault
1. processor sends virtual address to MMU
2-3. MMU fetches page table entry from page table in memory
4. valid bit is zero, so MMU triggers a page fault exception in kernel
    - process will be stopped and blocked in kernel mode
    - so MMU goes to page fault handler after the exception
    - if VA (virtual address) is invalid, kill process
    - if VA has been paged out to disk, then swaps in faulted page, update page table, resume faulted process
        - also requires an eviction algorithm if the memory is full
- what does page fault handler do?
    - OS looks at the address

