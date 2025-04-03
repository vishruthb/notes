Hardware detects and report exceptional conditions.
- e.g. Divide by zero, page faults

When the hardware faults upon an exception, we must save the state (PC, registers, mode, etc.) so faulting process can be restarted. Each exception type has an associated number, and the CPU finds the exception handler for that number. We then switch to kernel mode and start executing the exception handler. OS returns to program when done, reversing the steps.
# Recovery
Some faults are handled by **fixing the exceptional condition directly**, such as:
- A page fault causing the OS to bring the missing pages into memory => Fault handler returns to program, re-executing the instruction that caused page fault
Also handled by **notifying the process**:
- Applications can register a fault handler within the OS
- OS fault handler will return to the user-mode handler
# Termination
OS might handle unrecoverable faults by killing the user process.
- Program fault with no registered handler
- Halt process, write process state to a file, destroy process
Faults in kernel:
- Dereference NULL, divide by zero, undefined instruction
- These faults considered fatal, OS crashes