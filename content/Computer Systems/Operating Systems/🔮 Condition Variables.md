a synchronization primitive that enables a queue of threads waiting for something inside a critical section. supports three operations:
- `wait() / sleep()`: release the lock, go to sleep, wake up and re-acquire lock when signaled (releasing the lock and going to sleep is atomic).
- `signal() / wake()`: wake up a waiting thread, if any. signaled thread must **re-acquire the associated lock** before resuming.
- `broadcast() / wakeAll()`: wake up all waiting threads, if any. each thread must re-acquire the lock before resuming (one at a time).

used in conjunction with **locks**. on creation, must specify which lock it is associated with, must hold the lock when invoking condition variable operations, and the lock will be atomically released and acquired during `wait()`.

>[!warning]
>condition variables are not for mutual exclusion, its instead handled by an associated lock.
### contrast with [[🔗 Semaphores|🔗 semaphores]]
- no counting involved in tracking how many times `signal()` was called.
- memoryless: if `signal()` is called when no thread is waiting, it does nothing and is lost. semaphores on the other hand, remember and increment their count.
- more common in modern programming.
- condition variables must be used with a lock for coordination, not for mutual exclusion.
# signal semantics
when `signal()` is called, only one thread can hold the lock at once. two approaches to help us understand if the signaler or the woken thread run first, via mesa vs. hoare semantics.
### mesa semantics
- signaler keeps the lock and continues running
- waiter is put on the ready queue
- the condition is not necessarily true when the signaled thread runs again
- returning from `wait()` is only a hint that something changed, must recheck the conditional case

| signaling thread  | waiting thread                        |
| ----------------- | ------------------------------------- |
| `acquire()`       |                                       |
| `signal()` [lock] | runnable                              |
| `release()`       | acquire lock and return from `wait()` |
|                   | `release()`                           |
```c
while (count == N)
	wait(not_full);
```
### hoare semantics
- signaler passes the lock to the waiter, waiter runs immediately
- the condition is true when the signaled thread runs again, no need to recheck the conditional case

| signaling thread                        | waiting thread                        |
| --------------------------------------- | ------------------------------------- |
| `acquire()`                             |                                       |
| `signal()`                              | acquire lock and return from `wait()` |
|                                         | `release()`                           |
| acquire lock and return from `signal()` |                                       |
| `release()`                             |                                       |
```C
if (count == N)
	wait(not_full);
```
# pitfalls
- cannot be tested
- need to maintain a separate flag
- do not release the lock before using the condition variable, using it requires that the thread holds the lock
- purpose of a condition variable is to enable threads to block while in a critical section
- need to hold the lock while testing the condition
- the condition involves shared variables (e.g. `flag`) and is at risk of race conditions otherwise