for a user application to do something "privileged", must call an os procedure like system calls, the os's api.

cpu's provide a system call instruction that:
- causes an exception, which directs to a kernel handler
- passes parameter defining which system routine to call (sys call)
- saves caller state (pc, registers, mode) to be restored later
- returning from the system call will restore the state

needs hardware support to restore the saved state, reseting the mode, and finally resuming execution.
# categories
- process management
- memory management
- file management
- device management
- communication
# example
### user level
- application: `read()`
- library: `INT $0x03`
### kernel level
- os: traps to kernel (via trap handler), `read()` kernel routine