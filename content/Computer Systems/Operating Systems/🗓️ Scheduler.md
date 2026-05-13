the scheduler/dispatcher is the module that moves threads between queues and states.
- let a thread run for a while
- save its execution state
- load state of another thread
- let it run

scheduler runs when:
- a thread switches from running to waiting or ready
- a thread is terminated
- an interrupt or exception occurs
# policy vs. mechanism
```c
yield() {
	thread_t old_thread = current_thread;
	current_thread = get_next_thread(); // <- policy
	append_to_queue(ready_queue, old_thread);
	context_switch(old_thread, current_thread); // <- mechanism
	return;
}
```

**scheduling mechanisms**
- context switching: saving state of old thread and restoring state of new thread
- thread queues and thread states
- timer interrupts

**scheduling policies**
- which thread is run next and for how long
# scheduling policies
scheduling algorithm (aka policy) determines which thread to run.
## first-come first-served (fcfs) or fifo
- schedule jobs in the order they arrive
- non-preemptive: run them until completion or they block or yield
- pros: simplicity, jobs treated equally, no starvation
- cons: average waiting time can be large if short jobs wait behind long jobs
## shortest job first (sjf)
- run the job with the shortest run time first
- non-preemptive
- pro: minimizes average turnaround time if all jobs arrive at the beginning
- cons: difficult to predict run times, can't preempt long jobs, and can potentially starve long jobs
## shortest remaining time to completion first (srtcf)
- run the job with the shortest remaining run time first
- preemptive: scheduler can interrupt a running job
- pros: provably optimal, minimizes average turnaround time
- cons: difficult to predict run times, can potentially starve long jobs
## round robin
- fifo, with preemption
- each job runs for a time slice or quantum (or until it blocks or is interrupted)
- ready queue is treated as a circular queue
- pros: short response time, fair, no starvation
- cons: context switches are frequent and can add overhead
## priority scheduling
- assign each job a priority
- run the job with the highest priority first
	- use fifo for jobs with equal priority
- can be preemptive or non-preemptive
- pro: flexibility
- cons: starvation (low priority jobs can wait indefinitely), priority is set internally by the os and externally by users/admin
## multi-level feedback queues (mlfq)
- multiple queues, each with a different priority
- jobs start at the highest priority queue
- if timeout expires, drop one level
- if timeout doesn't expire, or a job doesn't run => stay or move up one level
- pros: dynamically adapts priorities, no starvation
- cons: more complex, parameters to tune
### goals
- minimize turnaround time
	- time to complete a job: $T_{\text{turnaround}} = T_{\text{completion}} - T_{\text{arrival}}$
- maximize throughput
	- jobs per second
	- minimize overhead (e.g. of context switches)
	- use system resources efficiently (cpu, memory, disk, etc.)
- minimize average response time
	- time until a job starts: $T_{\text{response}} = T_{\text{firstrun}} - T_{\text{arrival}}$
- **fairness**
	- no starvation, no deadlock, fair access to cpu
# starvation
situation in which a job is prevented from making progress because some other job has the resource it requires (e.g. cpu, lock). usually a side effect of the scheduling algorithm, such as if a high priority process always prevents a low priority process from running. can also be a side effect of synchronization, such as a constant supply of readers blocks out any writers.
# challenges
- jobs can have different run times
- jobs can arrival at different times
- scheduler can interrupt jobs
- jobs can use other resources besides the cpu (e.g. i/o)
- the runtime of each job may not be known ahead of time
# i/o
modern time-sharing oses time slice threads on the ready list
- a cpu-bound thread may use its entire quantum (e.g. 1 ms)
- an io-bound thread might only use part (e.g. 100 $\micro$s) then issue io
- the io-bound thread will go on a wait queue, goes back on the ready list when the io completes
# overhead
os aims to minimize overhead:
- context switching isn't doing any useful work, just overhead
- overhead includes making a scheduling decision + context switch

- typical scheduling quantum: 1 ms
- typical context-switch time: 1 $\micro$s
# cpu utilization
the fraction of time the system spends doing useful work. time doing useful work / total time.
- quantum of 1 ms + context switch of 1 $\micro$s
# scheduling in practice
challenges:
- multiple cpu cores
- scheduling over groups of threads or processes
- generality; supporting many different kinds of workloads

in practice:
- macos, windows: multilevel feedback queue
- linux: completely fair scheduler
### application goals
- batch applications
	- ml training, simulations, etc.
	- care about high throughput and low turnaround time
- interactive applications
	- browser, zoom, etc.
	- care about low response time
- all applications want high cpu utilization and fairness to avoid **starvation**