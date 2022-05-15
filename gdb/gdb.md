# GDB

GDB stands for GNU DeBugger which helps you to debug your binary object file created in compilation process.

GDB allows to you see what is happening in your program which really helps much when program crashes, especially when segmentation fault occurs.

Some uses of gdb:

* Step through a program line by line
* Set breakpoints that will stop your program
* Make your program stop on specified conditions
* Show the present values of variables
* Examine the contents of any frame on the call stack

## Languages supported by GDB

Ada, Assembly, C, C++, D, Fortran, Go, Objective-C, OpenCL, Modula-2, Pascal, Rust

## Installing gdb

To install gdb, run the following command on the terminal.

```bash
sudo apt-get install gdb
gdb --version
```

If no debugging symbols are found, gdb cannot debug the code.

To generate the debugging symbols, compile the source code using the `-g` flag as shown below. `-g` option tells the compiler to store symbol table information in the executable. The size of the executable with increases due to the presence of debugging symbols generated during compilation.

```bash
gcc source.cpp -o source -g
```

## Launching gdb

To launch gdb with a binary object file, run the following command. It loads the debugging symbols into the memory. It launches the gdb terminal where gdb commands can be executed.

```bash
% gdb
(gdb) file ./source
```

or directly

```bash
gdb ./source
```

Using `--quiet` or just `-q` option with `gdb` command doesn't show the unnecessary version information while launching gdb.

## Different ways of passing  command-line arguments

You can run gdb with `--args` parameter,

```bash
gdb --args executablename arg1 arg2 arg3
```

```bash
gdb ./a.out
(gdb) r arg1 arg2 arg3
```

## Commands

`run` command starts the execution of the file. If no breakpoints are placed, it will run till the end. Shortcut for `run` is `r`.

```shell
(gdb) r
```

`help` command provides more information about a command. In case of confusion, just use `help`.

```gdb
(gdb) help <command>
```

Breakpoints can be used to stop the program run in the middle, at a designated point.

To place a breakpoint, run the `breakpoint` command or just `b`.

Whenever gdb gets to a breakpoint it halts execution of your program and allows you to examine it.

Simplest way of putting a breakpoint is using the function name or a line number.

```gdb
(gdb) b <line no>
(gdb) b 3
```

```gdb
(gdb) b <function name>
(gdb) b main
(gdb) b file.cpp:fcnName  # In case of multiple source files.
```

`info` command provides information regarding different aspects. Shortcut is `i`.

```gdb
(gdb) info breakpoints  # lists all the breakpoints/watchpoints.
(gdb) info variables  # lists all the static/global variables.
(gdb) info locals  # lists all the local variables in the current stack frame.
(gdb) info functions  # lists all the functions in the current file.
(gdb) info registers  # lists all the CPU register values.
(gdb) info registers eax  # shows only the eax register value.
(gdb) info frame 1  # lists the information about the stack frame 1.
(gdb) info args  # lists the arguments of the current stack frame.
```

`list` command displays the source code around the current point of execution.

```gdb
(gdb) list - displays 10 lines of code.
(gdb) list 12 - displays the code from line 12.
(gdb) list 10,25 - displays the code from lines 10-25.
(gdb) list <function name> - displays the function code.
```

`bt` command lists the currently active stack frames that enables debugging across functions. frame 0 is the current function. The function that called it is the frame 1 and so on.

`frame` command lets you move from one frame to another. Moving to the other frames enables to view the variables in that function.

```gdb
(gdb) frame <frame number>
(gdb) frame 1
```

`print` command prints the value of the variable. Shortcut is `p`.

```gdb
(gdb) print<format> var
(gdb) print i
(gdb) print &i
(gdb) print sizeof(i)
(gdb) print sizeof(&i)
(gdb) print/x var - prints the value in hexadecimal format.
```

`ptype` command prints the type of the variable.

```gdb
(gdb) ptype var
type = int
```

## Examining the memory

The `x` command examines memory, starting at a particular address. Use `x` to see how it looks under the hood.

It comes with a number of formatting commands that provide precise control over

* how many bytes you’d like to examine and
* how you’d like to print them

```gdb
(gdb) x &i

(gdb) x/4xb &i
```

