Goals of memory management include multitasking, transparency, isolation (protection), and efficiency.
# Challenges
- Finite memory capacity, as processes's data might not fit in physical memory and might run many processes at once
- Locating data in memory, we don't know where each processes's data is located in memory
- Processes should not be able to read or write each other's memory and not corrupt OS memory
- Need to be able to support many processes at once, keeping CPU overheads low
# Multitasking
### w/ Static Relocation
- Support multiple processes by relocating once at load time
- Highest memory holds the OS, so when a process is loaded, a region of memory is allocated, and the loader rewrites all memory addresses to relocate the process
- Limitations:
	- N protection between **processes** or of **operating system**
	- Low memory utilization, as addresses are fixed after loading => cannot relocate at runtime to fill holes
	- No sharing: one segment per process, and cannot share parts of the process address space
	- Entire address space needs to fit in memory
### w/ Dynamic Memory Relocation
- Change address **dynamically** as a process executes
- Virtual addresses are independent of physical location of referenced data, are used to refer to memory locations, and are translated to **physical addresses** during every memory reference
- OS makes decision on where to place data in **physical memory**
# Virtual Memory
The abstraction that the OS provides for managing memory. Has two views of memory, **physical address space** and **virtual address space** (seen by the program).

>[!Note]
> Virtual address space often much larger than physical adddress space (64-bit addresses)

Allows for:
- Flexibility: OS can move processes around in memory as they execute
- Transparency: Hardware handles address translation
- Protection: Can check for isolation during translation
- Efficiency in memory usage
# Multi-Level vs. Linear
Allows for allocation of page-table pages for regions of the address space actually being used, instead of one huge table that covers the entire 4 GB (on 32 bit) or larger space. Sparsity => Saves RAM at the cost of one extra memory indirection on a [[Translation Lookaside Buffer]] miss.

