an unnatural change in control flow.
- immediately stop the current execution
- changes mode, context (machine state), or both

the [[⚙️ Operating System|⚙️ operating system]] defines a handler for each event type. specific types of events are defined by the machine, and event handlers execute in kernel mode.

after the system is booted, all entry to the kernel occurs in response to some event. os is kind of like one big event handler, only executing in reaction to events.
# types
two main types: exceptions and interrupts.
## exceptions
caused by program executing instructions. (synchronous)
- executing a privileged instruction (fault)
- requesting services from the operating system (sys call)
- also called 'traps'
## interrupts
caused by an external event. (asynchronous)
- device finishes i/o, timer expires, etc.
