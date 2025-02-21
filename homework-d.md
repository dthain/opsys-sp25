# Homework D - Synchronization

## Synchronization Background

As we discussed in class, a monitor is a code-organization strategy for writing safe concurrent programs.
A monitor consists of a data structure, a mutex, a condition variable,
and one or more methods to manipulate the data structure.
Each method must lock/unlock the mutex, and may wait for or wakeup other threads.

Concurrent programming is trickier than it first appears:
it is all too easy to write a program that appears to be correct, but has a subtle bug.
Here is a foolproof recipe to write simple and correct monitor code:

- Always lock/unlock at the beginning/end of each method.
- Broadcast whenever a covering condition is triggered.
- After sleeping, consider the wakeup as a hint that your condition might be satisfied. Check again to be sure. 

In this homework, you are going to practice messing around with several synchronization problems.
Each of these requires **only a few lines of code** but each requires some subtlety to get right.
Start by downloading the `opsys-sp25-examples` repository to get started:

```
git clone https://github.com:dthain/opsys-sp25-examples
```

Build the examples, and try them out to see what happens:
```
cd opsys-sp25-examples
make
./monitor-bank
./sem-rw
./sem-dining
...
```

## The Readers-Writers Problem

We discussed the readers-writers problem in class, and developed a solution based on semaphores.
Now, solve the problem using a monitor instead.  Use the (incomplete) example code `monitor-rw.c`
as a starting point, and fill out the missing functions `read_lock`, `read_unlock` and so forth.  Test your solution by reading the output
carefully, making sure that only one writer is admitted at a time, and only when no readers are present.

## Avoiding Starvation

The most straightfowrard solution to the readers-writers problem is subject to *starvation*.  If there are enough reader
threads waiting, then writer threads will be delayed indefinitely, only succeeding once all the readers have completed.
Copy `monitor-rw.c` to `monitor-rw-nostarve.c` and modify the latter to avoid starvation.  Test your solution by reading the output
carefully, making sure that writers have an opportunity to enter, at least periodically.

## The Shared GPU Problem

Here is a new synchronization problem to think about. Suppose you have a large number of threads that each want to run an inference function on a GPU.
Each inference function requires a large model dataset to be loaded in to the GPU.
The GPU is capable of running up to four threads at once, but only if those threads are all using
the same model dataset.  Use `monitor-gpu.c` as a starting point.
Modify `gpu_enter` and `gpu_exit` appropriately to ensure that only (up to four) threads using the same dataset are admitted at once.
As above, test your solution and look at the output carefully.

## The Dining Philosophers

The Dining Philosophers is usually described as a problem that makes use of semaphores,
because the obvious solution results in deadlock.  But like many problems, a good solution
is more naturally expressed using a monitor.  Using `monitor-dining.c` as a starting point,
write a solution to the problem that makes use of a monitor.  As above, test and examine the output carefully.

## Present Convincing Evidence!

As we have discussed in class, synchronization problems are subtle and can fail in various ways.
You should always assume a solution is incorrect until proven otherwise.
Write a plain text document `evidence.txt` that convinces the reader that your solutions above are correct.
Explain how each one works, and present a sample of the output, demonstrating that each one behaves as intended.

## Turning In

Review the [general instructions](general) for turning in code.

Turn in your code to the `homework-d` directory in your dropbox.

Make sure to include:
- `monitor-rw.c`
- `monitor-rw-nostarve.c`
- `monitor-gpu.c`
- `monitor-dining.c`
- `evidence.txt`
- `Makefile` that builds the three programs.
