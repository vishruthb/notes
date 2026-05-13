concept of a process is separate from its execution state:
- process: address space, privileges, resources, etc.
- execution state: pc, sp, registers
execution state is called [[🧍🏼‍♂️Threads|🧍🏼‍♂️threads]]
# components
serves as the os's container for a program's execution environment (an instance of a program in execution), storing all state information for the program:
- a memory address in space
- code and data for the executing program
- execution stack encapsulating state of procedure calls
- program counter indicating next instruction
- set of registers with current values
- set of os resources => open files, network connections, etc.

naming: process id (pid)
# address space
|               |                             |                      |
| ------------- | --------------------------- | -------------------- |
| 0xffffffff    | stack                       |                      |
| &uarr;        | &darr; &uarr;               | stack pointer (sp)   |
| address space | heap (dynamic memory alloc) |                      |
| &darr;        | static data (data segment)  |                      |
| 0x00000000    | code (text segment)         | program counter (pc) |
# process state
indicates what the process is currently doing:
- **running**: executing instructions on the cpu, this process has control of the cpu. number of running processes at any given moment is dependent on the number of cpu cores.
- **ready**: waiting to be assigned to the cpu, ready to execute, but another process is executing on the cpu
- **waiting (blocked)**: waiting for some event to occur (e.g. i/o completion), most processes spend the majority of their time in this state (sleeping).
process moves from state to state as it executes.
# process control block (pcb)
a *heavyweight abstraction* that tracks the many processes running simultaneously, providing a way for the os to represent a process in the kernel:
- contains all the info about a process
- memory management information
- scheduling and execution information
- i/o and file management
# process creation
every process is created by another process.
- the `parent` process creates a `child` process using a sys call.
- child inherits some properties from the parent (on unix: process user id, children execute with parent's privileges)
- after creating a child, parent may either wait for the child to finish its task or continue in parallel
- os creates the first process (e.g. `launchd` on macos, `init` or `systemd` on linux, all with pid 1)
creation api:
- create from scratch (windows)
- clone from an existing process (unix)
## `CreateProcess`
- sys call on windows for creating a process: `bool CreateProcess(char *prog, char *args)` (simplified)
- creates and initializes:
	- new pcb
	- new address space
- loads the program specified by `prog`
- copies `args` into memory allocated in the address space
- initializes the saved hardware context
- sets process state as ready
## `fork`
- sys call in unix for creating a process: `int fork()`
- `fork()`:
	- creates and initializes:
		- new pcb
		- new address space
	- initializes the address space with a copy of the entire contents of the parent's address space
	- initializes the kernel resources to point to the resources used by the parent (e.g. open files)
	- initialize hardware context to be a copy of parent's
	- sets process state as **ready**
- process creation:
	- creates a duplicate of the original process
	- `fork()` returns twice, returning `0` to the child and the child's pid to the parent
- useful when the child is cooperating with the parent, relies upon the parent's data to accomplish task
- example: web server
```c
while (1) {
	int sock = accept();
	int child_pid = fork();
	if (child_pid == 0) {
		// Handle client request and exit
	} else {
		// Continue
	}
}
```
# starting a new program
`exec` in unix. serves as the sys call for starting a program: `int exec(char *prog, char *argv[])`
- stops the current process
- loads the program `prog` into the process address space
- initializes hardware context and args for the new program
- files remain open
- sets the process state as ready
- **does not create a new process**, just replaces current process's memory image with a new program (new code, data, stack, etc. but same pid and key attributes)
returns only if failed with an error code.

`fork()` creates a new process, and `exec()` in that child process loads the new program.
# process termination
- unix: `exit(int status)`
- windows: `ExitProcess(int status)`

the os frees resources and terminates the process by:
- closing open files and network connections
- releasing allocated memory
- terminating all threads
- removes pcb from kernel data structures, delete

process does not need to clean itself up, instead the os does it because it doesn't "trust" the process to do it itself.

# `wait()`
- pauses the current process until any child process ends
- `waitpid()` suspends until the specified child process ends
- `wait()` returns the status code of the child
- unix: every process must be collected by a parent after it finishes executing (and becomes a zombie process)
- if a parent process exits before its child, the child becomes an orphan
# communication between processes
- at process creation time, parents get once chance to pass information via `fork()`
- os provides mechanisms for communication
	- ipc: inter-process communication, typically expensive due to sys calls
	- message passing: explicit communication via `send()` and `recieve()` sys calls
	- files: `read()` and `write()` sys calls
	- shared memory for multiple processes that read/write to the same physical portion of memory, sys calls to allocate the shared region (e.g. `shm_open()`)