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