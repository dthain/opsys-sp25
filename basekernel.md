# Bonus Info about Basekernel Basekernel Process Implementation

**(This is not required information, but may be of interest if you want to explore operating system kernels more deeply.)**

[Basekernel](https://github.com/dthain/basekernel) is a small operating system
kernel by Prof. Thain for demonstrating the principles of operating systems.  You can build and try it out yourself in a virtual machine on your laptop, if you like.  Most every principle that we work with in this class can be found in the code of Basekernel:

## Booting

- On an X86 based PC, the CPU starts up by looking for a pointer known as the *reset vector* at a fixed location (`0xfffffff0`) at the top of memory.  At this location, a ROM provides a pointer to the boot-up code contained in the ROM BIOS (Basic Input/Output System).  This boot-up code performs some simple initialization, power-on-self-test (POST), and then starts to load the operating system from disk.  It does so by loading just the first sector (or *bootblock*) from the disk into location `BOOTBLOCK_START` (`0x7c00`) and then executing the first instruction there.
- [bootblock.S](https://github.com/dthain/basekernel/blob/master/kernel/bootblock.S) contains the assembly language code for the basekernel bootblock.  Because the bootblock itself is not large enough to contain code for a disk driver (or any other driver!), it relies upon the ROM BIOS, which contains basic routines for accessing the disk, keyboard, screen, etc.  The bootblock calls the BIOS routines to load more sectors from the disk into memory at `KERNEL_START` (`0x10000`).  Once all sectors are loaded, it jumps there.
- [kernelcore.S](https://github.com/dthain/basekernel/blob/master/kernel/kernelcore.S) contains the starting instructions of the kernel
as well as the low-level interrupt handling routines in assembly.  It performs some further initialization, measures the available memory, probes a suitable video mode, activates protected mode, and then jumps to the C-language entry point `kernel_main`.
- [main.c](https://github.com/dthain/basekernel/blob/master/kernel/main.c) now starts to look like some clean, familiar code.  It initializes each of the kernel modules and drivers in order, sets up the first process, and runs the kernel shell.
- [kshell.c](https://github.com/dthain/basekernel/blob/08be8c66d3dd39ec539ed45c3a38a932617349d6/kernel/kshell.c#L384) is the simple program that runs in kernel mode at boot time to accept commands from the user.  Here you can use a handful of commands like `install`, `format`, or `mount` to set up the system, and then finally `run` to start the first process in user mode.

## Interrupts

- The low-level Interrupt Descriptor Table (IDT) is found at [idt](https://github.com/dthain/basekernel/blob/08be8c66d3dd39ec539ed45c3a38a932617349d6/kernel/kernelcore.S#L466) in `kernelcore.S`.  Each entry in the table jumps to an assembly language routine like [intr00](https://github.com/dthain/basekernel/blob/08be8c66d3dd39ec539ed45c3a38a932617349d6/kernel/kernelcore.S#L349) that may push a value on to the stack, to get a consistent layout.  These routines jump to [intr_handler](https://github.com/dthain/basekernel/blob/08be8c66d3dd39ec539ed45c3a38a932617349d6/kernel/kernelcore.S#L406) which saves registers, sets up a C-compatible stack, and jumps to the C-language function `interrupt_handler`
- [interrupt_handler](https://github.com/dthain/basekernel/blob/08be8c66d3dd39ec539ed45c3a38a932617349d6/kernel/interrupt.c#L129) is a quite simple routine that just calls a function in `interrupt_handler_table` corresponding to the desired interrupt.  Various parts of the kernel code can use `interrupt_register` to arrange for a C-language function to be called for specific interrupts.  These handlers are universially known as `something_interrupt` to make it clear that they are limited.
- For example, [keyboard_init](https://github.com/dthain/basekernel/blob/08be8c66d3dd39ec539ed45c3a38a932617349d6/kernel/keyboard.c#L136) calls `interrupt_register` to set up [keyboard_interrupt](https://github.com/dthain/basekernel/blob/08be8c66d3dd39ec539ed45c3a38a932617349d6/kernel/keyboard.c#L103) as the function to read the next item from the kyeboard.

## System Calls

- [library/syscalls.c](https://github.com/dthain/basekernel/blob/master/library/syscalls.c) contains the user-mode functions for each system call.  Each one gathers arguments and passes it to the generic function `syscall`.
- [library/syscall.S](https://github.com/dthain/basekernel/blob/master/library/syscall.S) contains the assembly-language source for `syscall` which puts arguments into registers, and then forces a trap with the `int 0x80` instruction.
- [kernel/kernelcore.S](https://github.com/dthain/basekernel/blob/master/kernel/kernelcore.S#L466) which points to handlers named `intr00-intr48` that
eventually call C functions `interrupt_handler` and `syscall_handler`.
- [kernel/syscall_handler.c](https://github.com/dthain/basekernel/blob/master/kernel/syscall_handler.c#L554) contains the C function `syscall_handler` which examins the provided arguments, and then calls functions like `sys_process_fork` and `sys_open_file` as appropriate to carry out the request.

## Processes

- [kernel/process.h](https://github.com/dthain/basekernel/blob/master/kernel/process.h) describes the states of a process, and `struct process` is the equivalent of the Process Control Block (PCB).
- [kernel/process.c](https://github.com/dthain/basekernel/blob/08be8c66d3dd39ec539ed45c3a38a932617349d6/kernel/process.c#L22) contains the `current` pointer to the currently-running process, as well as the `ready_list` of processes, and the `grave_list` of completed processes.
- [process_switch](https://github.com/dthain/basekernel/blob/master/kernel/process.c#L22) has the (surprisingly simple) context switch code, which pushes all the current registers, switches the stack pointer, selects a new process, and then pops all the registers.  Note that this (simple) kernel just runs whatever process is next in the list.
- [process_switch](https://github.com/dthain/basekernel/blob/08be8c66d3dd39ec539ed45c3a38a932617349d6/kernel/process.c#L275C1-L275C59) also contains the scheduler, which is nothing more than `list_pop_read(&ready_list)`.  When preemption is disabled, the policy is effectively FCFS.  When preemption is enabled, it is effectively Round Robin.

## Page Tables

Coming soon!

## Virtual Memory

Coming soon!

## I/O Devices

Coming soon!

## Buffer Cache

Coming soon!

## Filesystem

Coming soon!

## Security

Coming soon!

