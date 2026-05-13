hardware detects and report exceptional conditions.
- e.g. divide by zero, page faults

when the hardware faults upon an exception, we must save the state (pc, registers, mode, etc.) so faulting process can be restarted. each exception type has an associated number, and the cpu finds the exception handler for that number. we then switch to kernel mode and start executing the exception handler. os returns to program when done, reversing the steps.
# recovery
some faults are handled by **fixing the exceptional condition directly**, such as:
- a page fault causing the os to bring the missing pages into memory => fault handler returns to program, re-executing the instruction that caused page fault
also handled by **notifying the process**:
- applications can register a fault handler within the os
- os fault handler will return to the user-mode handler
# termination
os might handle unrecoverable faults by killing the user process.
- program fault with no registered handler
- halt process, write process state to a file, destroy process
faults in kernel:
- dereference null, divide by zero, undefined instruction
- these faults considered fatal, os crashes