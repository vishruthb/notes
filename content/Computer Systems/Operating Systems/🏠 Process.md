# Components
Serves as the OS's container for a program's execution environment, storing all state information for the program:
- A memory address in space
- Code and data for the executing program
- Execution stack encapsulating state of procedure calls
- Program counter indicating next instruction
- Set of registers with current values
- Set of OS resources => Open files, network connections, etc.

Naming: process ID (PID)
# Address Space
| 0xFFFFFFFF    | Stack                       |                      |
| ------------- | --------------------------- | -------------------- |
| &uarr;        | &darr; &uarr;               | Stack Pointer (SP)   |
| Address Space | Heap (Dynamic Memory Alloc) |                      |
| &darr;        | Static Data (Data Segment)  |                      |
| 0x00000000    | Code (Text Segment)         | Program Counter (PC) |
# Process State
Indicates what the process is currently doing:
- **Running**: Executing instructions on the CPU, this process has control of the CPU. Number of running processes at any given moment is dependent on the number of CPU cores.
- **Ready**: Waiting to be assigned to the CPU, ready to execute, but another process is executing on the CPU
- **Waiting (blocked)**: Waiting for some event to occur (e.g. I/O Completion), most processes spend the majority of their time in this state (sleeping).
Process moves from state to state as it executes.
# Process Control Block (PCB)
A *heavyweight abstraction* that tracks the many processes running simultaneously, providing a way for the OS to represent a process in the kernel:
- Contains all the info about a process
- Memory management information
- Scheduling an execution information
- I/O and file management
# Process Creation
Every process is created by another process.
- The `parent` process creates a `child` process using a sys call.
- Child inherits some properties from the parent (on UNIX: process user ID, children execute with parent's privileges)
- After creating a child, parent may either wait for the child to finish its task or continue in parallel
- OS creates the first process (e.g. `launchd` on macOS, `init` or `systemd` on Linux, all with PID 1)
Creation API:
- Create from scratch (Windows)
- Clone from an existing process (Unix)
## `CreateProcess`
- Sys call on Windows for creating a process: `bool CreateProcess(char *prog, char *args)` (simplified)
- Creates and initializes:
	- New PCB
	- New address space
- Loads the program specified by `prog`
- Copies `args` into memory allocated in the address space
- Initializes the saved hardware context
- Sets process state as ready
## `fork`
- Sys call in Unix for creating a process: `int fork()`
- `fork()`:
	- Creates and initializes:
		- New PCB
		- New address space
	- Initializes the address space with a copy of the entire contents of the parent's address space
	- Initializes the kernel resources to point to the resources used by the parent (e.g. open files)
	- Initialize hardware context to be a copy of parent's
	- Sets process state as **ready**
- Process Creation:
	- Creates a duplicate of the original process
	- `fork()` returns twice, returning `0` to the child and the child's PID to the parent
