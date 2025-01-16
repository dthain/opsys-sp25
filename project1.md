
# Project 1: System Calls and Error Checking

First, read the [general instructions](general) for assignments. Make sure you are using the correct compiler, flags, and machine.

This project is a warm up assignment for the course. The basic concept is simple: write a program
that continuously monitors and displays the contents of a directory. However, the main challenge of engineering operating systems is dealing with errors and unexpected conditions. Thus, the main focus of this assignment will be on the correct handling of errors. The goals of this project are:

- To review your knowledge of basic C programming.
- To learn the most essential Unix system calls.
- To gain experience in rigorous error handling.

## Directory Watch Tool

Write a C program called `dirwatch` that shows the contents of a directory.
The program will be invoked like this:

```
./dirlist <pathname>
```

It should clear the screen and display information about each entry in that directory,
showing the type, name, size (in bytes), access mode (in octal) and owners name (in text).
Make sure to arrange the display nicely in columns, something like this:

```
Contents of /home/dthain/test on 13:15:03 on 1/18/2025

NAME             SIZE    TYPE MODE    OWNER CONTENTS
--------------------------------------------------------------------------------
program.c        3264 B  file 0644   dthain /* Program 1 for CSE 30341 */
test               32 B  dir  0755   dthain (directory)
homework.txt     7585 B  file 0644   dthain HW1 for CSE 30341 due on 01/01/2029
courses            16 B  link 0600   dthain -> /some/other/dir
--------------------------------------------------------------------------------

total: 2 files, 1 directory, and 1 symlink
```

`dirwatch` should run continuously: after displaying the screen once,
wait three seconds, and then display it again.  Keep going until the user hits Control-C.
This allows you to monitor the contents of a system, and see what happens as new
files are added, removed, renamed, and so forth.

The `CONTENTS` column should summarize the contents of each directory entry,
so you have some idea of what is there.  If the entry is a file, show the first
line of the file.  If the entry is a directory, just show `(directory)`.
If the entry is a symbolic link, show the target of the link.
If the summary contains any non-printable characters or anything that would
mess up the display, replace them with a placeholder character (perhaps `#`).

Once you have the basic functionality working, then fix up your program
so that it always nicely fits the available display space of your terminal window.
Adjust the size of the CONTENTS column smaller or larger to make it fit the available columns.
Likewise, if the listing would run off the bottom of the screen,
then instead display something like `(truncated)` on the last line to
show that not everything is displayed.  If the user resizes the window,
then `dirwatch` should adjust to the new size.
(See below about using `TIOCSWINSZ`.)

**Important**: `dirwatch` should smoothly handle any kind of error condition that you can think of.
If the user gives improper arguments, then exit with a helpful message.
If a directory entry cannot be accessed for whatever reason,
then display as much valid information as possible,
and include a useful error message with `strerror`.

In short, there should be no possible way to invoke the program that results in a segmentation fault, corrupted data, or a silent failure.  `dirwatch` should keep going optimistically and try again to display the directory.  Perhaps something has changed and the next time will work as desired. However, be careful that `dirwatch` does **not** cycle endlessly without a delay:
this is known as "busy waiting" and will consume resources in a way that is disruptive to other users.

## System Calls

To carry out this assignment, you will need to learn about the following system calls, and perhaps others:

```
opendir, closedir, readdir, stat, lstat, readlink
open, read, close, ioctl, getpwuid, strerror, errno, exit
```

