# Project 3: Parallel Threads

Please review the [general instructions](general) for assignments.

The goals of this project are:
- To learn how to exploit fine-grained parallelism with threads.
- To identify and protect critical sections in concurrent programs.
- To understand the inherent performance tradeoffs of multi-threading.

## Part 1: Sequential Baseline

Write a C program called `seqperf.c` that does the following:

- Declare a large array of integers.  (size given as a command line argument)
- Fill the array with random numbers.
- Sum up the values of the array.
- Print out the total value, and the time needed to compute the sum.

Now, experiment with the size of the array a little bit until the
time to compute the sum takes between ten and twenty seconds.
(The precise size doesn't matter as long as you use it consistently.)

**Note 1:**  You probably get a different total each time, because the random
number generator gives you different numbers.  Use `srand` at the beginning of
your program to fix the random seed and get consistent results for a given array size.)

**Note 2:**  The sum will certainly overflow and wrap around, resulting in
negative values some of the time.  This is still fine as long as the result is consistent.

## Part 2: Accelerate with Threads

Make a copy of the previous program, and call the new one `threadperf.c`

Modify your program to do the same task, but this time, split the work
into N threads.  Each thread should work on a disjoint part of the array,
producing a partial sum, which is then added together at the end to
produce a single total.

The size of the array and the number of threads should be given as command line arguments,
and your program should print out the number of threads, the size
of the array, the total sum, and the time to compute the sum, something like this:  (numbers are made-up)

```
% ./threadperf 128000 4
size 128000 threads 4 sum 12345678 time 15.42s
```

**Important: The total sum is your assurance that the computation
is being done correctly!  If you don't get the same number each time
(and the same as in Part 1) then something is wrong with your algorithm!**

Once this is working, run the program with one thread, two threads, etc.
and record the performance achieved in each step.  Keep going until the
performance flattens out.  In fact, keep going until the performance gets worse!
Keep notes on this for the lab report, below.

## Part 3: Synchronize on Output

Make a copy of the previous program, and call the new one `progressperf.c`

This final version should perform the same computation as the previous one,
but with one interesting twist: it should produce a (single) progress bar
that goes from 0 to 100 percent incrementally as the total work is accomplished.

You can format the progress bar how you like, but something simple like this would be ok:

```
37% [==============>
```

This is a little trickier than it first sounds, because the threads must have a little bit of
shared data in order to properly track progress.  And of course that shared data
must be protected so that you don't suffer from race conditions.  On the other hand,
if too much synchronization happens, the program will be unnecessarily slowed down.

Then, repeat your performance measurements in the same way as Part 2.  The performance of `progressperf`
should not be substantially slower than `threadperf`.
If it is, reconsider your method of synchronization and try a different approach.
Keep your notes for the lab report.

## Lab Report

Create a text document `labreport.md` that details the performance and behavior of what you have written:

- Show the command-line and output of `seqperf` that yields a runtime of 10-20s.
- Show the configuration and output of `threadperf` for a varying number of threads.
- Highlight the configuration that yields the fastest performance.
- Highlight the point at which adding threads makes the performance **worse**.
- Explain why the performance gets worse at some point.
- Show the configuration and output of `progressperf` for the same range of threads as `threadperf`
- Explain your strategy for performaning synchronization in `progressperf`.
- If the performance of `progressperf` differs from `threadperf`, explain why you think this is so.

This does not need to be a long, elaborate document: just capture the facts, arrange things neatly,
and explain yourself succinctly.

## Hints

- Use `gettimeofday` around lines of code to measure the elapsed time.
- Use the `pthreads` library to manage threads and perform locking.  You **will** need to use mutexes to solve this problem.  You will **not** need to use condition variables.  (that's in the next assignment)
- You don't need any special libraries to display the progress bar: `printf` is sufficient.  Try using `\r` instead of `\n` to update the current output line, instead of scrolling.
- On the CSE student machines, performance may vary due to the number of people logged in.

## What to Turn In

- `seqperf.c`, `threadperf.c`, `progressperf.c` and a `Makefile` that builds both correctly.
- `labreport.md` containing your (plain text) answers to the questions about.

## Grading

Your grade will be based on:

- Correct sequential implementation. (10%)
- Correct multi-thread implementation with good performance. (30%)
- Correct progress bar implementation with good performance. (30%)
- Lab report details. (20%)
- Good coding style, including clear formatting, sensible variable names, and useful comments. (10%)

## Turning In

This assignment is due on Friday, February 21st at 5:00PM.

Review the [general instructions](general) for assignments.

