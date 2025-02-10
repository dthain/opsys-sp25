# Project 3: Thread Parallelism

Please review the [general instructions](general) for assignments.

The goals of this project are:
- To learn how to exploit fine-grained parallelism with threads.
- To identify and protect critical sections in concurrent programs.
- To understand the inherent performance tradeoffs of multi-threading.

Linear algebra lies at the core of many applications of computing:
process optimization, machine learning, and computer graphics all
depend on efficient manipulation of large arrays and matrices.
Let's take a simple example of a O(n^2) matrix operation and see
how fast we can make it go.

This project will require writing some small amounts of code and
evaluating the performance.  You will write a short lab report detailing
your approaches and the performance achieved.

The **key idea** here is that threads are most effective when used
to operate on large, independent pieces of data, and only **occasionally**
synchronize when necessary.  The problem is that the synchronization is
the tricky part!

## Part 1: Sequential Baseline

Write a C program called `seqperf.c` that does the following:

- Declare two large integer arrays (A and B) of the same size, given on the command line
- Fill both arrays with random numbers.
- Compute the sum of the product of each pair of elements in A and B.
(e.g. `a0*b0 + a0*b1 + a0*b2 ... + a1*b0 + a1*b1 + a1*b2 ... an*bn).
- Print out the total value, and the time needed to compute the sum, with millisecond accuracy.

Experiment with the size of the array a little bit until the
time to compute the sum takes between ten and twenty seconds.
(The specific size doesn't matter as long as you use it consistently going forward.)

**Note 1:**  You will probably get a different total each time, because the random
number generator gives you a difference sequence of numbers.  Use `srand` at the beginning of
your program to fix the random seed and get consistent results for a given array size.)

**Note 2:**  The sum will certainly overflow and wrap around, resulting in
negative values some of the time.  This is still fine as long as the result is consistent.

## Part 2: Accelerate with Threads

Make a copy of the previous program, and call the new one `threadperf.c`

Modify `threadperf` to do the same task, but this time, split the work
into N threads.  Each thread should work on a disjoint part of the task,
producing a partial sum, which is then added together at the end to
produce a grand total.

The size of the arrays and the number of threads should be given as command line arguments,
and your program should print out the number of threads, the size
of the array, the total sum, and the time to compute the sum, something like this:  (example numbers are made-up)

```
% ./threadperf 128000 4
size 128000 threads 4 sum 12345678 time 15.425s
```

**Important:** The total sum is your assurance that the computation
is being done correctly!  If you don't get the same number each time
(and the same as in Part 1) then something is wrong with your code.

Once this is working, run the program with one thread, two threads, etc.
and record the performance achieved in each step.  Keep going until the
performance flattens out.  In fact, keep going until the performance gets worse!
Keep notes on this for the lab report, below.

## Part 3: Synchronize on Output

Make a copy of the previous program, and call the new one `progressperf.c`

This final version should perform the same computation as the previous one,
but with one twist: it should produce a (single) progress bar
that goes from 0 to 100 percent incrementally as the total work is accomplished.

You can format the progress bar how you like, but something simple like this would be fine:

```
37% [==============>
```

This is trickier than it first sounds, because the threads must have a little bit of
shared data in order to properly track progress.  And of course that shared data
must be protected so that you don't suffer from race conditions.  The bar
should proceed smoothly, printing each value between 0 and 100 as work is done.
However, you don't want to print out identical values unnecessarily, because that will
slow down execution.

This will require some thought to puzzle through, and you may need to try
some different approaches until you have a good solution.

Repeat your performance measurements in the same way as Part 2.
The performance of `progressperf` should not be substantially slower than `threadperf`.
If it is, reconsider your method of synchronization and try a different approach.
Keep your notes for the lab report.

## Lab Report

Create a text document `labreport.txt` that details the performance and behavior of your code:

0. Note specifically which student server on which you compiled and ran your code.
(You can use any one of the servers, but must use the same one for all tests.)
1. Show the command line and output of `seqperf` that yields a runtime of 10-20s.
2. How many cells/second does `seqperf` compute?  What is the average time to compute one cell?  (As always, put your answer in the form of a small number (1-1024) and a metric suffix with appropriate units.)
3. Show the command line and output of `threadperf` for a varying number of threads.
4. Which configuration yields the fastest performance?  Explain why.
5. Which configuration starts to show **worse** performance as you add threads?  Explain why.
6. Show the command-line and output of `progressperf` for the same range of threads as `threadperf`.
7. Explain your strategy for performing synchronization in `progressperf`.
8. Explain the difference in performance (if any) between `threadperf` and `progressperf`.

## Hints

- Use `gettimeofday` around lines of code to measure the elapsed time.
- Use the `pthreads` library to manage threads and perform locking.  You **will** need to use mutexes to solve this problem.  You will **not** need to use condition variables.  (that's in the next assignment)
- You don't need any special libraries to display the progress bar: `printf` is sufficient.  Try using `\r` instead of `\n` to update the current output line, instead of scrolling.
- On the CSE student machines, performance may vary due to the number of people logged in.  (And it will probably get worse closer to the due date!)  It would be wise to run each test several times, and then take the fastest time achieved.

## What to Turn In

Review the [general instructions](general) on how to submit to your dropbox.

- `seqperf.c`, `threadperf.c`, `progressperf.c` and a `Makefile` that builds all three from scratch.
- `labreport.txt` containing your answers to the questions above.

## Grading

Your grade will be based on:

- Correct sequential implementation. (10%)
- Correct multi-thread implementation with good performance. (30%)
- Correct progress bar implementation with good performance. (30%)
- Lab report details. (20%)
- Good coding style, including clear formatting, sensible variable names, and useful comments. (10%)

## Turning In

This assignment is due on Friday, February 21st at 5:00PM.
