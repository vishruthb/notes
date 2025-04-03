An unnatural change in control flow.
- Immediately stop the current execution
- Changes mode, context (machine state), or both

The [[⚙️ Operating System]] defines a handler for each event type. Specific types of events are defined by the machine, and event handlers execute in kernel mode.

After the system is booted, all entry to the kernel occurs in response to some event. OS is kind of like one big event handler, only executing in reaction to events.
# Types
Two main types: exceptions and interrupts.
## Exceptions
Caused by program executing instructions.
- Executing a privileged instruction (fault)
- Requesting services from the operating system (sys call)
- Also called 'traps'
## Interrupts
Caused by an external event.
- Device finishes I/O, timer expires, etc.
