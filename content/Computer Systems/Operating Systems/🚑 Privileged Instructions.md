a subset of instructions that can only run in kernel mode.
- cpu checks mode bit when privileged instructions execute
- attempts to execute in user mode are detected and prevented by the cpu
# capabilities
- directly access i/o devices (disk, network, etc.)
- manipulate memory-management state (page table pointers, etc.)
	- preventing apps from accessing other app's memory or the os's memory
- manipulate protected control registers (e.g. mode bit)
	- prevent apps from giving themselves privileges
# example
hlt: halt instruction (assembly), halts the cpu
