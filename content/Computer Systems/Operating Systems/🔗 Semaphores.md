# Producer-Consumer Problem
aka **Bounded Buffer Problem**
- Producer: Generates resources
- Consumer: Uses up resources
- Buffers: Fixed-size, used to hold resources between production and consumption

Problem is because producer and consumer can execute at different rates:
- No serialization of one behind the other
- There can be multiple producers and multiple consumers
- Tasks are independent
- The buffer allows each to run without explicit handoff

**Synchronization** allows us to ensure that concurrent producers and consumers access the buffer in a *correct way*.
# Theory
Semaphores are a synchronization variable that tkaes on non-negative integer values, and supports two operations:
- `wait()`: An atomic operation that waits for the semaphore to become greater than 0, then decrements it by 1
- `signal()`: An atomic operation that increments the semaphore by 1
- Initialize the semaphore to some value
- Cannot read the semaphore's value directly
### Spinning
```c
wait(s) {
	while (s <= 0);
	s--;
}

signal(s) {
	s++;
}
```
### Blocking
```c
wait(s) {
	if (s <= 0)
		sleep();
	s--;
}

signal(s) {
	if (queued thread)
		wakeup();
	s++;
}
```
# Blocking Semaphores
Each semaphore is associated with a queue of waiting thread.

`wait()`:
- If a semaphore is open (positive), thread continues
- If a semaphore is closed (non-positive), thread blocks on queue
`signal()` opens the semaphore:
- If a thread is waiting on the queue, the thread is unblcooked
- If no threads are waiting on the queue, the signal is remembered for the next thread
- Has "history", basically a counter (see [[#Implementation]])
# Types
**Binary Semaphore**
- Represents access to a single resource
- Guarantees mutual exclusion to a critical section
- Behaves like a lock

**Counting Semaphore**
- Represents a resource with many units available
- Multiple threads can pass the semaphore at once
- Number of threads determined by the semaphore "count"

Binary has count $1$, counting has count = $N$
# Semaphores vs. Locks
Semaphores have a value, enabling more semantics
- When at most one, can be used for mutual exclusion (only 1 thread in a critical section)
- When > 1, can allow multiple threads to access resources

**Use Cases:**
- Mutual exclusion - Only 1 thread accessing a resource at a time
- Event sequencing - Permit threads to wait for certain things to happen
# Producer-Consumer with Semaphores
- `signal(s)` increments s
	- `s` value is how  many items have been produced
- `wait(s)` will return without waiting only if s > 0

Constraints:
- Consumer must wait for the producer to produce items
- Producer must wait for the consumer to empty spaces
- Only one thread can manipulate the buffer at once

We use a semaphore for first two constraints (`full_count` and `empty_count`), and a lock/semaphore for the third.
# Readers-Writers Problem
An object is shared among several threads. Some threads only read the object, others only write it. We can allow multiple readers, but only one writer.

Using **semaphores** gives to constraints:
- Writers can only proceed if there no readers or writers
- Readers can only proceed if there are no writers
```c
int read_count = 0;
semaphore mutex = 1;
semaphore block_write = 1;

write() {
	wait(block_write); // wait until no readers or writers
	
	// do the writing
	
	signal(block_write);
}

read() {
	wait(mutex);
	read_count++;
	if (read_count == 1)
		wait(block_write); // wait until no writers
	signal(mutex);
	
	// do the reading
	
	wait(mutex);
	read_count--;
	if (read_count == 0)
		signal(block_write); // let a writer run
	signal(mutex);
}
```
# Implementation
```c
struct semaphore {
    int   count = 1;
    bool  guard = False;
    queue Q;
};

void wait(semaphore *s) {
    disable_interrupts();
    // acquire guard
    while (test_and_set(&s->guard))
        ;

    if (s->count <= 0) {
        // no resources – block
        enqueue(&s->Q, current_thread());
        s->guard = False;
        block_current_thread();
    }

    // consume one unit
    s->count--;
    // release guard
    s->guard = False;
    enable_interrupts();
}

void signal(semaphore *s) {
    disable_interrupts();
    // acquire guard
    while (test_and_set(&s->guard))
        ;

    if (is_empty(s->Q)) {
        // nobody waiting: increment count
        s->count++;
    } else {
        // wake one waiter
        thread_t *t = dequeue(&s->Q);
        move_to_ready_queue(t);
    }

    // release guard
    s->guard = False;
    enable_interrupts();
}
```