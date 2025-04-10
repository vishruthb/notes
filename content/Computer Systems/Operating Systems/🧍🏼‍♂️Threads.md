A sequential execution stream within a process (PC, SP, registers)
- Each thread is bound to a single process, but a process can have multiple threads
- Threads are the basic **unit of scheduling**, processors serve as the **containers** in which threads execute
# Address Space
|               |                                                          |                                                 |
| ------------- | -------------------------------------------------------- | ----------------------------------------------- |
| 0xFFFFFFFF    | Stack (Thread 1)<br>Stack (Thread 2)<br>Stack (Thread 3) | SP (Thread 1)<br>SP (Thread 2)<br>SP (Thread 3) |
| &uarr;        | &darr; &uarr;                                            | Stack Pointer (SP)                              |
| Address Space | Heap (Dynamic Memory Alloc)                              |                                                 |
| &darr;        | Static Data (Data Segment)                               |                                                 |
| 0x00000000    | Code (Text Segment)                                      | PC (Thread 3)<br>PC (Thread 1)<br>PC (Thread 2) 
# Thread Control Blocks
Each PCB contains two kinds of information:
- Shared information
	- Memory: code/data/heap segments, page tables
	- I/O and files: open file descriptors
- Per-thread information (in Thread Control Blocks):
	- State (ready, running, or waiting)
	- PC, registers
	- Execution stack
# TCBs and Hardware State
While a thread is running, its hardware state (PC, SP, regs, are in the CPU), where hardware registers contain the current values.
- OS stops running a thread => Saves the registers into the thread's TCB
- OS resumes running a thread => Loads the registers from the values store that thread's TCB
**Context Switch** is the process of changing the CPU hardware state from one thread to another, as often as every millisecond.
# Thread Queues
OS maintains a collection of queues to keep track of threads.
- Ready queue => Threads that are ready to run
- Waiting queues => Can be many, one for each type of wait (disk, timer, network, synchronization)
Each TCB is queued on a state queue according to its current state. When a thread changes state, the OS unlinks its TCB from one queue and links it into another.
# Thread Scheduling
## Non-Preemptive Scheduling
- Threads voluntarily give up the CPU with `yield()`
`yield()`:
- Gives up the CPU to another thread, **context switching** to the other thread
- Returns another thread called yield
### Thread Context Switch
All done in assembly.
- Saves context of the currently running thread, pushing all machine state onto its stack or TCB
- Restores context of the next thread, popping all machine state from the next thread's stack or TCB
- The next thread becomes the current thread
- Return to caller as new thread
## Preemptive Scheduling
- Uses involuntary context switches, uses timer interrupts to regain control of the CPU
- Timer interrupt handler forces current thread to yield
- Preemptive scheduling is the default in OS, as OS cannot rely on threads to cooperate
# Process/Thread Separation
- Separating threads and processes makes it easier to support concurrent applications, as concurrency does not require creating new processes
- Concurrency (multithreading) can be used to:
	- Improve program structure
	- Handling concurrent events (e.g. web requests)
	- Writing parallel programs
	- Useful with many cores or just one core
# Kernel-Level Threads
aka. OS-managed threads
- Windows: threads
- POSIX Threads: pthreads
OS manages threads and processes:
- All thread oeprations are implemented in the kernel
- OS schedules all the threads in the system
## User and Kernel Stacks
|                  |                  |                                                            |
| ---------------- | ---------------- | ---------------------------------------------------------- |
| Process          | User-level Stack | Use kernel stack during system call, event handling &darr; |
| Operating System | Kernel Stack     | &larr;                                                     |
- Multiple kernel threads (OS manages, schedules)
- Physical parallelism (can run on multiple cores)
- Multiple separate system calls/events
## Limitations


