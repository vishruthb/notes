An operating system (OS) sits between applications and hardware, providing abstractions to the applications and implementing abstractions and managing resources for the hardware.
# OS & Hardware
- Resource allocation
- Resource reclamation
- Protection (between & from applications)
# OS & Applications
- The OS defines a set of logical resources (objects) and a set of well-defined operations on these objects (interfaces)
	- Files: Create, read, write
	- Threads: Create, yield, exit
- Provides illusion of "infinite memory" or "sole application running"
# Design
## User Level:
- Application, Libraries
- Run in user mode, **cannot execute privileged instructions**
## Kernel Level:
- Portable OS layer, Machine-dependent layer
- Run in kernel mode, **can execute privileged instructions**
# Protections
OS uses:
- [🚑 Privileged Instructions](🚑%20Privileged%20Instructions.md) 
- [[🧠 Memory Protections]]
to protect itself from applications and protect applications from each other, while still performing special tasks like managing resources
# Interactions
OS is a giant interrupt handler, once the system is booted up, all entries to the kernel occurs due to [[🎡 Events]] such as:
- [[💥 Faults]]
- [[📞 System Calls]]
- Timer and I/O [[🛑 Interrupts]]

This happens via events at the user level and through the dispatcher at the kernel level that directs to interrupt service routines, system services, and fault handlers within the OS.
# Referencing Data
- Processes and the OS are in different address spaces
- Use names instead of pointers, such as the integer object handles or descriptors such as the UNIX file descriptors
# Processes
- A [[🏠 Process]] is the OS's abstraction for a running program, used to manage execution, scheduling, and other resources. Includes things such as an address space, OS resources and accounting information, and execution state
# Threads
- [[🧍🏼‍♂️Threads]] are sequential execution streams within a process, allowing us to divide a process into smaller, concurrent units of work.
# Concurrency
[[🚴‍♂️ Concurrency]] allows for multiple tasks to be in progress at once.
- Application benefits:
	- Web Servers -> Handle multiple request simultaneously
	- Multicore -> Utilize multiple cores with one aplication
	- Overlapping I/O -> Perform multiple I/O operations in parallel
- Can use multiple processes by 1) creating several processes (e.g. via `fork()`) and 2) setting up a shared memory region between them
- Inefficient due to space and time
	- Space: PCBs, memory-management state (page tables)
	- Time: Create data structures, fork and copy address space
- Cooperating processes share same code and data (address space) and resources (file, sockets, etc.), but have their own execution state (PC, SP, registers)
# Synchronization Primitives
Locks are useful for implementing critical sections, but have limited semantics as they just provide mutual exclusion, which doesn't solve all synchronization problems. Ideally, we'd like to be able to wait for shared resources to become available, allow multiple threads to generate different resources, and use certain conditions to decide when to enter a critical section.
- [[🔗 Semaphores]] allow us to keep a count of available “permits,” blocking threads when none remain and waking them as permits are released.