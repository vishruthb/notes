goals of memory management include multitasking, transparency, isolation (protection), and efficiency.
# challenges
- finite memory capacity, as processes's data might not fit in physical memory and might run many processes at once
- locating data in memory, we don't know where each processes's data is located in memory
- processes should not be able to read or write each other's memory and not corrupt os memory
- need to be able to support many processes at once, keeping cpu overheads low
# multitasking
### w/ static relocation
- support multiple processes by relocating once at load time
- highest memory holds the os, so when a process is loaded, a region of memory is allocated, and the loader rewrites all memory addresses to relocate the process
- limitations:
	- n protection between **processes** or of **operating system**
	- low memory utilization, as addresses are fixed after loading => cannot relocate at runtime to fill holes
	- no sharing: one segment per process, and cannot share parts of the process address space
	- entire address space needs to fit in memory
### w/ dynamic memory relocation
- change address **dynamically** as a process executes
- virtual addresses are independent of physical location of referenced data, are used to refer to memory locations, and are translated to **physical addresses** during every memory reference
- os makes decision on where to place data in **physical memory**
# virtual memory
the abstraction that the os provides for managing memory. has two views of memory, **physical address space** and **virtual address space** (seen by the program).

>[!note]
> virtual address space often much larger than physical adddress space (64-bit addresses)

allows for:
- flexibility: os can move processes around in memory as they execute
- transparency: hardware handles address translation
- protection: can check for isolation during translation
- efficiency in memory usage
# multi-level vs. linear
allows for allocation of page-table pages for regions of the address space actually being used, instead of one huge table that covers the entire 4 gb (on 32 bit) or larger space. sparsity => saves ram at the cost of one extra memory indirection on a [[Translation Lookaside Buffer|translation lookaside buffer]] miss.

