a sequential execution stream within a process (pc, sp, registers)
- each thread is bound to a single process, but a process can have multiple threads
- threads are the basic **unit of scheduling**, processors serve as the **containers** in which threads execute
# address space
|               |                                                          |                                                 |
| ------------- | -------------------------------------------------------- | ----------------------------------------------- |
| 0xffffffff    | stack (thread 1)<br>stack (thread 2)<br>stack (thread 3) | sp (thread 1)<br>sp (thread 2)<br>sp (thread 3) |
| &uarr;        | &darr; &uarr;                                            | stack pointer (sp)                              |
| address space | heap (dynamic memory alloc)                              |                                                 |
| &darr;        | static data (data segment)                               |                                                 |
| 0x00000000    | code (text segment)                                      | pc (thread 3)<br>pc (thread 1)<br>pc (thread 2)
# thread control blocks
each pcb contains two kinds of information:
- shared information
	- memory: code/data/heap segments, page tables
	- i/o and files: open file descriptors
- per-thread information (in thread control blocks):
	- state (ready, running, or waiting)
	- pc, registers
	- execution stack
# tcbs and hardware state
while a thread is running, its hardware state (pc, sp, regs, are in the cpu), where hardware registers contain the current values.
- os stops running a thread => saves the registers into the thread's tcb
- os resumes running a thread => loads the registers from the values store that thread's tcb

**context switch** is the process of changing the cpu hardware state from one thread to another, as often as every millisecond.
# thread queues
os maintains a collection of queues to keep track of threads.
- ready queue => threads that are ready to run
- waiting queues => can be many, one for each type of wait (disk, timer, network, synchronization)

each tcb is queued on a state queue according to its current state. when a thread changes state, the os unlinks its tcb from one queue and links it into another.
# thread scheduling
## non-preemptive scheduling
- threads voluntarily give up the cpu with `yield()`
`yield()`:
- gives up the cpu to another thread, **context switching** to the other thread
- returns another thread called yield
### thread context switch
all done in assembly.
- saves context of the currently running thread, pushing all machine state onto its stack or tcb
- restores context of the next thread, popping all machine state from the next thread's stack or tcb
- the next thread becomes the current thread
- return to caller as new thread
## preemptive scheduling
- uses involuntary context switches, uses timer interrupts to regain control of the cpu
- timer interrupt handler forces current thread to yield
- preemptive scheduling is the default in os, as os cannot rely on threads to cooperate
# process/thread separation
- separating threads and processes makes it easier to support concurrent applications, as concurrency does not require creating new processes
- concurrency (multithreading) can be used to:
	- improve program structure
	- handling concurrent events (e.g. web requests)
	- writing parallel programs
	- useful with many cores or just one core
# kernel-level threads
aka. os-managed threads
- windows: threads
- posix threads: pthreads
os manages threads and processes:
- all thread operations are implemented in the kernel
- os schedules all the threads in the system

makes concurrency much cheaper than processes, much less state to allocate and initialize.
## user and kernel stacks
|                  |                  |                                                            |
| ---------------- | ---------------- | ---------------------------------------------------------- |
| process          | user-level stack | use kernel stack during system call, event handling &darr; |
| operating system | kernel stack     | &larr;                                                     |
- multiple kernel threads (os manages, schedules)
- physical parallelism (can run on multiple cores)
- multiple separate system calls/events
## limitations
- suffer from overhead for fine-grained concurrency
- thread operations still require sys calls
- have to be general to support languages, runtimes, etc.
# user-level threads
threads that are managed entirely by a runtime system (user-level library).
- small and fast, represented by a pc, registers, stack, and small tcb
- creating a new thread, switching between threads, and synchronizing threads are done via procedure calls
- user-level thread operations 10-100x faster than kernel threads
- multiple user threads multiplexed on top of kernel thread
	- no physical parallelism
	- only one sys call/event at a time
## limitations
invisible to the os -> os can make poor decisions:
- blocking a process that initiated an i/o, even though the process has other user-level threads that can execute
- scheduling a process with no runnable user-level threads
# multithreading models
- many-to-one:
	- many user-level threads mapped to a single kernel thread
	- used in user-level threads
- one-to-one:
	- each user thread to a single kernel thread
	- used in kernel-level threads
- many-to-many model
	- allows many user level threads to be mapped to many kernel threads
	- used in user-level threads
	- m:n threading models
# controlling execution
- `sleep()`: moves the thread to the waiting/blocked state, usually waiting for a condition or resource
- `yield()`: voluntarily gives up the cpu, placing the thread back on the ready queue
- `finish()`: signals that a thread is done executing, allowing for cleanup
- `join()`: blocks the calling thread until another specified thread finishes