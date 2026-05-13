a programming language construct that controls access to shared data while protecting its data from unstructured access. synchronization code is added by the compiler, enforced at runtime.

encapsulates:
- shared data structures
- procedures that operate on the shared data structures
- synchronization between concurrent threads that invoke the procedures

guarantees that threads accessing its data through its procedures interact only in legitimate ways.
# semantics
- guarantees mutual exclusion, only one thread can execute any monitor procedure at any time (thread is "in the monitor")
- threads can use [[🔮 Condition Variables|🔮 condition variables]] within a monitor, if a thread blocks within a monitor, another one can enter
# producer-consumer
- locking is implicit, compiler adds the code.
- equivalent to each procedure in the monitor calling `acquire()` on entry and `release()` on exit.

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