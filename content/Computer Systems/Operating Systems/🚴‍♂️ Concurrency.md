Threads cooperate in multithreaded programs in order to share resources, access data structures like a memory cache in a webs server, and to coordinate their execution (relative execution). With multiple cooperating threads, we have non-deterministic results and scheduling order **does** matter.
# Problems
### Race Conditions
Results depend on the timing execution of the code.
### Interleaved Executions
Threads interleave executions arbitrarily and at different rates, and scheduling is not under program control.
### Shared Resources
Threads can access shared resources (e.g. variables) and data structures (buffers, queues, etc.).
- Local variables are **not shared**, as they refer to data on each thread's own stack.
- Global variables and static objects are **shared**, and are stored in the static data segment accessible by any thread.
- Dynamic objects and other heap objects are **shared**, as they are allocated from the heap with `malloc/free` or `new/delete`.
# Synchronization
A way to control cooperation to restrict the possible interleavings of thread executions.
- **Mechanisms** to control access to shared resources: locks, mutexes, semaphores, monitors, condition variables, etc.
- **Patterns** for coordinating access to shared resources: producer-consumer, reader-writer, etc.
# Mutual Exclusion
Goal is to create **critical sections**.
- Section of code in which only one thread may be executing at a given time
- All other threads are forced to wait on entry
- Thread leaves critical section => Another can enter

Mutual exclusion allows us to create critical sections, with at most one thread in a critical section at once, allowing us to have larger atomic blocks (sections of code executed without interruption).
# Critical Section
### Goals
(Safety) Mutual Exclusion
- If one thread is in the critical section, then no other thread is.

(Liveness) Progress
- If some thread T is not in the critical section, then T can't prevent some thread S from entering the critical section
- A thread in the critical section will eventually leave

(Liveness) Bounded Waiting
- Some waiting thread T will eventually enter the critical section

Performance
- Overhead of entering and exiting the critical section is small, relative to work being done within it
### Building Critical Sections
- Atomic read/write
- Locks: Primitive minimal semantics, used to build others
- Semaphores and condition variables: Basic, easy to get the hang of, harder to program with
- Monitors: High-level, requires language support, implicit operations
- Messages: atomic transfer of data across a channel => distributed systems
# Locks
An object in memory providing two operations:
- `acquire()` or `lock()` to enter a critical section
- `release()` or `unlock()` to leave a critical section

Threads pair calls to `acquire` and `release`. Between `acquire`/`release`, the thread holds the lock, and `acquire` does not return until any previous holder releases.

Implementation of `acquire`/`release` needs to be atomic, executing as though it can't be interrupted.
## Spinlocks
- Threads waiting to acquire lock spin in test-and-set loop
- Wastes CPU cycles
- Longer the critical section, longer the spin
- Greater chance for lock holder to be interrupted
- Good to use as primitives to build high-level synchronization constructs
## Disabling Interrupts
- Doesn't work on multicore CPUs
- Should not disable interrupts for long periods of time
- Can miss or delay important events (e.g. timer, I/O)