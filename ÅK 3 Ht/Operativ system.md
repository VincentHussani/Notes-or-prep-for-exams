# Tenta

## 1.1
Which of the following operations is allowed in usermode?
* None of the listed operations is allowed in user mode (correct)
* read an entry in the page table
* modify the interrupt vector
* directly access a sector on a disk
* turn off interrupts

Usermode is one of the two modes most modern operating systems offer (the other being kernelmode). It is a so called dual mode operation where some system instructions are seen as priveleged instructions tht can cause harm. Interrupt handling is one of those privleged instructions, so is directly working with the memory. Accessing pagetables or sectors on a disk is strictly forbidden leaving meaning that none of these alternatives are allowed.

## 1.2
Protection is needed in many places in a computer system, e.g., for the processor ('CPU protection'). How can we implmenet CPU protection?
* Using dual-mode operation (user-mode and kernel-mode) (denna är möjligtvis korrekt)
* Using semaphores
* None of the alternatives provide CPU protection
* Using a timer and clock interrupts (men denna är definitivt korrekt)
* Protecting the interrupt vector from user-mode access

Jag är personligen osäker på denna, CPU protection uppnås med hjälp av dual-mode operations men det inkluderar timer och clock interrupts

## 1.3
Which of the following operations will result in a system call in a normal operating system?
* Allocating dynamic memory on the heap
* reading a file from disk (correct)
* push variables on the thread's stack
* executing a conditional branch instruction
* none of the alternatives

Reading a file from disk requires your operating system to fetch the file from your hardware. Anything relating to hardware always requires a systemcall as the os acts as a middleman between the user and hardware.

## 1.4
Which of the following alternatives is a mechanism/technique for inter-process communication (IPC)
* None of the other alternatives
* Message-passing (correct)
* Dual-mode operation
* Contiguous synchronization
* Hardware interrupt

https://en.wikipedia.org/wiki/Inter-process_communication
Interprocess communication are methods which allow processes to manage shared data. Of these Message-passing is the only data related task and also mentioned on the wiki about IPC

## 1.5
Which of the following shceduling algorithms gives the shortest average waiting time
* Shortest-job-first (correct)
* Priority scheduling
* Fair-share scheduling
* Round-Robin
* First-come first served

Shortest-job-first scheduling gives the shortest average waiting time but is not practical in reality as it requires you to the execution time of a process.

## 1.6
Which of the following scheduling algorithms is most suitable in a realtime system where it is important to meet certain deadlines?

* priority scheduling (correct)
* They all work equally well
* Round-robin
* First-come, first-served
* shortest-job-first

Priority scheduling is the most suitable algorithm in real time scenarios as the processes with a deadline can be given a higher priority.

## 1.7
A TLB (translation look-aside buffer) improves performance by
* Looking in the interrupt vector in advance and thus can handle interrupts faster
* Using a small table of the most recent translations between logical and physical addresses
* All alternatives are used
* Storing the most recently used disk blocks in main memory
* Keepin track of which virtual pages are stored where on the external disk

A TLB stores is a memory cache that stores the recent translations of virtual memory to physical memory. You find the TLB in the MMU (Memomory management unit)
https://en.wikipedia.org/wiki/Translation_lookaside_buffer

## 1.8
Which of the following terms below does not refer to a method used for allocation of disk space for a file
* Indexed (i-node) allocation
* Contiguous allocation
* linked-list allocation
* All alternatives are valid allocation methods
* Virtual allocation (correct)

Virtual allocation is not an allocation method but a call such as malloc :)

## 1.9
Assume a FAT (file allocation table) file system. The disk has 8192 bytes (8 Kbyte) blocks and each FAT entry is 4 bytes large. The FAT is stored in a single disk block. How large disk partitions can be handled by this system?

Okay we know that:
* one block is 8kb
* our fat is 8kb
* one entry is 4b

Thus the capacity of this file system is:
 $$ \frac{fat}{fat entry size}*Blocksize = number of slots * Blocksize = \frac{8kb*8kb}{4}=\frac{64mb}{4}=16mb $$

## 1.10
Which of the following devices is a typical block device?
* None of the alternatives is a block device
* High-resolution display (i/o device)
* Keyboard (i/o device)
* Solid-state disk (block device)
* line printer (i/o device)

Blockdevices are nonvolatile(stored even when shutdown) mass storage devices whose information can be accessed in any order. Of these alternatives SSD is the only block device

## 2.1
What is a process control block (PCB) and why doe we need it? Give four examples of data that are stored in a PCB

PCB is a data structure used by OS to store information about a process. When a process is created a the OS creates a corresponding process control block. They're required to know the state of a process. It's essential for the handling of a process during a context switch as the pcb stores information about register values and etc.

The type of data stored on a PCB includes:
* Program counter
* Process state
* Privleges
* Process id
* I/O status info (which I/O devices are allocated to the process)
* Process scheduling state

## 2.2
Assume a system with one CPU and a time quantum of 2 ms.

Five processes enter the system at specific times, each with a specific execution time and a certain priority level (5 is the highest priority and 1 is the lowest priority).Assume that a task will be eligible for scheduling immediately on arrival.


Give your answer as (as an example) : G,H,H,F,...,

where each letter corresponds to 1 ms!

A: 0ms, 3ms, 1

B: 2ms, 4ms, 2

C:4ms, 2ms, 3

D:7ms, 1ms, 4

E:9ms, 2ms,5


**In which order are they schedules/executed according to the fifo algorithm?**

FIFO will execute them in the order that they arrive
Answer:

*AAABBBBCCDEE*

Time quantum is not smth we have to worry about as it is not relevant with FIFO

**In which order are they scheduled/executed according to the round-robin algorithm?**

The roundrobin algorith works by using the time quanta assigned to swap processes in a cyclical fashion. A timequanta of 2ms means that our algorithm will look for a new process every 2ms. The previous process will be put in the back of the list waiting for its turn.

The first two miliseconds will be

*AA* since no other process exists. B enters and replaces A. A is placed in the waiting list. Our string of operations will now look like this

*AABB*. Another context switch happens since we've passed the timequanta. First thing to happen is putting C in the queue, B is put in the queue with 2ms left of execution time, A gets to finish its business

*AABBA*. A finishes and a context switch is forced. C is first in the list and gets its time. The time quanta timer is reset on forced context switch

*AABBACC*. Seven seconds has passed. D is put in the back of the list, C is done and B gets its time

*AABBACCBB*. B has finished, E is put into the list. D starts

*AABBACCBBD* forced context switch. D is done E starts

***AABBACCBBDEE*** is the final answer


**In which order are they scheduled/executed according to priority scheduling with preemption**

Priority scheduling does a context switch as soon as a higher priority process is available. A gets CPU time first since there are no competing processes

*AA*. B enters and has a higher priority

*AABB*. C enters with a higher priority than B. B has 2 seconds of execution time remaining whilst A has 1.

*AABBCC*. D has yet to enter so B gets to execute once

*AABBCCB*. D enters with a higher priority

*AABBCCBD*. D finishes, B gets more time

*AABBCCBDB*. E enters with higher priority and runs without interruption

*AABBCCBDBEE* A is the only remaining

***AABBCCBDBEEA*** is the final answer

A process can be in different states which state transitions can occur?

Blocked -> Ready
Ready -> Running
Running -> Blocked
Running -->ready

# 3.1

Consider the following pseudo code where three semaphores (A,B, and C) are declared and three processes (P1, P2 and P3) execute concurrently. Semaphore A(0) means that A is iniialised to the value 0 etc
```c
Semaphore A(0), B(0), C(1);

P1:
// do something
down(A);
print("A");
up(C);
// do something else

P2:
//do something
down(B);
print("B");
up(A);

P3;
// do something
down(C);
print("C");
up(B);
//do something else

```
Since C is initialised with 1 whilst the other are 0 C will be printed first. B is upped unlocking P2 and finaly A

Answer:CBA

# 3.2
Which are the four conditions that must hold for a deadlock to occur?

1. Mutual exclusion - only one thread can hold a resource at a time
2. Hold and wait - Threads can hold a resource whilst waiting on another
3. No preemption - Threads cannot steal resources
4. Circular wait condition - resources a is allocated to thread 1, thread 1 now needs resource b which is allocated to a process which is waiting for resource a

# 3.3
Consider the following concurrent code section
```c
// Shared Variables
double bankAccountBalance = 0.0;

void deposit(double amount) {
    bankAccountBalance += amount;
}

void withdraw(double amount) {
    bankAccountBalance -= amount;
}
// simulate id performing 1000 transactions
void do1000Transactions(unsigned long id) {
for (int i = 0; i < 1000; i++) {
    if (num % 2)
        deposit(100.00); // odd threads deposit
    else
        withdraw(100.00); // even threads withdraw
}
}
void* child(void* buf) {
    unsigned long childID = (unsigned long) buf;
    do1000Transactions(childID);
    return NULL;
}
int main(int argc, char** argv) {
    pthread_t *children;
    unsigned int id = 0;
    unsigned int nThreads = 20;
    children = malloc( nThreads * sizeof(pthread_t) );
    for (id = 1; id < nThreads; id++)
        pthread_create(&(children[id-1]), NULL, child, (void*)id);
    do1000Transactions(0); // main thread work (id=0)
    for (id = 1; id < nThreads; id++)
        pthread_join(children[id-1], NULL);
    printf("\nThe final account balance with %lu threads is $%.2f.\n\n", nThreads bankAccountBalance);
    free(children);
    return 0;
}
```
a) The code above contains a race condition. Describe what a race condition is, where in the code it is and why it occurs. [2p]

- A race condition happens when two processes/threads are trying to modify the same data at the same time. Process A & B are trying to change the same variable. Let's say that thread A is a bit quicker and accesses the variable before B, reading it in. Whilst A is modifying the read value, B reads it in as well. A writes the value, and B which had read an old version of the variable eventually overwrites A's work. It's a race to see who overwrites who.
- It occurs when withdraw and deposit are called
- It occurs because multiple threads can access the bankaccountbalance variable simulatanously but only one of their results will be stored whilst the others are discarded.

b) Modify the code and remove the race condition using test_and set instructions/operations. You only need to show the modified sections of the code. [1p] otroligt osäker

```c
volatile int lock = 0
void deposit(double amount) {
    while(test_and_set(&lock)==1);
    bankAccountBalance += amount;
    lock = 0;
}

void withdraw(double amount) {
    while(test_and_set(&lock)==1);
    bankAccountBalance -= amount;
    lock = 0;
}
```
c) by using test_and_set instructions/operations, we introduce busy waiting. describe what busy waiting is and why it may be a problem.

- Busy waiting is when a thread or process is consuming CPU time whilst doing nothing in wait for something to happen. Busy waits block other processes from using that time which the busy waiting process has no use for
- Busy waiting consumes unnecessary resources and reduces performance

