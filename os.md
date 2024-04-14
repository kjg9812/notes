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

# 3/4/24
### two challenges to virtual memory
- speed: VA to PA translation means we need to access the memory twice for each CPU memory request
    - generate virtual address, go to MMU, use address in PTBR, then go to memory to access page table, then get translation to physical address, then go to physical memory
    - this affects performance a lot
- size: page table can be huge, and we have a page table per process
    - page table per process running

### speeding up virtual memory: translation lookaside buffer (TLB)
- solution comes from hardware, another piece next to the MMU
- observation: most programs tend to make a large number of references to a small number of pages -> only fraction of the page table is heavily used
    - so there are definitely more frequently used pages
- solution: TLB
    - small hardware cache inside the MMU
    - maps virtual page numbers to physical page numbers adress without going to the page table
    - contains complete page table entries for small number of pages, the last x (usually 256)
        - previous x pages that you have accessed

### TLB hit
- TLB hit eliminates a memory access, saves time
- VA: virtual address
- PA: physical address
- PTE: page table entry
- the MMU goes to TLB first and does not need to look in the page table

### TLB miss
- incurs an additional memory access (the PTE)
- TLB does not have PA, so it works as usual and goes to the page table to find the translation to physical memory
- but TLB misses are rare
- in case of TLB miss (did not find the address translation you want) -> MMU addresses page table
- remember: if page fault, page is in the disk so that it can move from disk to memory
- note: TLB misses occur more frequently than page faults

### tlb diagram
- we filled TLB from the page table
- is there a scenario where you can find entry in TLB but no actual entry in the page table?
    - yes, tlb miss so i put page in TLB from page table, but maybe this page is removed to the disk
    - so you also need to update the TLB when you remove the page from the memory
- suppose we have one core, one process is running, two processes are in ready state
    - how many page tables do we have? 3 but the CPU is only using 1
    - how many MMUs? 1
- ex. one core, one process running, one is ready and one is blocked
    - how many page tables? still 3, pages tables still exist as long as the processes exist
    - how many MMUs? 1
- only situation where this is not the case is if there is a zombie, a process that is terminated but not removed
- how many MMUs on two cores? 2 because it is hardware

### TLB in case of context switch
- TLB contains translations from the page table of a process
- if a new process is scheduled for execution, the page table of that new process is used
- solution 1: TLB must be flushed and start anew
- solution 2: TLB augmented with process ID
    - but makes it more complicated cause now need to compare process ID and virtual page entry

### thinking about cache (not on exam)
- page is much larger than a cache
- if we have a cache miss go to memory, but to go to memory we need to go to physical memory
- if we get a cache hit, disregard physical address
- so work in parallel, go get the physical memory address from virtual address anyways so you have it
- but theres security and more expensive, so don't access cache with virtual memory at all
- when virtual address comes in it will have the physical address, once you get this access the cache, then see if theres a cache hit or miss, go to memory if miss

### tackling the page table size problem
- assume: 4KB-page, 48 bit address space, and 8 byte PTE (page table entry)
- size of page table needed?
    - (total memory size/page size) * PTE
    - $2^{48-12} * 2^3 = 512 GB$
- wasteful: most PTEs are invalid not used
- solution: multi level page table
    - example: 2 level page table
        - level 1 table: each PTE points to a page table
        - level 2 table: each PTE points to a page
### multi level page table
- to reduce storage overhead in case of large of large memories
- how is this saving space?
- virtual address has virtual page number and offset, if we have two levels divide virtual address into 3 pieces
    - PT1(page table 1), PT2 (page table 2), and offset

# 3/8/24
### why two level page table?
- double accesses now, but TLD reduces this use of page table
- if a PTE in the level 1 is null, then the corresponding level 2 page table does not even exist
- why can entries be null? any program does not use most of its virtual address space
- only the level 1 table needs to be in memory at all times, level 2s can be in the disk
- level 2 page tables can be created and paged in and out by the VM system as they are needd
    - when would we need to create a level 2 page table?
        - when the program needs more space -> stack is growing or your heap is growing
- note: second level page table does not contain physical address, but the physical page number (add the offset for the address)

### another solution: inverted page table
- page table has one entry per page frame not one entry per page
    - for each page slot (frame) it shows which virtual page it has
- each index is a physical page number
- virtual page number, you search for the entries that contain this virtual page number, then the index is the physical page number
- plus: save vast amount of storage
- cons: virtual to physical translation much harder

