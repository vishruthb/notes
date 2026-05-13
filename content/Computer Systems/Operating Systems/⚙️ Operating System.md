an operating system (os) sits between applications and hardware, providing abstractions to the applications and implementing abstractions and managing resources for the hardware.
# os & hardware
- resource allocation
- resource reclamation
- protection (between & from applications) and interactions rely on hardware support
# os & applications
- the os defines a set of logical resources (objects) and a set of well-defined operations on these objects (interfaces)
	- files: create, read, write
	- threads: create, yield, exit
- provides illusion of "infinite memory" or "sole application running"
# design
## user level:
- application, libraries
- run in user mode, **cannot execute privileged instructions**
## kernel level:
- portable os layer & machine-dependent layer
- run in kernel mode, **can execute privileged instructions**
# protections
os uses:
- [🚑 privileged instructions](🚑%20Privileged%20Instructions.md)
- [[🧠 Memory Protections|🧠 memory protections]]
to protect itself from applications and protect applications from each other, while still performing special tasks like managing resources
# interactions
os is essentially a giant interrupt handler, so once the system is booted up, all entries to the kernel occurs due to [[🎡 Events|🎡 events]] such as:
- [[💥 Faults|💥 faults]]
- [[📞 System Calls|📞 system calls]]
- timer and i/o [[🛑 Interrupts|🛑 interrupts]]

this happens via events at the user level and through the dispatcher at the kernel level that directs to interrupt service routines, system services, and fault handlers within the os.
# referencing data
- processes and the os are in different address spaces
- use names instead of pointers, such as the integer object handles or descriptors such as the unix file descriptors
# processes
- a [[🏠 Process|🏠 process]] is the os's abstraction for a running program, used to manage execution, scheduling, and other resources. includes things such as an address space, os resources and accounting information, and execution state
# threads
- [[🧍🏼‍♂️Threads|🧍🏼‍♂️threads]] are sequential execution streams within a process, allowing us to divide a process into smaller, concurrent units of work.
# concurrency
[[🚴‍♂️ Concurrency|🚴‍♂️ concurrency]] allows for multiple tasks to be in progress at once.
- application benefits:
	- web servers -> handle multiple request simultaneously
	- multicore -> utilize multiple cores with one aplication
	- overlapping i/o -> perform multiple i/o operations in parallel
- can use multiple processes by 1) creating several processes (e.g. via `fork()`) and 2) setting up a shared memory region between them
- inefficient due to space and time
	- space: pcbs, memory-management state (page tables)
	- time: create data structures, fork and copy address space
- cooperating processes share same code and data (address space) and resources (file, sockets, etc.), but have their own execution state (pc, sp, registers)
- [[💀 Deadlock|💀 deadlock]]s happens when two or more threads each hold one resource and wait indefinitely for the other's.
# synchronization primitives
locks are useful for implementing critical sections, but have limited semantics as they just provide mutual exclusion, which doesn't solve all synchronization problems. ideally, we'd like to be able to wait for shared resources to become available, allow multiple threads to generate different resources, and use certain conditions to decide when to enter a critical section.
- [[🔗 Semaphores|🔗 semaphores]] allow us to enforce critical sections via mutual exclusion while enabling coordination between threads via scheduling, helping us solve problems like producer-consumer and reader-writer
- [[🔮 Condition Variables|🔮 condition variables]] allow a thread to sleep until another signals that a condition is true. always used with a **lock**.
- [[🖥️ Monitors|🖥️ monitors]] provide a high-level construct that bundles a mutex and its condition variables into one object.
# cpu scheduling
the [[🗓️ Scheduler|🗓️ scheduler]] allows for cpu resources to be shared across processes or threads by time-slicing the cpu.
# managing memory
[[🍎 Memory Management|🍎 memory management]] allows us to share the memory on one server amongst many processes.
