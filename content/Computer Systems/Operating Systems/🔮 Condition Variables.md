A synchronization primitive that enables a queue of threads waiting for something inside a critical section. Supports three operations:
- `wait() / sleep()`: Release the lock, go to sleep, wake up and re-acquire lock when signaled (releasing the lock and going to sleep is atomic).
- `signal() / wake()`: wake up a waiting thread, if any. Signaled thread must **re-acquire the associated lock** before resuming.
- `broadcast() / wakeAll()`: wake up all waiting threads, if any. Each thread must re-acquire the lock before resuming (one at a time).

Used in conjunction with **locks**. On creation, must specify which lock it is associated with, must hold the lock when invoking condition variable operations, and the lock will be atomically released and acquired during `wait()`.

>[!Warning]
>Condition variables are not for mutual exclusion, its instead handled by an associated lock.
### Contrast with [[🔗 Semaphores]]
- No counting involved in tracking how many times `signal()` was called.
- Memoryless: if `signal()` is called when no thread is waiting, it does nothing and is lost. Semaphores on the other hand, remember and increment their count.
- More common in modern programming.
- Condition variables must be used with a lock for coordination, not for mutual exclusion.
# Signal Semantics
When `signal()` is called, only one thread can hold the lock at once. Two approaches to help us understand if the signaler or the woken thread run first, via Mesa vs. Hoare semantics.
### Mesa Semantics
- Signaler keeps the lock and continues running
- Waiter is put on the ready queue
- The condition is not necessarily true when the signaled thread runs again
- Returning from `wait()` is only a hint that something changed, must recheck the conditional case

| Signaling Thread  | Waiting Thread                        |
| ----------------- | ------------------------------------- |
| `acquire()`       |                                       |
| `signal()` [Lock] | Runnable                              |
| `release()`       | Acquire lock and return from `wait()` |
|                   | `release()`                           |
```c
while (count == N)
	wait(not_full);
```
### Hoare Semantics
- Signaler passes the lock to the waiter, waiter runs immediately
- The condition is true when the signaled thread runs again, no need to recheck the conditional case

| Signaling Thread                        | Waiting Thread                        |
| --------------------------------------- | ------------------------------------- |
| `acquire()`                             |                                       |
| `signal()`                              | Acquire lock and return from `wait()` |
|                                         | `release()`                           |
| Acquire lock and return from `signal()` |                                       |
| `release()`                             |                                       |
```C
if (count == N)
	wait(not_full);
```
# Pitfalls
- Cannot be tested
- Need to maintain a separate flag
- Do not release the lock before using the condition variable, using it requires that the thread holds the lock
- Purpose of a condition variable is to enable threads to block while in a critical section
- Need to hold the lock while testing the condition
- The condition involves shared variables (e.g. `flag`) and is at risk of race conditions otherwise