### page faults
- definition: when the page table is consulted, and we found that the requested page is not in the memory but in the disk
- in reality, the OS does not wait til the memory is full to start making page replacement because the OS wants some spare memory just in case

### replacement policies
- decides what to kick out if its full
- things to consider
    - measure of success
    - cost
        - storage efficiency
        - time efficiency

### optimal page repalcement algorithm
- each page labeled with the number of instructions that will be executed before this page is referneced
- page with the highest label will be removed
- the page that will be accessed farthest in the future will be removed
- impossible to implement because we do not know the future

### not recently used (NRU) replacement algorithm
- two status bits with each page
    - R: set whenever the page is referenced (used)
    - M: set when the page is written
- R and M bits are available in most computers implementing virtual memory
- those bits are updated with each memory reference
    - must be updated by hardware
    - reset only by the OS
- periodically the R bit is cleared
    - to distinguish pages that have been referenced resently

### FIFO
- OS maintains a list of the pages currently in memory
- most recent arrival at the tail
- on a page fault, the page at the head is removed
- so the page that entered first is removed

### the second change page replacement alg
- modification to FIFO
- inspect the R bit of the oldest page
    - if R = 0 page is old and unused -> replace
    - if R = 1 then
        - bit is cleared
        - page is put at the end of the list
        - the search continues
- if all pages have R = 1, the alg degenerates to FIFO
- so its like, get the oldest but if its been used then dont pull that one

### LRU - least recently used
- good approximation to optimal, so closest to optimal
- when page fault occurs, replace the page that has been unused for the longest time
- realizable but not cheap

### LRU hardware implementation
- 64 bit counter incremented after each instruction
- each page table entry has a field large enough to include the value of the counter
- after each memory reference to a page, the counter is incremented in the entrys counter field
- at page fault, teh page with lowest value is discarded
- this is too expensive and too slow!
    - scan through to find lowest counter

### LRU hardware implementation 2
- machine with n page frames (real page in physical memory)
    - so if a page is 4k bytes and physical memory is 16 gb ram, then number of page frames is 16/4k
- hardware maintains a matrix of n x n bits
- matrix is initialized to all 0s
- whenever page frame k is referneced
    - set all bits of row k to 1
    - set all bits of column k to 0
- the row with the lowest value is the LRU
    - because when other pages are used then the columns for other pages are initialized to 0, which contributes to a lower score
    - note, the bits are counted in binary, so 0011 is 3 and etc.
- when a new page comes to memory, do we have to reset?
    - no
- do these pages have to belong to the same process?
    - no they don't, this is looking at physical memory
    - if one process is running it may cause the page of another process to be kicked out
- suppose we have this matrix then turn off the machine and re-boot again
    - if we turn off laptop and open it after a while, can something happen that makes the matrix 10x10 or 7x7; change the matrix dimensionality?
        - matrix is nxn, say you upgraded the RAM, then the matrix will be bigger

# 3/10/24
### how to study for miterm
- read the slides carefully
- listen to lecture
- check questions and answers on dicussion forums

### LRU implementation
- slow
- few machines have the required hardware

### simulating LRU in software -> NFU
- not frequently used (NFU) algorithm
- counters, lowest count is least frequently used
- software counter associated with each page, initially zero
- at each clock interrupt, the OS scans all pages and add the R bit of each page to the counter of that page
    - so if R is 1 then the page has been accessed
    - clear the R bit afterwards
    - what is clock interrupt?
        - an interrupt is something that is external to program, requires OS attention; like a keyboard press
        - the clock is a small circuitry that generates an electrical signal that generates a square wave, down up down up, the frequency means in 1 second how many times it goes up and down; down and up is 1 cycle; 4 ghz machines means in 1 second 4 billion cycles; the CPU works only when the clock is up

- at page fault: the page with lowest counter is replaced

### enhancing NFU
- we don't decrement counter, so what if page is accessed a lot in beginning but not later
- NFU never forgets anything -> called high intertia
- modification:
    - shift counter right 1 bit before adding R
        - dividing by 2
        - ex. 10 -> 01
    - R bit is added to the leftmost
    - an example 10000000 -> 010000000 -> add leftmost 110000000
    - so old bits will eventually fall off
- this modified algorithm is called aging
- the page whose counter is lowest is replaced at page fault

