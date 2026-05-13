threads cooperate in multithreaded programs in order to share resources, access data structures like a memory cache in a webs server, and to coordinate their execution (relative execution). with multiple cooperating threads, we have non-deterministic results and scheduling order **does** matter.
# problems
### race conditions
results depend on the timing execution of the code. will only sometimes result in data corruption or some other incorrect behavior.
### interleaved executions
threads interleave executions arbitrarily and at different rates, and scheduling is not under program control.
### shared resources
threads can access shared resources (e.g. variables) and data structures (buffers, queues, etc.).
- local variables are **not shared**, as they refer to data on each thread's own stack.
- global variables and static objects are **shared**, and are stored in the static data segment accessible by any thread.
- dynamic objects and other heap objects are **shared**, as they are allocated from the heap with `malloc/free` or `new/delete`.
### [[💀 Deadlock|💀 deadlock]]
threads can become stuck in a cycle of resource waits. if each thread holds one lock and waits for another, none can move forward.
# synchronization
a way to control cooperation to restrict the possible interleavings of thread executions.
- **mechanisms** to control access to shared resources: locks, mutexes, [[🔗 Semaphores|🔗 semaphores]], [[🖥️ Monitors|🖥️ monitors]], [[🔮 Condition Variables|🔮 condition variables]], etc.
- **patterns** for coordinating access to shared resources: producer-consumer, reader-writer, etc.
# mutual exclusion
goal is to create **critical sections**.
- section of code in which only one thread may be executing at a given time
- all other threads are forced to wait on entry
- thread leaves critical section => another can enter

mutual exclusion allows us to create critical sections, with at most one thread in a critical section at once, allowing us to have larger atomic blocks (sections of code executed without interruption).
# critical section
### goals
(safety) mutual exclusion
- if one thread is in the critical section, then no other thread is.

(liveness) progress
- if some thread t is not in the critical section, then t can't prevent some thread s from entering the critical section
- a thread in the critical section will eventually leave

(liveness) bounded waiting
- some waiting thread t will eventually enter the critical section

performance
- overhead of entering and exiting the critical section is small, relative to work being done within it
### building critical sections
- atomic read/write
- locks: primitive minimal semantics, used to build others, provides mutual exclusion
- [[🔗 Semaphores|🔗 semaphores]] and condition variables: basic, easy to get the hang of, harder to program with
- monitors: high-level, requires language support, implicit operations
- messages: atomic transfer of data across a channel => distributed systems
# locks
an object in memory providing two operations:
- `acquire()` or `lock()` to enter a critical section
- `release()` or `unlock()` to leave a critical section

threads pair calls to `acquire` and `release`. between `acquire`/`release`, the thread holds the lock, and `acquire` does not return until any previous holder releases.

implementation of `acquire`/`release` needs to be atomic, executing as though it can't be interrupted.
- use a queue to block waiters
- leave interrupts enabled within critical section
- use disabling interrupts and/or spinning only to protect the critical sections within `acquire`/`release`

**limitation:** locks don't provide ordering or sequencing.
## spinlocks
- threads waiting to acquire lock spin in test-and-set loop
- wastes cpu cycles
- longer the critical section, longer the spin
- greater chance for lock holder to be interrupted
- good to use as primitives to build high-level synchronization constructs
## disabling interrupts
- doesn't work on multicore cpus
- should not disable interrupts for long periods of time
- can miss or delay important events (e.g. timer, i/o)
# implementation
implements a lock using a queue to block waiters while using a guard on the lock itself.

```c
struct lock {
    bool held = False;
    bool guard = False;
    queue Q;
}

void acquire(lock) {
    disable interrupts;
    while (test_and_set( & lock -> guard));
    if (lock -> held) {
        put current thread on lock -> Q;
        lock -> guard = False;
        block current thread;
    }
    lock -> held = True;
    lock -> guard = False;
    enable interrupts;
}

void release(lock) {
    disable interrupts;
    while (test_and_set( & lock -> guard));
    if (lock -> Q is empty)
        lock -> held = False;
    else
        move a waiting thread to the
    ready queue;
    lock -> guard = False;
    enable interrupts;
}
```