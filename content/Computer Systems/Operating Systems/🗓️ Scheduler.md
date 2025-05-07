The scheduler/dispatcher is the module that moves threads between queues and states. 
- Let a thread run for a while
- Save its execution state
- Load state of another thread
- Let it run

Scheduler runs when:
- A thread switches from running to waiting or ready
- A thread is terminated
- An interrupt or exception occurs
# Policy vs. Mechanism
```c
yield() {
	thread_t old_thread = current_thread;
	current_thread = get_next_thread(); // <- policy
	append_to_queue(ready_queue, old_thread);
	context_switch(old_thread, current_thread); // <- mechanism
	return;
}
```

**Scheduling Mechanisms**
- Context switching: saving state of old thread and restoring state of new thread
- Thread queues and thread states
- Timer interrupts

**Scheduling Policies**
- Which thread is run next and for how long
# Scheduling Policies
Scheduling algorithm (aka policy) determines which thread to run.
## First-Come First-Served (FCFS) or FIFO
- Schedule jobs in the order they arrive
- Non-preemptive: Run them until completion or they block or yield
- Pros: Simplicity, jobs treated equally, no starvation
- Cons: Average waiting time can be large if short jobs wait behind long jobs
## Shortest Job First (SJF)
- Run the job with the shortest run time first
- Non-preemptive
- Pro: Minimizes average turnaround time if all jobs arrive at the beginning
- Cons: Difficult to predict run times, can't preempt long jobs, and can potentially starve long jobs
## Shortest Remaining Time to Completion First (SRTCF)
- Run the job with the shortest remaining run time first
- Preemptive: Scheduler can interrupt a running job
- Pros: Provably optimal, minimizes average turnaround time
- Cons: Difficult to predict run times, can potentially starve long jobs
## Round Robin
- FIFO, with preemption
- Each job runs for a time slice or quantum (or until it blocks or is interrupted)
- Ready queue is treated as a circular queue
- Pros: Short response time, fair, no starvation
- Cons: Context switches are frequent and can add overhead
## Priority Scheduling
- Assign each job a priority
- Run the job with the highest priority first
	- Use FIFO for jobs with equal priority
- Can be preemptive or non-preemptive
- Pro: Flexibility
- Cons: Starvation (low priority jobs can wait indefinitely), priority is set internally by the OS and externally by users/admin
## Multi-Level Feedback Queues (MLFQ)
- Multiple queues, each with a different priority
- Jobs start at the highest priority queue
- If timeout expires, drop one level
- If timeout doesn't expire, or a job doesn't run => Stay or move up one level
- Pros: Dynamically adapts priorities, no starvation
- Cons: More complex, parameters to tune
### Goals
- Minimize turnaround time
	- Time to complete a job: $T_{\text{turnaround}} = T_{\text{completion}} - T_{\text{arrival}}$
- Maximize throughput
	- Jobs per second
	- Minimize overhead (e.g. of context switches)
	- Use system resources efficiently (CPU, memory, disk, etc.)
- Minimize average response time
	- Time until a job starts: $T_{\text{response}} = T_{\text{firstrun}} - T_{\text{arrival}}$
- **Fairness**
	- No starvation, no deadlock, fair access to CPU
# Starvation
Situation in which a job is prevented from making progress because some other job has the resource it requires (e.g. CPU, lock). Usually a side effect of the scheduling algorithm, such as if a high priority process always prevents a low priority process from running. Can also be a side effect of synchronization, such as a constant supply of readers blocks out any writers.
# Challenges
- Jobs can have different run times
- Jobs can arrival at different times
- Scheduler can interrupt jobs
- Jobs can use other resources besides the CPU (e.g. I/O)
- The runtime of each job may not be known ahead of time
# I/O
Modern time-sharing OSes time slice threads on the ready list
- A CPU-bound thread may use its entire quantum (e.g. 1 ms)
- An IO-bound thread might only use part (e.g. 100 $\micro$s) then issue IO
- The IO-bound thread will go on a wait queue, goes back on the ready list when the IO completes
# Overhead
OS aims to minimize overhead:
- Context switching isn't doing any useful work, just overhead
- Overhead includes making a scheduling decision + context switch

- Typical scheduling quantum: 1 ms
- Typical context-switch time: 1 $\micro$s
# CPU Utilization
The fraction of time the system spends doing useful work. Time doing useful work / total time.
- Quantum of 1 ms + context switch of 1 $\micro$s
# Scheduling in Practice
Challenges:
- Multiple CPU cores
- Scheduling over groups of threads or processes
- Generality; supporting many different kinds of workloads

In Practice:
- MacOS, Windows: Multilevel Feedback Queue
- Linux: Completely Fair Scheduler
### Application Goals
- Batch applications
	- ML training, simulations, etc.
	- Care about high throughput and low turnaround time
- Interactive applications
	- Browser, Zoom, etc.
	- Care about low response time
- All applications want high CPU utilization and fairness to avoid **starvation**