### the working set model
- working set: the set of pages that a process is currently using
- thrashing: a program causing page faults every few instructions
    - every few instructions, there is a severe memory shortage for your process
- question: can a process in blocked state have a page in physical memory?
    - yes, why not -> only if it needs to be kicked out
- important question
    - in multiprogramming systems, processes are sometimes swapped to disk (ie all their pages are removed from memory) when they are brought back, which pages to bring?
- model: try to keep track of each processes' working set and make sure it is in memory before letting the process run
- w(k,t): the set of pages accessed in the last k references at instant t
    - which just goes up over time, but stabilizes on a graph
- so the OS must keep track of which pages are in the working set
- new replacement alg: evict pages not in the working set
- possible implementation (but expensive)
    - working set = set of pages accessed in last k memory references
- approximations
    - working set = pages used in the last 100 msec

### some design issues for paging
### local vs global allocation: how much physical memory should each process be alloweed?
- how memory should be allocated among the competing runnable processes
    - if the OS gives equal physical memory allocation, is it good?
        - no this is a bad idea, some processes might require a lot of memory or less memory than the others
- this is called local algorithms: allocating every process a fixed fraction of the memory
- global algorithms: dynamically allocate page frames
- global works better
    - local alg used and working set grows means thrashing will result
    - if working set shrinks then local algs waste memory

### global allocatio: how does it work?
- method 1: periodically determine the number of running processes and allocate each process an equal share
- method 2(better): pages allocated in proportion to each process total size (in terms o fnumber of pages)
- page fault frequency (PFF) alg: tells you when to increase/decrease page allocation for a process but says nothing about which page to replace

### more how it works
- every few seconds it calculates the number of page faults per second
    - there is an unacceptable number and a low number
    - if high, then the process is given more pages
    - if low, the process has too much memory so page frames may be taken away
- so the OS is keeping track of this for every process

### more design issues: load control
- what if PFF indicates that some processes need more memory but none need less?
    - swap some processes to disk and free up all the pages they are holding
    - which processes to swap?
        - strive to make cpu BUSY (IO bound vs CPU bound processes)
            - IO bound will leave
        - process size, pick the one that is huge in size in terms of pages

### more design issues: page size
- large page size -> internal fragmentation
- small page size ->
    - larger page table
    - more overhead transferring from disk
    - the virtual page number is getting bigger and the entries increase
    - a big array will span several pages

### more design issues: shared libraries
- dynamically linked
- loaded when program is loaded or when functions in them are called for the first time

### cleaning policy
- paging daemon
- a daemon is a process started by the OS and is in the background
- this process sleeps most of the time
- awakened periodically to inspect state of the memory
- if too few page slots are free -> daemon begins selecting pages to evict

# 3/12/24
## Midterm Review
- note, no questions about threads, no deadlocks
- write an assumption of the question is unclear
    - "i understood the question as..."
- 32 bit machine
    - 2^32 virtual
    - maximum physical memory is 2^32 -> can put less however

### process 3 state model
- why i got it wrong -> pay attention to "neglect time taken by scheduler in that question", if you consider this time then all processes could be waiting in ready
- all in blocked? can happen
- all in ready? no, since scheduler will always schedule 1 ready
- two in ready and two in blocked? no, same reason as above
- one in running and three in blocked? can happen

- why don't we have arrows from:
    - blocked to running: because the scheduler will not pick a blocked process to run; in order for process to be in running, the OS scheduler will only pick from the pool of ready processes
    - ready to blocked: how can the process become blocked if its not even running/executing something that requires IO

### shortest time to completion
- why i got it wrong -> pay attention to "assume no other processes with enter the system"
    - then A will be the shortest, BCD can not arrive with a shorter time

### linking
- if a program consists of only a single file, does it require a linking step? 
    - yes, if it requires dynamically linked libraries then it will need a linking step done by the OS

### round robin
- assume time slice is fixed, how to give one process higher priority?
    - add duplicate of the same process to the queue

### fork
- if we start with one process and fork() is called three times
    - what is the max processes we will end up with?
        - 8
        - fork();fork();fork();
    - what is the minimum?
        - 4
        - the forks are split and each child doesnt also call fork, use if statements

### parallel processes
- if we have four CPUs in a multicore processor, what is the largest number of processes that can be running in parallel?
    - the max: 4
    - the min: 0, all processes could be blocked for IO

