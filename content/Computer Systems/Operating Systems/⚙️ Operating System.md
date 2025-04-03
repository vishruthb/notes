An operating system (OS) sits between applications and hardware, providing abstractions to the applications and implementing abstractions and managing resources for the hardware.
# OS & Hardware
- Resource allocation
- Resource reclamation
- Protection (between & from applications)
# OS & Applications
- The OS defines a set of logical resources (objects) and a set of well-defined operations on these objects (interfaces)
	- Files => Create, read, write
	- Threads => Create, yield, exit
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
OS is a giant interrupt handler, once the system is booted up, all entries to the kernel occurs due to:
- [[💥 Faults]]
- [[📞 System Calls]]
- Timer and I/O [[🛑 Interrupts]]

This happens via events at the user level and through the dispatcher at the kernel level that directs to interrupt service routines, system services, and fault handlers within the OS.
# Referencing Data
- Processes and the OS are in different address spaces
- Use names instead of pointers, such as the integer object handles or descriptors such as the UNIX file descriptors