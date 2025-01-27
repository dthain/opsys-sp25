# Project 2: Processes and the Shell

Please review the [general instructions](general) for assignments.

The goals of this project are:
- To learn the relationship between the kernel, the shell, and user-level programs.
- To learn how to employ the Unix process management system calls.
- To gain more experience in rigorous error handling.

## Essential Requirements

You will write a program called `myshell` which is capable of executing, managing, and monitoring user level programs. This program will be similar in purpose and design to everyday shells like bash or tcsh, although the syntax will be slightly different. myshell will be invoked without any arguments, and will support several different commands.

Your shell should print out a suitable prompt like `myshell>` when it is ready to accept input.
It must read a line of input, accepting several possible commands:

- list
- chdir 
- pwd
- start
- wait
- waitfor
- kill
- run
- array

The first few command in this list will be "built in" commands in which the shell will do the actual work.  The `list` command should cause the shell to list the entries in the current directory, similar to the `dirwatch` program from the previous assignment.  (No need to include the file contents or the window measurements.)  Go right ahead and adapt the code that you already wrote, being sure to fix any bugs or problems.  For example:

```
myshell> list
NAME             SIZE    TYPE MODE OWNER 
-----------------------------------------
program.c        3264 B  file 0644 dthain
test               32 B  dir  0755 dthain
homework.doc     7585 B  file 0644 dthain
courses            16 B  link 0600 dthain
-----------------------------------------
```

If `list` is given an argument, it should list that directory instead:
```
myshell> list /etc
NAME             SIZE    TYPE MODE OWNER 
-----------------------------------------
hosts             150 B  file 0644 root
passwd           3846 B  file 0644 root
...
-----------------------------------------
```

The `chdir` command should cause the shell to change its working directory to the named directory:

```
myshell> chdir /tmp
```

The `pwd` command should cause the shell to print the current working directory:

```
myshell> pwd
/escnfs/home/dthain
```

The next few commands will focus on creating and managing sub-processes that execute external programs.

The `start` command should start another program with command line arguments, print out the process number of the running program, and then go back to accepting input.  For example:

```
myshell> start cp data.txt copy.txt
myshell: process 346 started
myshell> 
```

The `wait` command causes the shell to wait for **any** child process to exit. When this happens, indicate whether the exit was normal or abnormal, along with the exit code or signal number and name, respectively. Display any errors encountered. For example:

```
myshell> wait
myshell: process 502 exited normally with status 5

myshell> wait
myshell: process 347 exited abnormally with signal 11: Segmentation fault.

myshell> wait
myshell: No children.
```

The `waitfor` command should do the same thing, but waits for a *specific* child process to exit:

```
myshell> waitfor 346
myshell: process 346 exited normally with status 0

myshell> waitfor 346
myshell: No such process.
```

The `kill` command should send signal 9 (SIGKILL) to a specific process.

```
myshell> kill 346
myshell: process 346 has been killed

myshell> kill 346
myshell: unable to kill process 346
```

The `run` command should combine the behavior of `start` and `waitfor`. `run` should start a program, wait for that particular process to finish, and print the exit status. For example:

```
myshell> run date
Mon Jan 27 11:51:57 EST 2025
myshell: process 348 exited normally with status 0
```

The `array` command should start multiple copies of the same command concurrently,
and then wait for all of them to complete.
The first argument to `array` is the number of copies to run, and the remainder
of the arguments describe the command to run.  If one of the arguments to the command
is exactly `@` then replace that argument with its position in the array.

For example, this command:

```
myshell> array 8 /bin/echo hello @
```

should run `/bin/echo` eight times: `/bin/echo hello 0`, `/bin/echo hello 1`, ... , `/bin/echo hello 7`.  And the output should look something like this:

```
hello 0
myshell: process 372 exited normally with status 0
hello 2
hello 1
myshell: process 374 exited normally with status 0
myshell: process 373 exited normally with status 0
...
```

After each shell command completes, your program must continue to print a prompt and accept another line of input. The shell should exit with status zero if the command is `quit` or `exit` or the input reaches end-of-file. If the user types a blank line, simply print another prompt and accept a new line of input. If the user types any other command, the shell should print a reasonable error message:

```
myshell> bargle ls -la
myshell: unknown command: bargle
```

To keep things simple, your shell doesn't need to deal with arbitrarily long commands. It must accept input lines of up to 1024 characters, and must handle up to 128 distinct words on a line.

## Hints

You will need to read the manual pages for the following system and library calls, and possibly others:

```
fgets, printf, strtok, strsignal, opendir, readdir, closedir, stat, chdir, getwd, fork, execvp, wait, waitpid, kill, exit, signal
```

- Use `fgets` to read one line of text after printing the prompt. Note that if you `printf` a prompt that has no newline on the end, it will not immediately display. Call `fflush(stdout)` to force the output.

- Breaking the input line into separate word is a little tricky, but is only a few lines of code once you get it right. Call `strtok(line," \t\n")` once to obtain the first word, and then `strtok(0," \t\n")` repeatedly to get the rest, until it returns null. Declare an array of pointers char `*words[100]`, then, for each word found by `strtok`, save a pointer to the word in `words[i]`. Keep track of the number of words as `nwords`, then set `words[nwords] = 0;` when you have found the last one.

- Once you have broken the input line into words, you can check `words[0]` for the command name, use `strcmp` to check for string equality and `atoi` to convert a string to an integer.

- Make sure to stop if `fgets` returns null, indicating end-of-file. This allows you to run myshell and read commands from a file. For example, if you create myscript with the following contents:

```
start ls
wait
start date
wait
```

Then, you can run the shell on that input like this:

```
./myshell < myscript
```

## Testing

Make sure to test your program on a wide variety of conditions. Try running multiple programs simultaneously. Create some simple programs that crash or exit with values other than zero, to make sure that `wait` and run report the correct exit status.

Make sure to carefully handle all possible error conditions. Every system call can fail in a number of ways. You must cleanly handle all possible errors with a reasonable error message, as discussed in Project 1. It is up to you to read the man pages carefully and learn what errors are possible.

## Grading

Your grade will be based on:

- Correct functioning of the shell according to the specification. (50%)
- Correct handling of all possible error conditions. (40%)
- Good coding style, including clear formatting, sensible variable names, and useful comments. (10%)

## Turning In

This assignment is due on Friday, February 7th at 5:00PM.

Review the [general instructions](general) for assignments.

Please turn in only the source code files `myshell.c` and a `Makefile` that builds the executable. Do not turn in executables or other files, since those just take up space, and we will build your code from source anyway.