### main tasks of OS
- what are the two main tasks of the OS?
- maintain an abstract set of resources, and manage the hardware
    - abstraction through virtualization, and abstraction for application programs
    - manage hardware
        - keep CPUs busy, make best use of memory, etc

# 3/27/24
### concurrency
- we'll discuss multithreading
    - threads belonging to the same process

### process vs. threads
- process has its own address space
- threads are created within a process
    - sequential program = one process that contains one thread
- a thread has:
    - an execution state (running, ready, etc)
    - saved thread context when not running
    - access to the memory and resources of its process ( all threads of a process share this )

### benefits of threads
- takes less time to create a new thread than a process
- less time to terminate than a process
    - process control block contains lots of information about what threads need (same open files, data, etc)
- switching between two threads takes less time than switching between processes

### thread
- def: a sequence of related instructions executed independently of other instruction sequences from the same process
- thread can create another thread
- threads within the same process share
    - the process address space
    - any opened files
- each thread has its own stack
- each thread has its own set of registers

### why use threads?
- parallelism
- lighter than processes
- reduction of process blocking due to IO

### different types of threads
- user level threads
- kernel level threads
- hardware threads

### user level threads
- program that has created threads
- all thread management is done by the application
- the kernel is not aware of the existence of threads
- threads are implemented by a library
- kernel knows nothing about threads, it only sees one thread
- each process needs its own private thread table
    - thread table is managed by the runtime system

# 4/2/24
### user level threads (ULTs)
- advantages
    - thread switch does not require kernel mode
    - scheduling of threads can be application specific
    - can run on any OS
- disadvantages
    - a system call by one thread can block all threads of that process
    - in pure ULT, multithreading cannot take advantage of multiprocessing/multicore
- does state model need to change?
    - will MLFQ contain processes or threads?
        - scheduler forgets about processes and only focuses on threads
            - but a process with more threads may have more priority on the CPU
            - so the scheduler has to consider threads with processes in mind

### kernel level threads
- OS is aware of the threads
- thread management is done by the kernel
    - no thread management is done by the application
- windows OS and linux threads are examples of this approach

### implementing threads in kernel space
- kernel knows about and manages threads
- creating/destroying/other related oeprations for a thread involves a system call (since all done in OS)
- advantages
    - kernel can simultaneously schedule multiple threads from the same process on multiple procesors
    - if one thread in a process is blocked, the kernel can schedule another thread of the same process
    - kernel routines can be multithreaded
- disadvantages
    - the transfer of control from one thread to another within the same process requires a switch to the kernel

### combined hybrid approach
- thread creation is done completely in user space
- multiple ULTs from a single appliaction are mapped onto (smaller or equal) number of KLTs

### relationship between ULTs and KLTs
- 1:1
    - user level thread maps to kernel level thread
- N:1 (user level threads)
    - kernel is not aware of the existence of threads
    - eg. early version of java
- M:N
    - not used by OS usually

# 4/2/24
### example 2
- concurrency
- what APIs does OS give you to write parallel code?
    - creating a thread
    - killing a thread
    - waiting for a thread (to impose a sequence)
    - locking
- main thread in the function
- more threads created with pthread_create
- what is pthread_join?
    - wait to return until the thread finishes

### possible executions of example 2
- non deterministic output
- so the order of what happens can be whatever, because it depends on the scheduler
- why is the counter wrong on the second execution and the third?
    - counter = counter + 1
    - two different threads trying to update the same piece of data, so you don't know when a thread will start executing

### POSIX Threads
- Portable Operating SysTEM Interface
- is an IEEE standard
- API
- Maintain compatibilities among OSes
- Pthreads -> a POSIX standard for threads

### POSIX Threads (Pthreads)
- low level threading libraries
- native threading interface for linux
- use kernel level thread (1:1 model)
- developed by the IEEE committees in charge of specifying a portable operating system interface (POSIX)
- shared memory
- because threads within the same process share resources (including all the IO devices):
    - changes made by one thread to shared system resources (such as closing a file) will be seen by all other threads
    - two pointers having the same value point to the same data
    - therefore, requires explicity synchronization by the programmer
- implemented with a pthread.h header
    - ie #include <pthread.h>
- to compile with GNU compiler, 2 methods:
    - gcc/g++ ./progname -lpthread
    - gcc/g++ -pthread ./progname
- programmers are responsible for synchronizing access (protecting) globally shared data