d) propose a solution without busy waiting to the problem above [1p]
- mutex :)

# 4.1 osäkerish (ganska säker)
Typically processes require space for two variable sized memory regions: the stack and the heap.

Describe why they need to be of variable size, which type of data is stored in each place (how they are used), and where they typically are placed in the memory space to allow the maximum variation in size of each region.

- The stack is used for temporary variables created by a function, methods, and reference variables.
- A heap is a memory to store global variables and dynamically allocated memory.
- The heap and stack are placed "last" and grow towards eachother

# 4.2
Consider the following piece of code:
```c
#define SIZE 8192
static int a[SIZE];
static int b[SIZE];
int main()
{
    int i = 0;
    for (i = 0; i < SIZE; ++i)
        a[i] = i;
    for (i = 0; i < SIZE; ++i)
        b[i] = a[SIZE - i - 1];
}
```
Estimate the number of page faults that are generated when the program is executed on an embedded computer with 8kB of main memory, a page size of 1kB, FIFO replacement, and the size of an int is 4 bytes.

The first access will of course be a page fault. This will load in the first $\frac{1kb}{4b}$ slots of the list which is 256 slots meaning that no pagefault will occur until i = 256 which will generate another fault. We have a total of 8 pageframes

In other words,every 256 accesses will cause a page fault. Thus, every 1024 readings will cause 4 pagefaults. That gives a total of $4*8=32$ pagefaults for the first for-loop.

In the other for-loop, the first 1024 are of importance

b[0:1024] has never been read in, this will cause a pagefault, but a[8912-1024:8192]is already in the memory and will not be deleted upon reading in the slots of b.

The first 1024 readings will therefore cause 4 page faults whilst the remaining accesses cause double that every 1024 readings. Afterwards every 1024 will cause 8 page faults since both a and b have the read in.

This results in us having a total of $4+8*7 = 4+56=60$ pagefaults in the second for-loop.

A total of 32 + 60 = 92 pagefaults will occur by running this program

# 4.3
Assume a segmented memory with the following segment table

segment number: base, limit

0: 1024, 128

1:4096, 2048

2:512, 128

3:2048, 512

which physical address corresponds to the logical address <2,32>?

- Let's look at the base value of segment number 2
- it is 512.
- To get the physical address we have to add 512 to 32 and make sure our value is less than 512+128
- 512 +32 = 544 which is lower than 512+128
- answer is ***544***

# 5.1
Assume a mechanical hard disk that rotates with 7500 rpm (rotations per minute). How long is the average rotational delay

we know that:
$$\frac{7500rpm}{60s}=125 v/s$$
this is:
$$\frac{1}{125}=8ms/v$$
Each rotation takes 8 ms. The average delay will then be mean between the worst and best performance
$$\frac{8+0}{2}=4ms$$

# 5.2
Assume a (mechanical) hard disk that rotates with 7500 rpm (rotations per minute). The disk has 800 sectors per track, and each sector (i.e., a disk block) is 512 bytes (0.5 kB). What is the maximum data transfer rate (in MB/s)?

We know that we can maximally transfer $800*0.5kB = 400$ kb per rotation

We can do 1 rotation in 8 ms

$\frac{400kb}{8ms}=50mb/s$

answer is ***50mb/s***

# 5.3 osäker
These are methods of transferrring data between the CPU and I/O.

Under programmed I/O, data transfer is initiated by instructions written in a computer program. Processor issues a command to the I/O module and waits for the operation to complete. Also, the processor keeps checking the I/O module status until it finds the completion of the operation. This wastes several CPU cycles but easy to implement

Interrupt-driven I/O uses an interrupt and to let the know that an operation is completed. This allows the processor to do its business in the meantime but can become expensive as constant interrupts will cause many context switches. It also requires additional hardware such as a DMA controller chip
# 5.4 osäker
The only way to access kernel mode is through interrupt, calling procedures is usermode stuff
# 5.5 osäker
DMA is a feature which allows certain hardware to access main system meory independently of the CPU. With DMA, the cpu can initiate a transfer, and then go on about its business until it receives an interrupt from the DMA contoller when the operation is done. This allows non programmed i/o which removes wasted clockcycles in the form waiting for a I/O operation signal completion. But it requires extra hardware :( (more can be read on chapter 7.1)
***

# FL 1
## What is an operating system?
A operating system serves two main purposes: to hide complexity of the hardware and manage resources in a fair way.

Operating systems manage many different components which have to work together in order to fulfill their task. Communication between hardware occurs through device controillers and device drivers.

* A device controller controls a device and supplies the api
* A device driver talks to the controller e.g
  * driver says: I want first block of the file X
  * controller translates it to i want 512 bytes from sector

### I/O devices
Controllers provide a somewhat good looking API for OS to interact with devices

Commuicaction via software drivers
- Device drivers written by manufacturers execute in kernel mode

### Methods of working with I/O devices
Start I/O - then poll
- Busywait
Start I/O - then interrupt
- Block caller until I/O done

Start I/O then DMA
- CPU configures the DMA to transfer bytes from local buffer to ram. Fires of the DMA. DMA interrupts when done

## Buses
* Lots of communication channels are called buses in a computer
* Every bus looks different in accordance with its goals
* in concept: a bus is a communication path with rquest/reply
***
## CPU instructions

* The CPU contains a set of instructions
  * Add,mul,sub,mov for example

Some instructions are restricted (gatekeeping) depending on your access level. The operating system works in kernel mode (also called ring0), and thus has access to all instructions. Users of a computer tend to be in usermode.

Usermode relies on syscalls for restricted instructions. A syscall is basically kindly asking your computer if they can do X for us (syscalls are requets from ring3 to ring0 using an interrupt).

## CPU is pipelined
If you remember piplining from DATORTEKNIK it should come as no guess.
Doing one instruction at a time and leaving all the other slots empty is not good so we have multiple instructions being processed simultaneously in different steps of the way and then held in a buffer until they can be executed.

Conditional executions are handled by guessing, if a guess is wrong the pipeline is emptied and starts over :)

## How does an interrupt work?

### Short version
1. Pauses current process
2. Handle interrupt
3. Continue current process

### Longer version
1. Interrupt signal contains information about which procedure to execute
2. Found in IDT vector
3. Procedures are called interrupt handlers
4. Push current Process control block (contains information about the current process) onto stack
5. Setup new stack
6. and JMP to position in interrupt vector
7. Carry out procedure
8. Resume previous process as if nothing happened
***
## Memory Structure
Memory is divided into different layers, depending on size and speed. It's impossible to have both large and fast storage so many different memories exist each for their own purpose. Memory is connected to the CPU and I/O through buses :)

### Cache: fast small memory

The cache is a fast memory close to CPU which is further divided into 3 levels. Each level being bigger but slower than the previous one. The cache stores the most recent references from memory

A cache stores stuff in a cache line. The data is fetched from RAM and if it already is in the cache then A cache hit occurs which is good :)

### How the cache works
First we check the cache, then the memory
- If what we're looking for is already in the cache then the information is used
- If not look in memory
  - Move it from memory to cache

A cache line is typically 32-64 bytes and requires extra care from the programmer to make your data "cache friendly".

### Problems with the cache
- What to cache what not to cache
- what to evict
- where to place the evicted cache
- cache size and replacement policy
***
## System calls (syscalls)
Requests from user to kernel: Think:
- Open file
- Create new process
- printf

Usermode application will send request to "open this file, read this many bytes and place them here
- The operating system figures out the rest
- OS returns code
- Errno means error

### A system call example :)
command: Count = read(fd,buffer, nbytes)

* read nbytes from file fd
* push parameters onto stack places syscall number in register
* trap (move to kernel)
* kernel looks up syscall number and calls handler
* results returned and user cleans the stack
* Handler can block
  * Block process and execute other processes

### Type of sys calls
* process managment
  * Fork - allocate resources and start a new process
* memory managment
  * Brk- chaging tables
* File management
  * Open / close files
  * Read and write
  * talking to controllers
* Directory
  * creating/removing directories
  * talking to controllers
* Protect the system from malicious users
***
## Files
Files are an abstraction made for programmers and users to display data
* They can be stored on disks, usb, RAMDISK and many other mediums

Files have names which translate to block numbers at the lowest level which is what our filesystem manages

Directories are used to create hierarchies from the root folder

Syscalls for files
* Create, remove, rename

Each process has a cwd (no idea what this is)

File descriptors
- A handle to open a file, eg where to read and write

***
# FL 2 Processes and threads
## Introduction
PC, Servers, Tablets, mobilephones do many tasks and apparently in paralell (not really but it appears as parallel because they're done so quickly, it's called pseudo paralellism)

Each task is managed by one or more entities named processes/threads. Processes/threads use system resources such as CPU power and memory. They compete for the use of resources, this competition is handled by the operating system through scheduling and Interprocess communication (FL 3)

## Processes
A runnable software is organised into a number of sequential processes

***A process***
* is an instances of an executing program
* includes the value of the program counter, register and variables
* has its own virtual CPU (a process always thinks it has sole access to the cpu)

The program is an algorithm expressed in some language

The process is the activity performed when executing the program (Can be seen as an instance of said program. It has
* The program
* the input/output
* a state

Executing a program twice yields two processes, each of them being a seperate instance :)

In a single CPU system, processes are swapped back and forth to simulate parallelism.

## Process creation
Creating processes can occur in the following ways
 * System initialization
   * Foreground/background processes
   * deamon processes
 * Execution of a processes creation system call by a running process
   * e.g you start firefox, and it creates another process to manage each tab/window
 * A user request to create a new process
   * double clicking on a program icon
 * Initialisation of a batch job
  * e.g in linux with crond (no fucking clue)

### Trivia about process creation
* A new process is always a child of an existing one (in the unix world)
  * Fork/createprocess
  * Hierarchical structure of processses
* Parent and child have different address spaces
  * A change by one is not visible to the other
  * The child address space is initially a copy of the parent's address space
  * The system call *execve* allows you to change the content (e.g replacing the parent program text with the code of a new program)
* A child process can share the parent's memory
  * Memory is shared copy on write
  * Any change is local

## Process termination
A process can be executed in the following ways
* Normal exit (voluntary)
  * Its work is done :D
* Error exit (voluntary)
  * An error, e.g a wrong input parameter (the process itself handles the error and exits) :/
