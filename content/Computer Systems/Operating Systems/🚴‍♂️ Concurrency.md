Threads cooperate in multithreaded programs in order to share resources, access data structures like a memory cache in a webs server, and to coordinate their execution (relative execution). With multiple cooperating threads, we have non-deterministic results and scheduling order **does** matter.
# Problems
### Race Conditions
Results depend on the timing execution of the code.
### Interleaved Executions
Threads interleave executions arbitrarily and at different rates, and scheduling is not under program control.

**Assumption:** All instructions are atomic
- Either execute completely or not at all
- E.g. read or write of a word

****
### Shared Resources
Threads can access shared resources (e.g. variables) and data structures (buffers, queues, etc.).
- Local variables are **not shared**, as they refer to data on each thread's own stack.
- Global variables and static objects are **shared**, and are stored in the static data segment accessible by any thread.
- Dynamic objects and other heap objects are **shared**, as they are allocated from the heap with `malloc/free` or `new/delete`.