Manual pages ("man pages") provide the complete reference documentation for system calls. They are available on any Linux machine by typing man with the section number and topic name. Section 1 is for programs, section 2 for system calls, section 3 for C library functions. For example man 2 open gives you the man page for open.   There are also a variety of online services ([linux.die.net](https://linux.die.net/man/2)) that provide the same information.

As you probably already know, man pages are a little difficult to digest, because they give complete information about one call, but not how they all fit together. However, with a little practice, you can become an expert with man pages. Consider the [man page for open(2)](https://linux.die.net/man/2/open). At the top, it tells you what include files are needed in order to use open:

```
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
```

It also gives the the prototype for several variations of the system call:

```
int open(const char *pathname, int flags);
int open(const char *pathname, int flags, mode_t mode);
int creat(const char *pathname, mode_t mode);
```

The documentation contains many more details than you need, but it is usually a good bet to read the first paragraph, and then skim ahead until you find what you are looking for. It is also always fruitful to read the RETURN VALUE section, and to use the SEE ALSO section to look up related system calls.

## Handling Errors

An operating system (and any program that deals with it) must be vigilant in dealing with errors and unexpected conditions. End users constantly attempt operations that are illegal or make no sense. Digital devices behave in unpredictable ways. So, when you write operating system code, you must always check return codes and take an appropriate action on failure.

How do you know if a system call succeeded or failed? In general read the RETURN VALUE section of the manual page, and it will tell you exactly how success or failure is indicated. However, nearly all Unix system calls that return an integer (open, read, write, close, etc.) have the following convention:
- If the call succeeded, it returns an integer greater than or equal to zero.
- If the call failed, it returns an integer less than zero, and sets the global variable errno to reflect the reason for the error.

And, nearly all C library calls that return a pointer (malloc, fopen, etc.) have a slightly different convention:
- If the call succeeded, it returns a non-null pointer.
- If the call failed, it returns null, and sets the global variable errno to reflect the reason for the error.

The errno variable is simply an integer that explains the reason behind an errno. Each integer value is given a macro name that makes it easy to remember, like EPERM for permission denied. All of the error types are defined in the file /usr/include/asm/errno.h. Here are a few of them:

```
#define EPERM            1      /* Operation not permitted */
#define ENOENT           2      /* No such file or directory */
#define ESRCH            3      /* No such process */
#define EINTR            4      /* Interrupted system call */
...
```

You can check for specific kinds of errors like this:

```
fd = open(filename,O_RDONLY,0);
if(fd<0) {
    if(errno==EPERM) {
        printf("Error: Permission Denied\n");
    } else {
        printf("Some other error.\n");
        ...
    }
    exit(1);
}
```

This would get rather tedious with 129 different error messages. Instead, use the strerror routine to convert errno into a string, and print out a descriptive message like this:

```
fd = open(filename,O_RDONLY,0);
if(fd<0) {
     printf("Unable to open %s: %s\n",filename,strerror(errno));
     exit(1);
}
```

## Some Hints

- You may need to put `#define _GNU_SOURCE` at the top of your programs (above the `#includes`)
in order to access all of the relevant system calls.
- See the `stat(2)` man page and read about the macros `S_ISDIR`, `S_ISREG`, etc. which determine
whether an entry is a directory, file, etc...
- Note the difference between `stat(2)` and `lstat(2)`: one follows symbolic links and the other does not.
- See `printf(3)` for many different ways of arranging output, justifying left and right, etc.
- See `isprint(3)` for a listing of many functions that determine whether a character is printable, whitespace, numeric, etc...
- See `getpwuid(3)` for converting an integer `uid` into information about a user.
- See `TIOCSWINSZ(2)` to get the current terminal window size using `ioctl`

## Testing

Make sure to test your program on a wide variety of conditions.
Start `dirwatch` in one window.
From another window, add/remove/modify files in the watched directory,
Resize the window many different ways.
Induce a variety of error conditions to ensure `dirwatch` responds appropriately.
Repeat for a variety of directories.

## Grading

Your grade will be based on:
- Basic functionality of `dirwatch`. (60%)
- Correct handling of all error conditions. (30%)
- Good coding style, such as clear formatting, sensible variable names, and useful comments. (10%)

## Turning In

This assignment is due on Friday, January 24th at 5:00PM.

Review the [general instructions](general) for assignments.

Please turn in only the source code file `dirwatch.c` and a `Makefile` that builds the executable. Do not turn in executables or other files, since those just take up space, and we will build your code from source anyway.

## Things to Try for Fun

- Figure out how to display colors and special characters to make a more appealing display.
- Update your display to include system information like memory in use, CPU core load, etc...
- Catch the SIGWINCH signal to detect window size changes, and adjust the display immediately.
- Learn about the [inotify](https://man7.org/linux/man-pages/man7/inotify.7.html) interface to be notified about file system changes.
