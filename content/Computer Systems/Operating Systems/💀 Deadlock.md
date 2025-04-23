Threads acquiring resources generate dependencies. Locks, semaphores, etc. protect resources, and incorrect use of synchronization can block all threads.

Deadlock is a problem that can arise when:
- Threads compete for access to limited resources
- Threads are incorrectly synchronized

Deadlock exists among a set of threads if every thread is waiting for an event that can be caused only by another thread in the set.
# Conditions
Deadlock can exist iff the following conditions hold **simultaneously**:
- Mutual exclusion: A resource is assigned to at most one thread at once
- Hold and wait: Threads holding resources can request new resources while continuing to hold old resources
- No preemption: Resources cannot be taken away once obtained
- Circular wait: One thread waits for another in a circular fashion

Eliminating any of these conditions eliminates deadlock.
# Resource Allocation Graph
Allows us to illustrate deadlock.

---
Thread A holds resource R:
```mermaid
graph RL
  R[R]
  A(A)
  R --holds--> A
```

Thread B requests resource S
```mermaid
graph LR
  R[S]
  A(B)
  A --requests--> R
```
---
If the graph has a cycle, then a deadlock may exist. If no cycles, no deadlock.

Example: Thread 1 holds Lock 1, Thread 2 holds Lock 2, Each requests the others' lock
```mermaid
graph LR
  T1(T1) --> L2[L2]
  L2 --> T2(T2)
  T2 --> L1[L1]
  L1 --> T1
```
# Multi-Unit vs. Single-Unit Resources
- Multiple resources of some types.
	- If the graph has a cycle, deadlock may exist
- Single resource of each type.
	- If the graph has a cycle, deadlock exists.
	- Useful for tracking locks.
# Preventing Deadlocks
- No mutual exclusion, make resources shareable
- No hold and wait:
	- Threads cannot hold one resource while requesting another
	- Threads try to lock all resources at once at the beginning
- Preemption: OS can preempt resources (costly)
- No circular wait:
	- Impose an order on all resources, request in order
	- Popular OS implementation technique when using multiple locks
# Deadlock Avoidance
- Avoidance
	- Specify in advance what resources will be needed by threads
	- System only grants resources requests if it knows that the process can obtain all resources it needs in future requests
	- Avoids circularities
- Banker's Algorithm
- Hard to determine all resources needed in advance