* Fatal error (involuntary)
  * illegal instruction, referencing nonexisting memory, dividing by zero (big nonos) :(
* Killed by anopther process (involuntary)
  * kill command in the cmd for example D:
## Process states ¡MUI IMPORTANTE!
There are three main states that we will concern ourselves with (there are 8 but who cares)

1. Running
2. Ready
3. Blocked

The allowed transitions are as follows

Ready -> running

Running -> ready

Running -> blocked

Blocked -> ready

## Implementation of processes
All processes's references are stored in a process table. The process table contains the PID(process ID) and a reference to the process control block which stores all kinds of information about the process.

[](https://cdn.discordapp.com/attachments/527886683664941077/1061302023250784256/image.png)

## Threads
* traditional process
  * 1 address space
  * 1 thread of control
* Threads
  * 1 address space
  * n quasi parallel threads of control

explanation: a process in the traditional sense does not parallelise its task whilst threads are a segment of a process being treated as its own process

A process is an instance of a program that is being executed or processed. Thread is a segment of a process or a lightweight process that is managed by the scheduler independently.

A process is an instance of a program whilst a thread can be seen as segments of a process being treated as processes by the scheduler.
## When to use threads

Threads are appropriate for applications where multiple activities are going on at once and each acitivity can block from time to time
    Make programming model simpler
    Allow parallel activities (processes) to share the same address space and modify it

Threads are lightweight, i.e 10-1000 times faster to create and destroy than a process
* which is good when the number of threads change dynamically

They're perfect for tasks that are I/O bound or that balance I/O and computation but offer no advantage for CPU bound programs

If your system has multiple CPUS then real parallelism can be achieved.

## Trivia about threads
### How do blocking/non-blocking system call work with threads contra processes

With threads
* Parallelism using blocking systems call

With single-threaded process
* No parallelism using blocking system calls
* Using non-blocking system calls and interrupts we can achieve parallelism
but the code must be architected like a finite state machine, i.e., the state
of the computation must be saved and restored

### Thread model
In the thread model the process serves as a container for resources. The process groups resources such as address space, open files, pending allarms, signal handlers and child processes

A thread contains private information such as the registers stack and program counter.

###  Threads creation/termination/coordination
* Usually a single-thread process is created
*  More threads in the process are created by the main thread with a library procedure thread_create
*  When a thread has terminated its job, it calls thread_exit
* One thread can wait (block until) the termination of a specific
thread (or a thread in general) calling the thread_join
*  One thread can voluntary give the CPU to another thread by
calling thread_yield

### Thread challanges
* Fork challanges
  * Should the child process have the same threads of the parent? Only one? something in between?
  * Should child threads inherit pending signals and interrupts?
* Files
  * What if a thread closes a file used by another thread?
* Memory
  * what if two threads try to alloocated more memory forthe same data structure?
* ***MULTI-THREAD PROGRAMMING MUST BE DONE PROPERLY AND IT REQUIRES COORDINATION MECHNAISM (next FL)***

## How are theads seen on user-level/kernel-level by threads?
### User level
* OS unaware of threads
* Implementation of threads is easy
* Context switching is fast, no Hardware support needed
* A blocking call performed by a thread blocks the entire process
* examples: posix (pthreads)& java threads

### Kernel level
* OS knows of the threads
* Implementation of threads is not easy
* Context switch is time/resource consuming and require hardware support
* A block call performed by a thread does not block the entire process
* Example: Windows

# FL 3 Interprocess communication (IPC)

## Race conditions
### What is a race condition?
Definition:
Two or more threads/processes in an OS which share some common storage are trying either read or write at the same time. Creatin uncertainty to what result might come out.

Example: Print spooler and two processes
- Place file in spooler increase number
- Two processes A and B
- A reads 7 as an empty slot, store locally, sleep
- B reads 7, store locally, place file in slot 7 in SD and increases counter to 8
- A wakes up, places another file in slot 7 in SD and increases counter to 8
- Problem? File put by B is lost

### What causes race conditions?
Critical regions are the cause of race conditions. Critical regions contain code which accesses the shared memory possibly at the same time in different threads.

### How to avoid race conditions
* Avoid race conditions alltogether
  * Prohibit multiple threads from reading/writing shared data at the same time
* Mutual exclusions
  * If one process is using a shared variable, the other processes will be excluded from doing so


## Mutual exclusion
Prohibiting two processes from entering a critical region does prevent
race conditions, but does not perfectly define mutual exclusion

The conditions that need to be met for mutual exclusion
1. No two processes may be simultaneously inside their critical regions.
2. No assumptions may be made about speeds or the number of CPUs.
3. No process running outside its critical region may block any process.
4. No process should have to wait forever to enter its critical region.

## How is mutual exlcusion achieved?

#### **We can disable interrupts**

Here are some pros and cons:
  * \+ CPU will not be swtiched from one process to another
  * \+ No other processes will intervene when performing critical work
  * \- Unwise to give user processes power to turn off CPU interrrupts
    * \- What if the user does not enable interrupts after the critical region?
  * \- Works good for single-cpu systems, but not for multi-core or multi-CPU systems

This used to be the common solution for old OS, but with the advent of multiple processors it stopped being a thing

example code:
```c
__asm__("CLI"); //disable CPU interrupts
performCriticalWork();
__asm__("CLID"); //enable CPU interrrupts
```

#### **We could use lock variables**
A lock variable is a single shared variable which is either 0 or 1, available or taken

This is not a good solution since the same issue can happen with the lock. What if a race condition occurs with this variable?

#### **We could use strict alternation**
By taking turns in the critical regions we avoid race conditions. This is done through continously testing a variable until some value appears (busy waiting)

A lock that uses busy waiting is called spin lock and consumes CPU cycles

It works but is a lot of overhead
* it can also violate condition #3 if it is incorrectly implemeneted

#### **One alternative is TSL- Test, set and lock**
* Instruction implemented in the hardware
```arm
TSL RX, LOCK // read content of memory word LOCK and copy into register RX and set LOCK to 1
```
RX is indivisible (Atomic)

The CPU locks the memory bus to prohibit others from accessing the mmeory
* Different from disabling interrupts
* Works for multi-cpu systems
* Keeps other processors from accessing the mmemory

But has one main disadvantage which is busy waiting

#### **Semaphores are another solution**
Semaphores are a shared variable between processes, they store wakeups for a particular process
* 0- no wakeups saved
* \>0- wake ups pending

UP increases the semaphore count
Down decreases the semaphore
* If \>0 decrement; If 0 sleep (do not complete the down for the moment)

* Checking the semaphore value, changing it and going to sleep/wakeup are atomic operations (done all together or none)
* If two processes use the same semaphore, pick one (I have no idea what this means, maybe that only one semaphore per process)

#### **A simpler implementation of semaphores are mutexes**
A mutex has two states represented by a single bit: 0 and 1 which means unlocked and locked respectively.

Mutexes are easy and efficient

* mutex_lock() – called when a thread needs access to a critical region
  * Unlocked? enter the critical region: block until the thread in critical region calls mutex_unlock()
  * If multiple threads are blocked on the mutex, one is chosen randomly.
* mutex_unlock() – called when a thread leaves critical region

It's implemented in the user space since it is based off TSL and XCHG instructions

But unlike TSL once it calls mutex lock and the lock is already in use, thread_yield() is called. Eliminating busywaitin

## Message passing
Used for information exchanging
* SEND (Destination, &message)
* receive(source, &message)

Design issues in message passing
* Messages can be lost in network (the solution is acknowledgment of message being received)
* Acknowledgement is lost (solution, retransmit, the receiver should distinguish retransmissions)
* Naming processes (desination, and source) correctly
* Authentication making sure that the client is communicating with the server and not an imposer
* Interprocess communication using message passing within the same machine is slower than semaphores
***
# FL 4 Deadlocks
## Terminology
### Resource
A resource can be anything from a hardware device to a piece of information (examples: printer, memory, database record). Resources are sometimes sharable between processes

Resources can be one of two types:

**Preemptable resources**
* A resource that can be taken from a process without something unexpected happening
* E.g., Memory (1 GB, two 1GB processes want to execute)

**Non-preemptable resources**
* The opposite of preeemptable resources
* Behaviour is undefined, we can't remove it mid execution (i.e., take from a process).
* E.g., Removing a printer from a process mid execution causes half printed pages
* This lecture centers around non-preemptable resources

### Deadlock
A state where two or more processes or systems are indefinitely waiting for a resource that will never arrive. Deadlocks occur when resources are granted with exclusively. For example a process grabs a reesource and sits on it.

The definition of a deadlock is as follows

*"A set of processes is deadlocked if each process in the set is waiting for an event that only another process in the set can cause"*

This event is usually a resource release

Deadlocks are an unrecoverable state where the process must be terminated or else it will and wait for something indefinitely. These errors are expensive and could be rare, making it very hard but crucial to hunt them down.

## Strategies to deal with deadlocks

* Detect and recovery
  * Let them occur, detect them, and act.
* Dynamic avoidance
  * By careful resource allocation.
* Prevention
  * By removing one of the four conditions.
* The ostrich algorithm
  * “stick your head in the sand and pretend there is no problem”
  * Do this when the the likelihood is miniscule and stakes are low :)

## Detection and recovery
By making directed graphs we can detect deadlocks
* A cycle leaves room for deadlocks
* It's very easy since visualising a problem always helps



### There are a couple of methods to recover from a deadlock

**Recovery through preemption - take a resource away**
  * Sometimes this is possible, e.g., take printer away
    * collect all pages
    * printer is assigned to another process
    * put the pile of printed sheets back, continue the process
  * Difficult and sometimes impossible


**Recovery through rollbacks**
  * Checkpoint processes periodically
    * Write processes state in a file so it can be restarted later
  * Detect deadlock, rollback
    * Disregard all the output generated since the last checkpoint; it will be generated again when rolled back
  * This works but is very expensive
   * Resources need to be re-allocated


**Recovery through killing process (several methods of killing will be presented here)**
  * Kill a process in the cycle
    * Repeat until the cycle is broken
  * Kill a process outside the cycle
    * Carefully select the process that is holding resources another process in the cycle needs
  * Killing a process that can be restarted
    * E.g., a compilation process can easily be restarted without any harm
    * E.g., killing a database related process may not be easily restarted

In summary, Detect and recover can occur by causing a rollback, reallocating resources, or killing processes until the cycle is broken

## Avoid deadlocks
* We can transition to two states
  * Safe - deadlocks are not possible
  * unsafe - deadlocks
* Safe
  * If there is some scheduling order in which every process can run to completetion even if all of them suddenly request their maximum number of resources
* Unsafe
  * Is the opposite :(
  * We have three processes but only 2 printers

## Preventing deadlocks (MUI IMPORTANTE)
There are four conditions that need to be met for deadlocks to occur
1. Mutual exclusion - a process can lock other processes from using a resource
2. Hold and wait - a process can hold a resource whilst waiting for another resource to become available
3. no-preemption - resources cannot be reallocated mid process
4. Circular wait - A waits for B to relinquish C whilst B is waitng for A relinquish D

Only when all of these requirements are met, can a deadlock occur. Removing one of these conditions frees us from deadlocks.


So attacking any of these issues in our app hinders deadocks from ever occuring

**Attacking mutual-exclusion**
* Two processes writing to one printer at the same time
  * We have to spool it
    * But what if we allow two processes to spool at the same time?
    * Deadlock
* Avoid assigning a resource unless absolutely necessary.
* Reduce the number of processes that may claim the resource
* Not all resources can be made sharable

**Attacking the hold and wait**
* Acquire all resources and run to completion
  * If one or more resources are busy, do not allocate any resources, and wait.
* Release already allocated resources, if we can’t get all resources
* Problems?
  * Do we know how many resources we need?
  * Are resources used optimally?

**Attacking the no-preemption**
* Not all resources can be preempted
* The ones that can may be tricky

There may be unknown consequences of taking a resource from someone that has allocated it

**Attacking the circular wait**
* Only grabbing one resource at a time
  * Moving a file from disk to printer
* Provide a global numbering for all resources
  * Processes can request any resource but all requests must be made in numerical order
  * E.g, First scquiring the printer, then the plotter, but not vice versa
  * The cycles are gone
* Multiple processes
  * Difficult to order reosurce to satisfy all processes

## Other deadlock related issues

Banker's algorithm is a popular solution to fix deadlocks but comes with flaws

Starvation
* processes wait forever for resources due to bad scheduling policy
* E.g, multiple processes need the printer but the scheduling policy works based on taking on the request with the smalest priorty
  * This maximises happy customers but a constant flow of small prints would make it impossilbe to print any larger file :(
* To avoid this use another scheduling policy such as FIFO
***
# FL 5 Scheduling
## A short introduction to scheduling
A process can be in 3 main states, running; ready; or blocked.
Allowed transitions are as follows
* Running ->Ready
* Ready -> Running
* Running -> Blocked
* Blocked -> Ready

Back in the past CPU time was a scarce resource. Clever and efficient scheduling algorithms had the task of managing CPU time.

Whilst this was not needed for simple pc's with a single user and often not demanding. In networked server on the other hand where multiple processes,often from different users, compete for resources scheduling matters a lot.

Wherever resources are scarce scheduling becomes allthemore relevant (mobile devices for example)

## The objectives of a scheduler
* To select the right process
  * CPU-bound processes
  * I/O bound processes
* To make efficient use of CPU (context/process switch have a cost)
* To make efficient use of other resources, e.g. I/O devices
  * Scheduling of I/O bound processes is more challenging
## When does scheduling make decisions?
If a new process is created,it has the opportunity to choose to run the parent or the child.

A process exit

When a process block on I/O
* Knowledge about process dependencies can help to make a clever and efficient scheduling

When an I/O interrupt occurs

When a clock interrupt occurs
  * HW clock can generate periodic interrupts (CPU CLOCK with X Hz)
  * Scheduling decisions can be made at each interrupt

## Terminology
### Preemptive scheduling algorithms
A preemptive scheduling algorithm picks a process based on some criteria and lets the process run until it either finishes or reaches a maximum of some fixed time. This type of algoirthm relies on a clock interrupt to do a switch, but guarantees that no sole process hogs too much time to itself

### Non-preemptive scheduling algorithms
A non-preemptive scheduling algorith also picks a process based on some criteria and then let's the process run until it either blocks or completes. This does not require a clock interrupt but can run into the risk of unfairly dividing cpu time.

### Dispatcher
The dispatcher module gives control of the CPU to the process selected by the CPU scheduler. A process has the following rights:
* switching context
* Switiching to user mode
* Jumping to the proper location in the user program to restart that program

Dispath latency is the time it takes for the dispatcher to stop one process and start another.
### Burst
A burst in scheduling context is a processes "time" in the sun so to say. It is the time when a process is running. A CPU burst is thus the time where a process is active.
### Turnaround time
The time taken to complete a process (from initial start to finish)

### Aging
Is a scheduling technique to avoid starvation. In essence process are assigned a higher priority in accordance with the time they've spent as ready.
### Runtime
In computer programming, a runtime system or runtime environment is a sub-system that exists both in the computer where a program is created, as well as in the computers where the program is intended to be run.
## Categories of scheduling (Should add more info)
Different scheduling algorithms exists for different ends. The following categories are those ends.
 ### Batch
 * Non-preemptive scheduling
 * preemptive scheduling with long time periods to reduce process switches
 * The goal is to get work done
### Interactive
* Preemptive scheduling is needed to processes starvation and poor perceived performance.
  * Starvation of certain process is perceived as lag on the end user which a good scheduling algorithm should not cause.
### Real time
* Preemption is sometimes not needed
* Small processes doing their work in a very short time

## Scheduling algorithm goals
### All systems
* Fairness - giving each process a fair share of the CPU
* Policy enforcement - seeing that stated policy is carrie out
* Balance - keeping all parts of the system busy (under utilised resources is a nono)
### Batch systems
* Throughput - maximise jobs per hour
* Turnaround time - minimise time between submission and termination (A process shouldn't have to stand ready unnecessarily)
* CPU utilisation - keep the CPU busy all the time

### Interative systems
* Response time - respond to requests quickly
* Proportioanlity - meet user's expecatations
### Real-time systems
* meeting deadlines - avoid losing data
* Predicability - avoid quality degradation in multimedia systems (idk)

## Scheduling in batchh systems
### First-come, first-served (FCFS)
* Non-preemptive
* Non optimal for a mix of CPU-bound and I/O bound processes. Preemption can improve performance when in such a situation
### Shorest Job first (SJF)
* Associate each process with the length of its next CPU burst (how long until it gets blocked or completes)
  * Use these lengths to schedule the process with the shortest time
* SJF is optimal - gives minimum average waiting time for a given set of the processes
* Quite hard to know th length of the next CPU request
### Shortest remaining time first (SRTF)
  * Preemptive version of SJF
  * Selects the process whose remaining time is shortest
  * Requires a knowledge about total execusion time of each arriving process

## Determining Length of next CPU burst
If you can only estimate CPU burst but you know that it is usually similar to the previous burst
* Then pick process with shortest predicted next CPU burst

Can be  done by using the length of previous CPU bursts, using exponential averaging

***Terminology***
1. $t_n$ = actual length of n:th CPU burst
2. $\chi_{n+1}$ = predicted value for the next CPU burst
3. $\alpha$, 0 $\underline{<}\alpha \underline{<}1$

The formula is as follows:
$$t_{n+1} = \alpha t_n+(1-\alpha)\chi _ n$$

$\alpha$ is usually 0.5

## Scheduling in interactive systems
### Round-robin (RR)
It is a Preemptive algorithm. Each process runs for a quantum (a timelimit), then it is put back in the ready queue. The queue works on FIFO principle
* Setting the appropriate quanum to is important toimprove cpu efficiency
  * Quantum = 4ms; context switch = 1ms $\rightarrow$ 20% of CPU time as overhead
  * Quantum 100 ms; context switch = 1ms $\rightarrow$ 1% of CPU time as overhead
  * Time quantum is usualy between 10-100ms

If there are n processes in the ready queue and the time quantum is q, then each process gets 1/n of the CPU time in chunks of at most q time units at once.
  * no process waits more than (n-1)q time units
* Has a higher turnaround than SJF but beter response
### Priority scheduling (PRI)
* Each process is assigned a priority (statically or dynamically) and the runnable process with highest priority is selected
* To prevent starvation the priorty of the running process can be decreased at each clock interrupt
* Dynamic priority assignmen for I/O bound process
  * Pri = 1/f; f = fraction of the quantum the process used
  * e.g f=50ms $\rightarrow$ f=1/50 $\rightarrow$ priority = 50 (higher is lower priority in this context)
* Possible to group orcesses into priority classess (high, medium,low) and use a different algorith inside the classes
  * RR in the class
  * PRI among the classes for example
### Shortest process next
The runnable process with the shortest estimated running time is select
* We need to estimate the next running time using the past behavior. Aging could be used if $T_0$ is the estimated running time and $T_1$ is the measured running time
  * $T_0 = aT_0+(1-a)T_1$
  * I have no idea if this is correct
### Guaranteed scheduling
The scheduling algorithm guarantees a real measurable promise, e.g.
  * If n process are running all will receive 1/n of the CPU cycles
  * if n users are logged in each will receive 1/n of the CPU power

Need to measure how much CPU a process used since its creation
  * R= actual CPU time consumed / CPU time entitled
  * Select the runnable process with the lowest value of R
### Lottery scheduling
Processes are assigned with a ticket, at scheduling decision, a ticket is chosen randomly. Some processes can get more tickets increasing the probability (in the long run) of getting more CPU time

  * Cooperative processes may exchange tickets if needed


## Multilevel feedback queue
In essence a multilevel feedback queue is scheduling but the processes are in different queues which work according to different principles and different time quantums. This could be an incorrect assessment, below you'll find the slides' descriptions:
* A process can move the various queues
* Multilevel-feebackqueue scheduler is defined by the following parameters
  * Number of queues
  * Scheduling algorithms for each queue
  * Method used to determine when to upgrade a process
  * Method used to determine when to demote a process
  * Method used to determine which queue a process will enter when that process needs service
* Aging can be implemented using multilevel feedback queue
## Thread Scheduling
I decided to include complementary information from Javapoint as the slides did deteriorated in quality significantly here for some reason

Link: https://www.javatpoint.com/user-level-vs-kernel-level-threads-in-operating-system
### User level threads
**Description**
The User-level threads are small and faster as compared to kernel-level threads, and the OS directly supports user-level threads. Users implement the user-level threads, and the kernel is unaware of their existence and handles them as though they are single-threaded processes.

It is also known as the many-to-one mapping thread, as the OS assigns every thread in a multithreaded program to an execution context. Every multithreaded process is treated as a single execution unit by the OS.

**Advantages**

* User level threads are simpler and faster to generate. They are also easier to manage.
* Thread switching in user-level threads doesn't need kernel mode privileges.
* These are more portable.
* These threads may be run on any OS.

**Disadvantages**

* The complete process is blocked if a user-level thread runs a blocking operation.
* User-level threads don't support system-wide scheduling priorities.
* It is not appropriate for a multiprocessor system.


**The slides had this to say:**
  * the runtime implement its own scheduling algorithm. RR and PRI are the most common
  * No clock interrupts exist since all threads are cooperative.
  * Thread switch is inexpensive
  * If a thread blocks for I/O the process blocks
### Kernel level threads
**Description**

In Kernel Level Thread, the kernel handles all thread management. Every process doesn't have a thread table, but the kernel has one that maintains track of all of the threads in the system. If a thread wishes to make a new thread or stop an existing one, it initiates a kernel call that performs the work.

The kernel-level threads table contains each thread's registers, status, and other information. The data is identical to that of user-level threads, except it is now in kernel space rather than user space.

**Advantages**

* If a thread in the kernel is blocked, it does not block all other threads in the same process.
* Several threads of the same process might be scheduled on different CPUs in kernel-level threading.
* Kernel routines can be multithreaded as well.

**Disadvantages**

* Compared to user-level threads, kernel-level threads take longer to create and maintain.
* A mode switch to kernel mode is important to transfer control from one thread in a process to another.

**The slides said this about kernel level threads**
* The scheduler can select any thread; it can
eventually consider the process the thread
belong
* Thread switch is expensive like process switch
* If a thread blocks for I/O the process doesn’t

# FL 6 Memory management
## Terminology
### Volatility
A volatile system does not store information once it has been turned off or crashed. A non-volatile system (think of your SSD or Harddrive) stores information even without power coursing through it.
### Compaction
Collecting  fragments into one large chunk of something
## The basics
The main memory (aka RAM) is an important resource in a computer system. We would like to have an infinitely large, fast, nonvolatile and cheap memory, but this is impossible so instead we settle for a memory hierarchy.


A memory hierarchy consists of:
- Very fast, small , expensive volatile cache memory
- Medium fast, medium size volatile main memory
- slow, large, nonvolatile disk storage

The part of the operating system that manages the memory is naturally called the memory manager. A memory mananger has the job of using this hierarchy to create an abstraction (illusion) of easily accessible memory

### **What would happen if we had no memory manager?**
The simples solution is to use the phyuscial main memory directly
  - OS reads program in from disk and it is executed

The problem with this approach is that it is not possible to run several programs at the same time then. By swapping out one program entirely and swapping in another program one could do some redimentary time slicing (timeslicing is roundrobin scheduling).

### **Programs grow as they execute**
Most programs create local variables and such which eventually are meant to store information. Data is also allocated dynmamically and released. A good idea would be to allocate extra memory when a program is moved to the main memory.

The stack can grow downwards and data grows uppwards towards eachother.

## Address space abstraction
In order to have more than one program in main memory at the same time we need to solve two problems:
- Protection, .i.e., the content of one program should be protected from manipulation by another program.
- Relocation, i.e., the program must be able to run even if it does not start at physical address 0 in the main memory

Each program runs in an operating system process and each process sees an addresss space that starts from address 0. One simple solution that achieves unique and non-overlapping address spaces for the processes is the use of a base and limit registers.
- Base describes where the program starts in the main memory. If the program wants to access address x, you simply read from x+base
- The limit tells you the length of your program. limit+base is the last address space

### **Problem with relocation**
Static relocation  - load first instruction of program at address x and add x to every subsequent address during loading. This is slow and nicht gut since all addresses cannot be modified

### **Base and limit registers**
- A form of dynamic relocation
  - Base contains beginning address of program
  - Limit contains length of program
- Program references memory:
  - Adds base address to address generated by process
  - checks to see if address is larger than limit. If so generates a fault
- This has the disadvantage of requiring addition and a comparison on each instruction :(
## What if the memory is not large enough to hold all processes?
A PC can easily have hundres of processes running simultaneously. There are two main strategies for handling the problem with small physical memory
- **Swapping**, move process in its entirety between the main memory and the disk
- **Virtual memory**, move parts of a program's address space between and the disk

## Swapping
Swapping may require memory compaction which takes a lot of time
If we only use swapping a process needs to be allocated in consecutive physical main memory (Physical main memory which is connected to each other, it's contiguous)

This would leave holes in the main memory which increases fragmentation.

### **Managing free memory**
The operating system must keep track of unused (free) memory.
There are two main ways to do this, Bitmaps and linked lists. A tradeoff between computation time and efficacy is made by using some minimum allocation unit. Instead of working with bits it works with chunks of bytes.
### **Linked list**
A process normally has two neighboring areas, one before the process and one after the process in the main memory. These areas may either be occupied or free

This creates four different cases when updating the link list with free memory.
- A process has neighbours on both sides and is terminated -> new hole
- A process only has no neighbour before but one after and is terminated -> No new hole, the before hole expands
- Reverse situation as before -> the after hole expands
- No neighbours process terminated -> two holes are merged into one

A linked list in this context would looks like this

\[H,18,2\]->\[P,20,6\]-> \[P,26,3\]->...


Where the first slot says whether that chunk of memory is a process or a free memory (hole).the second slot is the start of said chunk and the last element is its length.

### **Selecting free memory space for a new process**
When we search the linked list for a memory slot for a new process we could use different approaches
- First fit, select the first hole that is big enough - fast but maybe inefficient
- Next fit, the same as first fit but we start the search from that position of the linked list where we ended the last search
- Best fit, search the entire list and select the smallest hole that is big enough - slow, may create many small holes
- Quick fit, maintain seperate lists with common memory sizes
### **Bitmaps**
* If we have a small minimum allocation unit, the bitmap apporach will require a lot of overhead.

* If we have a large minimum allocation unit, we may waste memory since not all units will be completely filled.

A bit map is a compact way to keep track of memory but requires us to search for k consecutive zeros to bring a file of size k into the memory. We ourselves can choose the size of the units.

A bitmap would look like this

[1,0,1,1,1,0,1,1]

[1,1,1,1,1,1,0,0]

Where 1 means occupied, 0 means free

## Virtual memory
There are several problem with only using base and limited registers as swapping

* Some very large programs may not fit in physical memory
* Some programs only use a small part of their address space most of the time
* it is time consuming to move an entire process in and out of main memory
* We may end up with memory fragmentation in the form of many (too) small holes of memory. Compaction can solve this but this is a very time-consuming operation

An early solution to some of these problems was to split a program into several overlays. But this this hard to do for programmers in general


### **What is virtual memory?**
Virtual memory means that the program is stored in multiple pages which have a fixed size. One segment of a program can be in page A whilst the other part is in page Z. To make this seem continous for the program a MMU (Memory management unit) is used.

These are the steps described in the slides

1. Program's address space is broken up into fixed size pages
2. Pages are mapped to physical memory.
3. If instruction refers to a page in memory, fine
4. Otherwise, OS gets the page, reads it in and restarts the instruction
5. While page is being read in another process gets the CPU
6. Memory management Unit (MMU) generates physical address from virtual address provided by the program

### **Virtual pages and physical page frames**
* virtual addresses are divided into pages
* 512 bytes - 64KB
* Transfer between RAM and disk is in whole pages
* Example:
  * 16 bit addresses => 64 KB virtual memory space
  * 4 KB pages => 16 virtual pages
  * 32 KB physical memory => 8 pageframes.

The virtual pages are programs which we fit into the pageframe

Mapping between virtual pages and physical page frames is done by a pagetable

If we're trying to read a page which is not in the physical pageframe then a pagefault occurs.

### **Page fault processing**
Present/absent bit tells whether page is in memory

If the address is not in memory?
* Send a trap interrupt to the OS
* OS picks page to write from disk
* Brings page with needed address into memory
* Restart the instruction

### **Page table and MMU operation**
Virtual address = {virtual page number, offset}

Physical addresss = {page frame number, offset}

Virtual page number used to index into page table to find page frame number

If present bit is set to 1, attach page frame number to the front of the offset, creating the physical address which is sent on the memory bus.

### **Challenges for paging**
Virtual to physical mapping is done on every memory reference which requires the mapping to be really fast

If the virtual address space is large, the page table will be large
- Most processors support 64 bit virtual addresses

### **Translation lookaside buffers (TLBs)

* ost programs access a small number of pages most of the time
* add translation look aside buffer TLB to MMU which stores translations of frequently accesses pages

The TLB acts like a cache for the page table

If address is in MMU avoid page table
- Uses parallel search to see if virtual page is in TLB
- If not does page table look up and evicts tlb entry replacing it with page just looked up

Has traditionally beeen handled entirely in hardware by the MMU but the TLB is now often maintained in software i.e., the mmu throws a software TLB miss to tohe operating system

This simplifies the TLB and frees up spaces on the cpu chip

# FL 7 Memory management
## Terminology
### R and M bits
Two bits used to check if a page has been read or modified .
### Working set
All pages that a process needs to run

### Demand paging
bring a process into memory by trying to execute the first instruction and getting page fault
* Continue until all pages that the process needs to run are in memory

### Thrashing
Memory is too small to contain the working set so page faults all of the time :(

### Pinning
Locking a page into the memory for a certain duration to avoid it being swapped out whilst we're doing something with it.

## Page replacement
When there is a reference to a page that is not in the physical main memory, we get a page fault. When a pagefault occurs, the operating system moves a page out of the main memory (replace/evict) to make room for the page that we need to bring in from disk

Thepage that is evicted is written to disk if the M bit is set i.e., if the page has been modified since it was brough in from disk last time
* Otherwise a good copy exists on disk so we just overwrite the page frame with the new page

The performance by which page we select to replace

Page replacement use the R and M bits

## Page replacement algorithms

### **Optimal Page replacement algorith**
* Pick the one which will not be used for the longest time
* Not possible unless we know when pages will be referenced (crystal ball)
* Used as ideal reference algorithm

### **Not recently used NRU page replacement**
Use R and M bits
* periodically clear R bit
* Class 0: not referenced, not modified
* Class 1: not referenced, modified
* Class 2: referenced not modified
* CLass 3: referenced, modified

Highest priority 3, lowest is 0

Pick the lowest priority page to evict. Random selection in the class when a tie exists

### **First-in First-out (FIFO)**
Keep list ordered by time (latest to arrive at the end of the list) and evict the oldest every time. This method is really easy to implement and the oldest method.

FIFO does not account for popularity of a page so we might evict a highly read page.

### **Second chance page replacement algorithm**
It is very similar to FIFO but popularity is taken into account. Searches for oldest non-read file. Every file that it finds which is old and has been read is put at the ned of the list.

This might still evict a heavily used page but the chance is lower.

### **Clock page replacement**
Does not use age as a reason to evict pages. A handle points to a page, if the page R bit is 1 then it moves to the next. If the page's R  bit is 0 then that is evicted.

This is strictly faster than 2nd chance since no list manipulation occurs.

### **Least recently used (LRU)**
Assume that recent page usage approximates long term page usage, i.e, a page which hasn't been used recently will probably not be used soon.

Could associate counters with each page and examine them but this is is expensive
* Associate counter with each page
* At each reference increment counter
* Evict page with lowest counter

You could alternatively use a nxn array for n pages. Upon refrence page, k put 1's in row k and 0's in column k.
* Row with smallest binary value corresponds with lru page to evict.

### **Working set model**
The working set model uses demand pageing and tries to make sure that the working set is is in the mmeory before letting a process run

When a fault occurs the page replacement algorithm tries to evict a page that is not in the working set, i.e, not needed for proper execution (if it exists).


The stuff below is about implementation and i don't understand it :(


We could keep track of pages in memory at every memory reference
* Each k references results in a working set
* This is expensive

Using virtual time isntead of number of references is possibly better
* keep track of k last pages referenced during a aeriod of t of execution time
* Virtual time for a process is the amount of CPU time used since it started
  * measure how much work a process has done

#### **Working set pagereplacement algorithm**
On every page fault the following is done:
* The R Bit for each entry is examined
* If  R == 1 the current virtual time is written itn ot the Time of last use field in the table
  * If the time since last use > length of the WS time limit==> Evict the page and exit
  * If time since last use < Length of the WS time limit => Continue but keep track of the page with longest time since last use
* If no page could be evicted, evicted the page with longest time since last use
* Disadvantage: need to scan the whole page table at each page fault.

### **WSClock page replacement algorithm**
A combination of the clock algorithm and working set algorithm

The working set algorithm is problematic since the entire page table needsto be scanned at each page fault. An improved algorithm called WSCLock combines ideas from the WS and Clock algorithms

The main data structure  is a circular list of page frames
* If R==1 then R== 0 and we move to the next page
* If R== 0 and M ===0 the new page is loaded into the page frame
* If R==0 and M==1 the old page is written to disk and we move to the next the next page (when the write is finished the M bit will be cleared ofc)

If the hand comes all the way around to its starting point there are two cases to consider
* at least one write has been scheduled
  * Hand keeps moving looking for clean page. Finds it because a write eventually completes - evicts first clean page hand comes to
* No writes have been scheduled
  * Evict first clean page (we set every page to clean)


### **Short summary of all page replacement policies**

* Optimal is not implementable but a useful benchmark

* NRU is a crude approximation of LRU

* FIFO can throw out important pages

* Second chance is a big improvement over fifo but still runs the risk of doing stupid stuff

* Clock is okay

* LRU excelelnt but difficult to implmement use

* Working set somewhat expensive to implement
* WSCLOCK good efficient algorithm

* Any aging implementation is a good approximation of LRU

## Local versus global allocation policies

Page replacement can either be:

* **Global**, i.e., we can replace any page irrespectively of which process it belongs to
* **Local** i.e., we will only replace a page belonging to the process that caused the page fault

In general, global algorithms work better, especially when the working set sizes of the process varies with time. Allocating too few page frames to each process results in thrashing and allocating too many wastes memory. Global page replacement allows us to be a bit more flexible. Working sets grown and shrink over time, processes have different sizes

You usually assign pages to each process prortional to its size and use pagefault frequency to modify allocation size for each process.

### **Load control**
When all processes want to increase the number of page frames to hold their working set we need to swap out a process entirely and divide pageframes allocated to that process to other processes. By doing this we can avoid thrashing

The number of page faults is not the only aspect that nees to be considered when deciding how many processes to keep in the main memory. If we have multiple CPU:s we also need to keep enough processes in main memmory so that the CPU:s are fully utilized.

### **Page size**
Finding the optimal page size is not trivial
* Large pages may result in large internal fragmentation
* Small pages result in large page tables which is overhead

## Shared pages
Different users can run the same program with different data at the same time. Better to share pages than to have 2 copies
. Not all pages can be shared (data can't be shared, text can be shared though). If have data + instruction spaces/segments, we can have process entry in the process table to point to i and d pages

### Page sharing issues
A process can't drop pages without when it exits w/o being certain that they are not still in use
* Use a special data structure to track shared pages
Datasharing is painful (e.g., unix fork, parent and child share text and data) because of page writes
* Copy on write solution is to map data to read only pages if write occurs each process gets its own page

### Shared libraries
Large libraries are often used by many processes, it would be too expensive to bind each process which wants to use them. Shared libraries solve that problem

Unix linking: Id *.o -lc -lm. Files not present in *.o are located in m or c libraries included in binaries
* Linker uses a sub routine to call which binds to called function at run time
* Shared librariy is only loaded once (first time that a function in it is referenced)

This requires us to use position independent code to avoid going to the wrong address
* The solution to this problem is that the compiler does not produce absolute addresses when using shared libraries; only relative addresses.

This lecture is extremely long wtf

### Memory mapped files
* Processs issues system call to map a file onto a part of its virtual address space
* Can be used to communicate via shared memory. Processes share same file. Use it to read and write

## Paging daemon and cleaning policy
Paging works best when there are plenty of free pages

To make sure that there are empty page frames when a page fault occurs, there is a background process called pageing daemon

The paging daemon wakes up periodically and frees page frames based on some page replacement algorithm

If the selected pages are modified (dirty) the paging daemon writes the page to disk.
## Page fault handling MUI IMPORTANTE
1. The hardware traps to the kernel, saving the program counter on the  stack.
1. An assembly code routine is started to save the general registers and other volatile information.
1. The operating system discovers that a page fault has occurred, and tries to discover which virtual page is needed.
1. Once the virtual address that caused the fault is known, the system checks to see if this address is valid and the protection consistent with  the access
1. If the page frame selected is dirty, the page is scheduled for transfer to the disk, and a context switch takes place.
1. When page frame is clean, operating system looks up the disk address where the needed page is, schedules a disk operation to bring it in.
1. When disk interrupt indicates page has arrived, page tables are  updated to reflect position, frame marked as being in normal state1.
1. Faulting instruction backed up to state it had when it began and program counter reset to point to that instruction.
1. Faulting process scheduled, operating system returns to the (assembly language) routine that called it.
1. This routine reloads registers and other state information and returns to user space to continue execution, as if no fault had occurred.

## Storing pages on disk
*  Most operating systems have a special swap partition (or swap disk) for storing pages.
*  The swap partition does not have a normal file system, it used disk blocks directly.
*  You can however use a special swap file, and then the swap area is handled by the normal file system. This is sometimes a bit slower.
*  Each process has a swap area on the disk
*  The swap area can be static or dynamic (or a hybrid)

### **Static paritition**
Allocated a fixed partition to process when it starts up

Manage as list of free chunks. Assign big enough chunk to hold entire process. Startin address of partition kept in process table. Page offset in virtual address space corresponds to address on disk

Can assign different areas for data, text, and stack, as data and stack can grow

### **Dynamic Partition**
* Don't allocate disk space in advance
* Swap pages in and out as needed
* Need disk map in memory to know where they end up

## Seperation of policy and mechanism
The memory management system is divided into three parts:
* A low level MMU handler (depends on your machine)
* A page fault handler that is part of the kernel (machine independent). Asks MMU to assign space for incoming page in the process
* An external pager running in user space which contains the policy for page replacement and asks/receives pages from disk.
## Segmentation
A compiler has many tables that are built up as compilation proceeds, possibly including
* The source text being saved for the printed listing
* The symbol tables- the names and attributes of variables
* The table containing integer,floating-point contants used
* The parse tree the syntatctic analysis of the program
* The stack used for procedure calls within the compiler

A memory devided into segments allows each table to grow or shrink independtly of the other tables

This simplifies handling of data structures which are growing and shrinking
* Address space of segment n is of form (n, local address) where (n,0) is the starting address

We can compile segments seperately from other segments

Can put library in a segment and share it

Can have different protections for different segmentations

Paging and segmentation are differnt things

### Paging vs Segmentation
* Paging does not require the programmer to be aware of the fact that this technique is being used whilst the opposite is true for segmentation
* Paging has one linear address whilst segmentation has many
* Both allow for virtual memory
* Segmentation allows for procedures and data to be distinguished and seperately protected whilst paging does not
* Size fluctuations in tables are more easily accomodated with segmentation

Segmentation was invented to allow programs and data to be broken up into logically independent address spaces and to aid sharing and protection whilst paging was invented to get a large linear address space without having to buy more physical memory

Segmentation leads to extrernal fragmentation over time which requires compaction to solve.

Segmentation with paging is called the pentium


# Läs boken på föregående kapitel, det är otroligt mycket

# FL 8 File systems
## File systems motivation
Requirements for long-term storage are as follows:
* It must be possible to store a very large amount of information
* The information must survive the termination of process using it (persistence)
* Multiple processes must be able to access the information concurreently

Think of a disk as linear sequences of fixed-size blocks and supporting reading and writing of blocks

Many questions arise quickly:
* How do you find information
* How do you keep one user from reading another's data?
* How do you know which blocks are free?

These questions are solved by introducing an abstraction in the form of files

## Files
Files can be seen as container of data blocks on a disk. The user will see a file, while the os will see a collection of blocks spread out over the disk.

This file will be identified by its
* Naming
* Type
* structure
* access
* attributes
* operations
* decriptors
* directories

### **File naming**
Different OS:es have different naming conventions
* Some set limits on lengths
* Others on special characters

File extensions are used to describe what the purpose of file the file is.
* The extension is always a part of the file's name
* Certain OS:es enforce meaning based on the file's extensions
  * WINDOWS does
  * Unix does not

### **File structure**
A file can be structured in many ways
* You could structure it as a stream of bytes which the user has to figure out
* You could structure the bytes in records or keys

The most common method is to use raw byte streams and allow the user to figure it out.

### **File types**
There are two file types (three actually since I/O devices are counted as files)

Regular files
* Ascii files: plain text files, printable, can be edited with a text editor
* Binary files: executables, archives
  * Each os has to recognize its own executable format

Directories
* System files for maintaining the file system structure
* Does not reflect the structre on disk

Special files
* Character special files: model serial I/O devices, e.g., terminals printers
* Block special files: model block devices such as disks

### File access
There are two access types

sequential access
* Read from the beginning, can't skip around
* Corresponds to magnetic tape

Random access
* Start where you want to start
* Came into play with disks
* Necessary for many applications such as database systems

### **File attributes**
* Name - only information kept in human-readable form
* Identifier - unique tag that indetifies file within file system
* Type - needed for systems that support different types
* Location - ppointer to file location on device
* Size - current file size
* Protection - controls who can do reading writing executiong
* Time (creation, modification, access), date, and user identification
  * Data for protection security and usage monitoring
* Information about file are kept in the direcotry structure which is maintained on the disk
* Many variations including extended file attributes such as file checksum (my understanding of checksum from playing paradox games is that this checks wether the file has been modified in a manner which wasn't official) also
* Information kept in the directory structure
## File operations
* Create - with no data, sets some attributes
* Delete - remove the file to free disk space
* Open - after create, gets attributes and disk addresses into main memory, create entry in file descriptor table
* Close - frees table space used by attributes and addresses
* Get attributes - e.g., get most recent  modification times to arrange for group compilation
* Set attributes - e.g., protection attributes


* Read - usually from current read pointer position
  * Need to specify buffer into which data is placed

* Write - usually current write position
  - Need to specifiy buffer where the data to be written is placed

* Append - add content at the end of the file

* Seek - puts file pointer at a specific place in file
  * Read or write from that position onwards.

Example of a simulated driver:
```cpp
void readblockfromdisk(uint32_t blockNr, uint8_t* buffer)
{
std::fseek(fp, 0, blockNr * 4096);
std::fread(buffer, sizeof(uint8_t), 4096, fp);
}
void writeblocktodisk(uint32_t blockNr, uint8_t* buffer)
{
std::fseek(fp, 0, blockNr * 4096);
std::fwrite(buffer, sizeof(uint8_t), 4096, fp);
}
```

## Datastructures for file control - File descriptors

OS maintains a file control block (FCB)for each file, containing e.g.,
* File permissions
* File dates
* File owner
* file size

In-memory file system structures
* System-wide open-file table contains a copy of the FCB of each file and other info
*  Per-process open-file table contains pointers to appropriate entries in system-wide open-file table as well as other info

## Directories
Files that used to organize a collection of files. Two types:

Flat / Sing level directory
- One directory
- Simplest
- Has naming and grouping problems
- Think of a file system where you only have the root folder

Hierarchical directories
* Subdirectories (directories within directories)
  * Tree structure
  * Grouping
  * No naming problems

### **File and path names**
Each entry in a directory is a name and a pointer. We reserve a character to use as a seperator (\\ or / )

A path is defined as a sequence of components to a target. There are two types of paths
* Absolute paths
  * The path starts from the root
* Relative paths
  * All references are relative to the current working directory

Each processs has its own current working directory which can be changed during runtime

Two special directories exists
* '.' refers to the current working directory
* '..' refers to the parent directory of our current directory

### **Directory operations**
 * Create
 * Delete
 * Opendir
 * Closedir
 * Readdir
 * Rename
 * Link
 * Unlink

## Disklayout
Disks are logically divided into one or more partitions with seperate file systems on each system.

Disk sector 0 is the master boot record (MBR) and also contains the partition table

At boot time we read the MBR and look up which partitions that contain a bootable operating system with the partition table

### Partition layout
The first block in a partition is the boot block (even if there is no OS in the partition)

This is the order of parts in a partition:
1. Boot block, The program in the boot block locates the OS and loads/starts it
2. super block, contains key administrative parameters about the file system (number of blocks, which magic numbers mean what etc)
3. free space management, which keeps track of unused disk blocks
4. i-nodes, Administration of which diskblocks belong to which files e.g., i-nodes, FAT.
5. rootdir, the top of the directory hierarchy, sometimes located in a fixed disk block
6. files and directories, diskblocks for storing data. This is the largest portion of the partition

## Filesystem implementation (Probably important)
This part will go through 4 example approaches of implementing a filesystem and keeping track of which block belongs to which file..

### **Contiguous allocation**

**Idea**

Use consecutive blocks for each file

**The good**
* Easy to implement
* Great read performance :)

**The bad**
* Disks become fragmentaged over time

**Usage**
* CDROM / DVD - large known chunks of data with known size which are written once

### **Linked list allocation**
**Idea**

Each diskblock has a pointer to the new block of the file

**THe good**
* Gets rid of fragmentation

**The bad**
* random access is slow (we need to traverse the list)
* Data size in blocks is not a power of 2 (reduced space due to pointer)

### **Linked list using table (FAT)**
Basic idea: put all link pointers at the same place (in a table)

The good
* Gets rid of fragmentation
* Datablocks are now a power of 2
* Cache table in ram - faster random access

The bad
* table is proportion of disk-size (uses lots of ram as we have a lot of disk space but not a lot of ram in comparison)
* Eg., 200kb disk with 1 kb blocks needs a 600 mb table

Usage: Win 95

### **I-nodes**
Basic idea: use a small table of pointers for each file

The good
* Keep data strucutre in memory only for active files - reduces memoryu pressure
* K active files, n blocks per file => K*N blocks max (WHICH IS GOOD)

hOW BIG IS n
* sOLUTION LAST ENTRY IN TABLE POINTS TO DISK BLOCK WHICH CONTAINS POINTERS TO OTHER DISK BLOCKS


The bad

* More complex to implement

Used in linux :)

# FL 8 Memory systems part 2 D:
## Terminology
### Idempotence
An instruction that when executed multiple times will always do the same thing
  * Calling the delete on file1 will always free the same blocks and nothing else
## Performance aspects
* best method depends on file access type
  * Contiguous is great for seqquential and random
  * Linked is good for sequential but not random
* Declare access type at creation
  * Select either contiguous or inket

* indexed is more complex
  * Single block access could require 2 index block reads then data block read
  * Clustering can help improve throughput, reduce CPU overhead
* For NVM/SSD, no disk head exists so different algorithms and optimisations are needed
  * Using old algorithm uses many CPU cycles trying to avoid non existent head movement
  * Goal is to reduce CPU cycles and overall path needed for I/O

## Implementing directories
Path name could be used to locate directory

Logical file containing other files

Map the ASCII name to block by providing
* Address of first block (contiguous)
* Number of first block (linked)
* I-node number

Where do you store attributes?
We can either hold the attributes inside the directory (which is not flexible for some reason) or store it in the i-nodes - very flexible.

Variable name lengths can be fixed through either of these two approaches
- Have a fixed header followed by variable length names
- Heap-pointer points to names

### **Shared files and links**
There are many reason for making the same file appear in multiple locations
* Sharing between users
* Configuration data
* Shared repositories

There are two approaches (as always)
* hard links: alias the same i-node into two directories
* soft links: record the path to the target (also called a symbolic link)

## Log structured file system
CPU's are faster, disks and memories have exploded in size but disk seeking time has not decreased.
* Caches are bigger - allows us to do reads from cache
* Writes are often small, we want to optimize writes because disk needs to be updated
* Structure disk as a log, i.e., collect writes and periodically send them to a segment in the disk
* Segment has a summary of contents (i-nodes, directories)
* Keep i-node map on disk and cache it in memory to located i-nodes
* Cleaner thread compacts log. Scans segment for current i-nodes,  discarding onesnot in use and sending current ones to memory
* Writer thread writes current ones to memory
* Writer thread writes current ones out into new segment
* Needs a re-write of the file sstems - compability issues

## Journaling file systems
Want to guard against lost files when there are crashes such as a power loss. Consider what happens when a file has to be removed.

* remove the file from its directory
* Release the i-node to the pool of free i-nodes
* Return all the disk blocks to the pool of free disk blocks

If there is a crash somewhere in this process we have a big mess :(

The solution is to keep a journal of actions before you take them, write the journal to disk then perform your actions. This allows us to recover from a crash

We have to make operations idempotent then to avoid issues if a crash occurs and we have to redo some instructions. Data structures must be arranged to do so
* Marking a block n as free is an idempotent operation (can be done many times with same result)
* Adding freed blocks to the end of a list is not idempotent
* Windows uses journaling

### **Virtual file systems - Multiple file systems on the same machines**
Lets say that we have a USB running FAT32 a DVD and NTFS (windows filesystem) for the OS

We could either integrate these or create an interface which processes have to figure out.

Windows specifies FS whilst Unix integrates the VFS

### Virtual file systems - how does it work?
The filesystem registers with VFS (e.g., at boot time or insertion)

At registration time, FS provides list of addresses of function calls that the VFS wants.

VFSD gets info rom the new FS i-node and puts in a v-node

Makes entry in file descriptor table for processes.

When process issues a call (e.g., read) function pointers point to concrete function calls

***Explanation***: The virtual file system in essence stores pointers to functions instead of functions. When a file is going to be read or something from either file system, the appropriate pointer is used to read the said file. A VFS is a collection of filesystems (a filesystem interface of sorts)


### Disk space management (a short summary)
Block size is no easy thing to manage. THe optimum is not a non-tirivial matter which depends on file size ditribution.

In general, a large block size leads to wasted space but faster reading
whilst the opposite is true for small blocksizes.

fig 4-20 in the book has smth good apparently

### Keeping track of free blocks
The same method exists for memory as does blocks

Linked lists and bitmaps are the premier alternatives.

Linked lists have major problems
* I/O can destroy performance
  - Overhead
* Keep a block of pointers to free blocks in RAM
  - Reading from disk would be too slow, cache all
- Example, space for 2 free pointers in RAM
  - 3 blocks are freed
  - Write block to disk with 2 blocks, store 1 block in RAM
  - Create new file, 3 blocks, fetch block of pointers from disk
  - File was temporary, release 3 blocks
- Solve this by splitting blocks

## Disk quotas exist
In a multi user environment disk space can be scarce. The need to limit resources for each user might exist

Increase of a file's size are charged/counted towards the user's quoata

Soft vs hard limits
* Soft limits are often checked at login, may be exceeded
* When reaching the hard limit, new additions generate errors

## Filesystem backups
Hardware is replaceable but data is not. Back ups are there to recover data from stupidity and or disaster. Backups are expensive though
* What should we backup
* When should we do it
* How should we do it

Historically backups were done on tapes

Special files and temporary files don't need back ups :)


### **different approaches**

* Incremental dumps
  - Full/complete dump weekly /monthly
  - daily dump of modified files
  - to restore file system we need to start with full dump and include modifided files
- Storing everything 1:1
  - Waste of space => compress files
- Backup of active system
  - Need snapshot algorithms
- Physical dump
  - It is simple to implement
  - Block by block
    - but this means that we just backed up unused blocks
    - bad blocks
- Logical dump
  - Traverse the tree
  - dump files/ directories that have been modified since a given time
    - Can restore path on different computer
    - have the ability to restore a single file
  - The most common approach

### **phases of backup**
1. find modified files and mark them, mark all directories
2. unmark directories without modified files
3. dump marked directories
4. dump file
### **restoring filesystem to backup**
* Create a new empty filesystem
* use the full dump as starting point
* create folders
* create files
* restore incremental dumps

## Filesystem consistency
Most filesystems do batched writeouts
* Keep changes in memory, writeout later
* Crash before we write something?
  * => inconsistent state
* We fix this by looking at the i-nodes and a free/use list

Check block consistency during boot
* Count how many times a block occurs.
    * If mentioned 0 times -> add to free list
* Twice in the free list
  * Remove one instance of it
* Twice in use list
  * Zoinks scoob, something went wrong (duplicate the block to both files)

We also need to check directory consistency (i-nodes)

For each i-node increment a counter for that file's usage (remember hard links? NO)
* Too high
  * You may have removed all files but the I-node lives on
* Too low
  * Removing in one directory removes the file in both
  * data lost
  * dangligng pointer to i-node
    * i-node reused

Checking I-nodes and blocks are integrated

## Performance
We can speed up reading from disk by not reading at all
Memory vs harddisk
* Reading a block (16 bytes) takes 10 ns vs 5-10 ms in startup

This is called buffer cacheing
- We keep some blocks in memory (the really hot ones)

### disk caching
* create a hashtable
  * Hash the device and disk adress
* To manage the buffer cache we can do the following
  * Use a mix of FIFO based on LRU and second chance
    * There are some inherent risks of storing your most recently used blocks on memory
  * Sync to the disk regularly

### What to keep in cache
- First we have to differentiate between blocks
  - i-node, indirect, directory, full data blocks and partially full data blocks
  - Write critical blocks to disk
  - Sync data in intervalls
- Write-through caches
- When something is placed in the cache write to disk

### Block read ahead
Much like prefetching we can guess which blocks will be needed
A block read ahead, ie., if block k is read into cache, read in k+1 if it is not already there
Read operations are often sequential
But if access is random
  * Then everything is slowed down
  * Blocks are removed from  cache
* We have to see a pattern then guess
  * Track the pattern
  * Use a bit to flip between random and sequential
  * benefit of the doubt

# INPUT/OUTPUT FL 10
## Terminology
### Superscalar
denoting a microprocessor architecture where several instructions are loaded at once and, as far as possible, are executed simultaneously, shortening the time taken to run the whole program.

## Transfer data
We need to be able to issue commands somehow
* This is relevant to all devices
  * CPU to MMU
  * CPU to hard drive
  * MMU to CPU

Communication is different between all devices

Two types of devices: block and character

There are three types of methods
- Polling -"Do this. Are you done? Are you done? Are you done?..."
- Interrupt driven - "Do this one thing - come back when you're done"
- DMA - here is a shopping list - fetch me this and come back later

**A character device**
streams data contiguously, one word/byte at a time e.g., mouse and keyboard. Generally defined as non-addressable devices.

**A block device** stores information in fixed-size blocks, each with its own address, e.g., hard disks, usb sticks. Block devices are addressable and blocks can be read independently

The following things are required for hardware communication
* Common interface
* Large degree of variation
  * Different functions
  * bandwidth 1b/s -20 GB/s
  * Complexity: single function - small computer
* Must share buses
* Need to manage control and communication
### Device controllers
Device: mostly unique electronics
I/O unit has two parts:  a mechanical device and a electronic device (device controller)
Controller provides an interface
  - Regisers
  - Device ID (or unique register addresses)
  - Collision sensing
- Bus is a collection of shared wires
  - Read,write, lock
  - Address / Data
### Device registers
Information between the device and the software are passed through registers
* Status registers
  * read by CPU
  * Information about device state
* Control registers
  * Written to by CPU
  * Issue commands
* Data registers
* Functionality is bit-addressed.

### I/O over ports
**I/O ports definition**
Data transfer from controller register. Done via hardware instruction, restricted to kernel mode.

Register maps to i/o port number
* Special instructions
  * IN and OUT are ring0 instructions
* IN REG PORT
* OUT REG PORT
* Where REG is EAX, EBX

### Memory mapped I/O
**Definition**

All controller registers are mapped to memory. Read/write from/to addressses

All registers are mapped into memory
* OS controllers see them

Memory mapped I/O can be written in C
* No need for special instructions
* No protection
* Faster
* Do not cache

## DMA MUI IMPORTANT

**Direct access memory**

Physical hardware that handles all I/O operations. Sits on the bus. CPU configures it by manipulating registers and saying go. Result of I/O stored in internal buffer, then moved to memory

Normally CPU has to waste cycles requesting bits, moving bits
* Normally when the controller is done CPU receives an interrupt
  * CPU has to fetch all data by itself

Instead, we send a command to DMA
  - In a sense the cpu does fire and forget

When DMA is finished an interrupt is sent to the CPU

The Interrupt vector is an array like structure that holds addresses to interrupt service routines. These are routines that handle the interrupt


When the I/O is done, device asserts on bus line and signals interrupt controller. The motherboard detects the signal and if it is not busy jumps into the interrupt vector to identify where in the vector we should go to call right routine.  If controller is busy ora higher priority interrupt happened it continues asserting until it goes through.


An in sync replica is acknowledged by writing a value to interrupt controller register
  * If we  didn't do this, race hazards might occur

Acknowledges by writing number on I/O
  * Ready for more ints
  * Avoids race hazards

### Stuff to do ISR (in sync replica)
Where do we store the state of the CPU?
  * We could store it in registers
  * Whose stack?
    * It could be the user stack
      * Dangling esp
* Stock could be at the end of page
  * Where to store the data to handle page fault
* End of page
  * Pinned kernel page
  * But that will invalidate MMU and TBL

I genuinely have no idea

## Precise and imprecise interrupts
* Check for interrupts after instruction
* * Is this really smart on a superscalar machine (internally parallel)
  * Not all operations are finished

Various instructions could be half finished

**Two cases**

Precise interrupt (we know for a certainty that all instructions are finished)
1. Program counter is saved in known place
2. All instructions before program counter have completed
3. No instructions beyond program counter have finished
4. The execution state of the instruction point to be the PC is known

Imprecise interrupt is the opposite :(

### Solutions to imprecise
We don't need to worry about it X86 is superscalar and is precisse
* It is backwards compatible
  * has a complex interrupt logic
* Barrier
  * All up to some point but not more
* We pay with hardware
* Some are required to be precise, some not
  * I/O interrupt
  * Traps, division by zero
*
## Principles of I/O software
* Device independence
  - Reading from USB, DVD, or HDD
  - No details, use abstractions
  - Sort \<input\>output
  - Input should work from keyboard
  - Output destination shouldn’t matter
* We (i.e., OS) are responsible for this
  *  Taking care of problems arising from different interfaces to devices
* Uniform naming
  * Should not depend on the device
  * Name should be string or integer
* Error handling, as close to the device as possible
  * Controller first, then driver, etc.
  * Transient errors
  * Do not inform your boss, try to solve it yourself first.
* Synchronous vs asynchronous transfers
  * asynchronous start transfers, do something else, interrupt
* Buffering
  * Incoming data can’t be reasoned with until read.
  * Some data must be streamed at an even rate

# pROGRAMMED I/o
Simplest form of using the CPU (Example below):
- Goal is to move data from user to printer
- make a SYSCALL to see if printer is busy
- Print data SYSCALL, move data to kernel
- Kernel checks again
- Move data to printer
- Printer does line by line, fed by CPU, registers

Very simple but kills performance
## Interrupt driven I/O
Interrupt driven I/O basically blocks any process which request a busy I/O. Then when the I/O is done, it sends an interrupt to to the CPU and the process is rescheduled. The process is put back and the CPU checks every X ms to see if it is done.

Printer can do 100 chars per sec, 10 ms per char
  - CPU busy waits 10 ms at a time
    - Do something productive with your time

Idea: block process which requests I/O, schedule another process
  - Return to calling process when I/O is done
  - Printer generates interrupt when a character is printed
  - Keeps printing until the end of the string
  - Re-instantiate calling process
* Context switch

## I/O and DMA
We delegate the task to the DMA and the DMA gives us an interrupt when it is done.
Interrupting after each char is very costly
  - lots of overhead
- What if we program the DMA what we want printed
- CPU can now do something else while DMA works
  - Configure DMA
  - This reduces interrupts
  - DMA is slower than the CPU
    - So if the CPU is not busy, it might as well do itself

## Interrupt handlers (ISR - Interrupt service routine)
Driver blocks itself - interrupt handlers unblock when finished
The details are dirty (general outline)
* save registers
* setup context for ISR, TLB MMU and page table
* Setup stack for ISR
* Acknowledge interrupt controller/reenable interrupts
* Copy the registers from where they were saved to the process table
* Run ISR
* Scheduler picks next process
  * Setup MMU
  * Load new registers
  * Start new process
* All of this just because you pressed a key, and it is very expensive.


## I/O software layers
1. User-level i/o software
2. Device-independent operating system software
3. Device drivers
4. interrupt handlers
5. hardware
Hardware is the lowest level.

## Device drivers
I/O device specific code that handles communication with I/O device. Operates of then in kernel mode

Every device is different and requires specific code to work. Drivers are that code. Below are some examples of differences between I/O device drivers and what they have to take care of
* Mouse drivers
  * Which buttons
  * Delta-XY
* HDD driver
  * Sectors
  * Tracks
  * Heads
  * Arms

Each driver is for one device only (a device might have many drivers)
* Usually supplied by manufacturer
* Installed in the kernel (bad drivers can mess up OS)

USBs are an exception since they have one and the same interface (that of the computer)

Standard interface for block and character devices are checking input params, availablilty etc

Controlling the device is just issuing commands in sequence
  * Wait or do next
  * If we wait interrupt

Drivers are also in charge of checking for error

### Real-life problems with drivers
* I/O might finish while driver is running
* May cause another driver to start
* Which may cause the first driver to start
* While network driver processes a package, another one arrives
* Hot-pluggable systems
  * Removed while writing
* Syscalls are not allowed for drivers

## Device-Independent I/O software
Some I/O operations are applicable in all drivers.
If we have specific API per driver, we won’t do anything else but write OS handling of drivers

* Define a set of functions for a class of drivers, e.g.,
  - Disks
  - Printers
* Uniform driver interface
* Table of pointers for functions
  * OS calls functions
* OS maps symbolic device names onto the right
driver
* Unix: /dev/disk0 maps to an i-node which contains
the major and minor device numbers for disk0

## Buffering
Buffering is storing data in memory
* Modem sends bits
  * Interrupt on each char
  * Start user process to handle this
    * Not feasible
* N-char buffer in userspace
  * Wake up when full
  * If the OS just paged it out
    * Lock it in memory
    * Can’t launch a process if we have too many pinned pages
* N-buffer in kernel space
  * Can’t get paged out
  * But what happens when buffer is full, and we get new chars?
* 2 N-buffers in kernel space
  * When first is full, process and write to second
  * Dubble buffering, this how graphics cards work
### Circular buffering
* Two pointers, head and tail.
* Hardware controls the head, OS the tail
  * Hardware moves head forward when adding items
  * OS moves tail forward when removing items
* But how do we do output without buffering?
* Syscall with N bytes
  * Block
  * Or just let the user calculate something while waiting
    * How does the user know that the buffer is available?
### Buffering is not free
* Buffering in a sequential fashion will ruin performance
* First from user to kernel
* Kernel to controller
  * Uniform speed, DMA and cycle stealing
  * Not adhering to require speed ruins the packet
* Onto network
* Controller
* Kernel
* User
* ACK
* Then we can send next

## Userspace I/O-software
Some I/O takes place in libraries. Here are some examples:
- Printf / cout
- Write
- Printf is just a formatting library call
  * Who then calls the OS to do something with the string
  * This is why printf is expensive (syscalls)
* Spooling
  * Process grabs printer, does nothing
  * Daemon in charge of the spooler directory
  * Processes requests, daemon decides
* Network daemon


## RAID is something you should read about

## Clock hardware and software
Typical duties of a clock driver:
* Maintaining the time of day.
* Preventing processes from running longer than they are allowed to (clock interrupt).
* Accounting for CPU usage.
* Handling alarm system call made by user processes.
* Providing watchdog timers for parts of the system itself.
* Doing profiling, monitoring, statistics gathering
