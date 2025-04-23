A programming language construct that controls access to shared data while protecting its data from unstructured access. Synchronization code is added by the compiler, enforced at runtime.

Encapsulates:
- Shared data structures
- Procedures that operate on the shared data structures
- Synchronization between concurrent threads that invoke the procedures

Guarantees that threads accessing its data through its procedures interact only in legitimate ways.
# Semantics
- Guarantees mutual exclusion, only one thread can execute any monitor procedure at any time (thread is "in the monitor")
- Threads can use [[🔮 Condition Variables]] within a monitor, if a thread blocks within a monitor, another one can enter
# Producer-Consumer
- Locking is implicit, compiler adds the code.
- Equivalent to each procedure in the monitor calling `acquire()` on entry and `release()` on exit.

```c
Monitor producer_consumer {
	Condition not_full;
	Condition not_empty;
	
	void put_resource() {
		...
		wait(not_full);
		...
		signal(not_empty);
	}
	
	void get_resource() {
		...
		wait(not_empty);
		...
		signal(not_full);
	}
} 
```