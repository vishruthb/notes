For a user application to do something "privileged", must call an OS procedure like system calls, the OS's API.

CPU's provide a system call instruction that:
- Causes an exception, which directs to a kernel handler
- Passes parameter defining which system routine to call (sys call)
- Saves caller state (PC, registers, mode) to be restored later
- Returning from the system call will restore the state

Needs hardware support to restore the saved state, reseting the mode, and finally resuming execution.
# Categories
- Process management
- Memory management
- File management
- Device management
- Communication
# Example
### User Level
- Application: `read()`
- Library: `INT $0x03`
### Kernel Level
- OS: Traps to kernel (via trap handler), `read()` kernel routine