# 4/4/24
### thread execution
- two threads trying to update variable at the same time
    - race condition

### solution for race condition
- busy waiting
- every thread computes its own y
- flag is 0
- basically slow down a thread and make it do nothing until its allowed to
- but
    - busy doing nothing
    - serialization
    - optimizing compilers can mess with it

### mutexes
- mutex = mutual exclusion
- means of protecting shared data when multiple writes occur
- acts like a lock protecting access to a shared data resource
- only one thread can lock (or own) a mutex variable at any given time

### how are locks implemented?
- with hardware there is an instruction set to build locks
- the OS provides techniques to deal with several threads that need to lock

### solution 1: controlling interrupts
- early solution needs special hardware instructions
- something that happens during the lock
- ensures that the code within the critical section will not be interrupted

### solution 2: using loads and stores
- while a variable (mutex) is 1, which means its being used, dont do anything
- when its not anymore then you can set it to 1 and you have that lock

# 4/9/24
- how to deal with critical sections?
    - locks
    - there are many different implementations though

### solution 3: test and set
- a hardware support solution
    - a machine language instruction
- today all systems support this feature
- steps:
    - fetch old value at old pointer
    - store new into old pointer
    - return old value
- evaluating spin locks
    - correctness: we have a correct lock
    - fairness: no fairness is guaranteed. a thread can wait forever for the lock
    - perforamnce:
        - in a single CPU: bad
            - OS schedule will pick one and it could basically just spin and wait
        - for multiple CPUs: not as bad, but still bad

### solution 4: compare and wap
- another hardware support solution
- similar to test and set but with an expected value

### solution 5: load linked and store conditional
- another hardware support solution
- instructions provided by PowerPC and ARM (and old architectures like MIPS and Alpha)
- load linked just reads something from memory
- storeconditional asks if no update to that place in mmeory since loadlinked to that address, then you can return 1 for success and update that pointer
- else, you cannot update and return 0
- unlock will set flag to 0
- lock
    - infinite loop
    - spin while until the flag is 0
    - if another thread has set the lock, then return
        - if set to 1 then we get the lock, otherwise we try again
- correct execution, no fairness guaranteed, and spin locks waste CPU cycles

### Solution 6: fetch and add
- using fetch and add to build ticket locks
- fetch and add returns old value and increment by 1
- while lock does not equal my turn, spin
- unlock increments turn by 1
- at some point the turrn will loop back to you, so there are ticks each thread has a turn
- correct, ensures progress for all threads, but still spin lock

### Solution 7: no spinning
- if you are going to spin, just give up the CPU
- nees now an OS support to allow thread to deschedule itself -> thread moves from running to ready
- so while test and set, flag == 1, then yield() and give up the CPU
- code is correct, reduce CPU cycle waste, but increase context switches, and no guarantees for fairness where a thread may wait forever

### solution 8: sleeping instead of spinning
- if a thread is not getting the lock, put it to sleep and add it to a queue of waiting threads
    - thread is blocked for the lock
- when the lock is released, wake up one of the threads

### locks arent enough
- condition variables are a queue that threads can put themeselves on when some condition is not yet true
- synchronization
- when the condition is true, one of the threads waiting will be signaled to wake up and proceed
    - that is, it moves from blocked to ready, not necessary to running directly
- without condition variables, the programmer would need to have threads continually polling (possibly in a critical section)
    - very resource consuming since the thread would be continuously busy in this activity
    - condition variable is a way to achieve the same goal without polling

### OS needs to provide two primitives for condition variables
- syntax will differ among OSes
- wait()
    - assumes there is a lock acquired over the condition variable
    - releases the lock
    - puts calling thread to sleep (atomically with releasing the lock)
- signal()
    - acquires the lock over the condition variable
    - wakes up one of the sleeping threads over this variable

# 4/11/24
- why do locks save us from the deadlock? or some question idk
    - when condition done is true, there is a context switch, but wait is not called yet so child will start executing, child will try to lock but wait has not executed yet so child will be blocked
### producer/consumer
- producers: a group of threads
    - generate data items
    - place them in a buffer
- consumers: a group of threads
    - grab items from the buffer
    - consume the data
- the buffer is a shared resource -> need synchronization to access it
- buffer is a critical section
    - but is it enough to use locks?
    - regardless of locks, consumer will try to get from the buffer
        - want to only call get when the count == 1
    