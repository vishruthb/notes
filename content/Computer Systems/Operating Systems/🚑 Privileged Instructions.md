A subset of instructions that can only run in kernel mode.
- CPU checks mode bit when privileged instructions execute
- Attempts to execute in user mode are detected and prevented by the CPU
# Capabilities
- Directly access I/O devices (disk, network, etc.)
- Manipulate memory-management state (page table pointers, etc.)
	- Preventing apps from accessing other app's memory or the OS's memory
- Manipulate protected control registers (e.g. mode bit)
	- Prevent apps from giving themselves privileges
# Example
HLT: Halt instruction (assembly), halts the CPU
