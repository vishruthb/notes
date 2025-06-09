Goals of memory management include multitasking, transparency, isolation (protection), and efficiency.
# Challenges
- Finite memory capacity, as processes's data might not fit in physical memory and might run many processes at once
- Locating data in memory, we don't know where each processes's data is located in memory
- Processes should not be able to read or write each other's memory and not corrupt OS memory
- Need to be able to support many processes at once, keeping CPU overheads low
# Multitasking
### w/ Static Relocation
- Support multiple processes by relocating once at load time
- Highest memory holds the OS, so when a process is loaded, a region of memory is allocated, and the loader rewrites all memory addresses to relcoate the process
- Limitations:
	- N protection between **processes** or of **operating system**
	- Low memory utilization, as addresses are fixed after loading => cannot relocate at runtime to fill holes
	- No sharing: one segment per process, and cannot share parts of the process address space
	- Entire address space needs to fit in memory
### w/ Dynamic Memory Relocation
- Change address **dynamically** as a process executes
- Virtual addresses are independent of physical location of referenced data, are used to refer to memory locations, and are translated to **physical addresses** during every memory reference
- OS makes decision on where to place data in **physical memory**