The flags indicate that I want to examine 4 values formatted as the 'x' numerals, one 'b'yte at a time. On Intel machines, bytes are stored in “little-endian” order.

`set` command sets the variable to a given value.

```gdb
(gdb) set var 2
```

`next` command moves to the next line within the same function. Shortcut is `n`.

`continue` command continues the execution till the next breakpoint. If no breakpoint is encountered, it runs till the end. Shortcut is `c`.

`step` command steps inside the function in the current line. Shortcut is `s`.

`finish` command steps out of the current function. Shortcut is `f`.

`quit` command quits the gdb debugging session. Shortcut is `q`.

Typing `step` or `next` a lot of times can be tedious. If you just press `ENTER`, gdb will repeat the same command you just gave it.

Placing Watchpoints

Watchpoints are placed on variables. On giving `watch` comamnd, whenever the variable value changes, the debugger pauses the execution.

This is similar to the breakpoints, except that breakpoints are placed on functions or lines, whereas watchpoints are placed on variables.

```gdb
(gdb) watch var1 - Pause when var1 value is modified.
(gdb) rwatch var1 - Pause when var1 is read.
(gdb) awatch var1 ` Pause when var1 is either read or written.
```

`disable` command disables the watchpoint/breakpoint. This can be noticed by observing the enable column in the list of breakpoints obtained by `info` command.

```gdb
(gdb) disable <watchpoint/breakpoint number>
```

`delete` command deletes the provided watchpoint/breakpoint number. This deletes the breakpoint from the list.

```gdb
(gdb) delete <watchpoint/breakpoint number>
```

`start` command is the combination of the following two comamands.

```gdb
(gdb) b main
(gdb) run
```

## Conditional Breakpoints

As long as a breakpoint is enabled, the debugger always stops at that breakpoint.

However, sometimes it's useful to tell the debugger to stop at a break point only if some condition is met, like the when a variable has a particularly interesting value.

You can specify a break condition when you set a breakpoint by appending the keyword if to a normal break statement

```gdb
(gdb) break [position] if expression
```

In the above syntax position can be a function name or line number.

If you already set a breakpoint at the desired position, you can use the condition command to add or change its break condition:

```gdb
(gdb) condition bp_number [expression]
```

## GDB Text User Interface (TUI)

The gdb Text User Interface (TUI) is a terminal interface which uses the curses library to show the

* source file
* assembly output
* program registers
* and GDB Commands

in separate text windows

The TUI mode is supported only on platforms where a suitable version of the curses library is available.

The TUI mode is enabled by default when you invoke gdb as either `gdbtui` or `gdb -tui`.

You can also switch in and out of TUI mode while gdb runs by using various TUI commands and key bindings, such as Ctrl-x a

`Ctrl - l` allows you to repaint the screen //when there are printf's displayed.

You can type commands on the command line like usual, but the arrow keys will scroll the source code view instead of paging through history or navigating the entered command.

To switch focus to the command line view, type ctrl-x o and the arrow keys works as in the normal command line mode.

Switching back to the source code view is done using the same key combination a second time.

## Logging

You may want to save the output of gdb commands to a file. Useful when you’re dealing with a long stack trace, or a multi-threaded stack trace.

There are several commands to control gdb’s logging.

`set logging on`

* Enable logging.
* GDB saves all output from this point in a text file called `gdb.txt` that resides in the directory in which you are running GDB.

`set logging off`

* Disable logging.
* Note that you can turn logging on and off several times and GDB will concatenate output to the gdb.txt file

`set logging file file`

* Change the name of the current logfile. The default logfile is `gdb.txt`.

## Debugging an existing process

`attach <process-id>` attaches to a running process — one that was started outside gdb.

`detach` detaches the attached process from gdb.

## `disassemble` command

Another useful command of gdb debugger is the `disassemble` command. As its name suggests, this command helps in disassembling of provided function assembler codes.

For instance, if we want to disassemble `main` function. we just need to type

```gdb
(gdb) disassemble main
```

`command breakpoint-number` specifies commands to run whenever the breakpoint is reached.

Type commands for when breakpoint 2 is hit, one per line. End with a line saying just "end".

```gdb
(gdb) command 2
> p loop
> end
```
