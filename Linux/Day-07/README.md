
<div id="top"></div>

<p align="center">
  <a href="https://github.com/mammaddrik/RootBound">
    <img src="https://i.postimg.cc/pVDQmRGL/Root-Bound-Day-07.png" alt="RootBound-Day-07">
  </a>
</p>

# Day 07 — Process Management

## Table of Contents

- [Day 07 — Process Management](#day-07--process-management)
  - [Table of Contents](#table-of-contents)
  - [Process Management](#process-management)
    - [Understanding Processes](#understanding-processes)
      - [What Is a Process?](#what-is-a-process)
      - [Processes and Programs](#processes-and-programs)
      - [Process IDs (PID) and Parent Process IDs (PPID)](#process-ids-pid-and-parent-process-ids-ppid)
      - [The Init Process and PID 1](#the-init-process-and-pid-1)
      - [Processes and Threads](#processes-and-threads)
      - [The `/proc` Filesystem](#the-proc-filesystem)
    - [Managing Foreground and Background Processes](#managing-foreground-and-background-processes)
      - [Foreground Processes](#foreground-processes)
      - [Background Processes](#background-processes)
      - [Suspending a Foreground Process](#suspending-a-foreground-process)
      - [Managing Background Jobs with jobs](#managing-background-jobs-with-jobs)
      - [Resuming a Suspended Process with bg](#resuming-a-suspended-process-with-bg)
      - [Bringing a Background Process to the Foreground with fg](#bringing-a-background-process-to-the-foreground-with-fg)
      - [Using nohup](#using-nohup)
      - [Using disown](#using-disown)
      - [Practical Example](#practical-example)
    - [Listing and Finding Processes](#listing-and-finding-processes)
      - [Using ps](#using-ps)
      - [Using top](#using-top)
      - [Using htop](#using-htop)
      - [Using pgrep](#using-pgrep)
      - [Using pidof](#using-pidof)
      - [Using pstree](#using-pstree)
      - [Comparing Process Monitoring Commands](#comparing-process-monitoring-commands)
    - [Understanding Process States](#understanding-process-states)
      - [Common Process States](#common-process-states)
      - [Running and Runnable Processes (R)](#running-and-runnable-processes-r)
      - [Sleeping Processes (S)](#sleeping-processes-s)
      - [Uninterruptible Sleep (D)](#uninterruptible-sleep-d)
      - [Stopped Processes (T)](#stopped-processes-t)
      - [Zombie Processes (Z)](#zombie-processes-z)
      - [Inspecting Process States](#inspecting-process-states)
    - [Managing Process Signals](#managing-process-signals)
      - [What Are Signals?](#what-are-signals)
      - [Common Linux Signals](#common-linux-signals)
      - [Sending Signals with kill](#sending-signals-with-kill)
      - [Sending Signals with pkill](#sending-signals-with-pkill)
      - [Sending Signals with killall](#sending-signals-with-killall)
      - [Handling Signals in Shell Scripts](#handling-signals-in-shell-scripts)
    - [Terminating Processes](#terminating-processes)
      - [Graceful Process Termination](#graceful-process-termination)
      - [Forcefully Terminating a Process](#forcefully-terminating-a-process)
      - [Terminating Processes by Name](#terminating-processes-by-name)
      - [Terminating a Process Group](#terminating-a-process-group)
      - [Waiting for a Process to Finish](#waiting-for-a-process-to-finish)
      - [Using timeout](#using-timeout)
      - [Recommended Termination Workflow](#recommended-termination-workflow)
    - [Managing Process Priorities](#managing-process-priorities)
      - [Understanding Nice Values](#understanding-nice-values)
      - [Viewing Process Priorities](#viewing-process-priorities)
      - [Starting a Process with nice](#starting-a-process-with-nice)
      - [Changing an Existing Process Priority with renice](#changing-an-existing-process-priority-with-renice)
      - [Monitoring Changes in Process Priority](#monitoring-changes-in-process-priority)
      - [Understanding Scheduling Policies](#understanding-scheduling-policies)
    - [Understanding Process Creation and Forking](#understanding-process-creation-and-forking)
      - [What Is Process Creation?](#what-is-process-creation)
      - [Understanding fork()](#understanding-fork)
      - [Return Values of fork()](#return-values-of-fork)
      - [Example of fork() in C](#example-of-fork-in-c)
      - [Understanding exec()](#understanding-exec)
      - [Understanding fork() and exec() Together](#understanding-fork-and-exec-together)
      - [Understanding wait() and waitpid()](#understanding-wait-and-waitpid)
      - [Understanding Orphan Processes](#understanding-orphan-processes)
      - [Understanding Zombie Processes in Process Creation](#understanding-zombie-processes-in-process-creation)
      - [Understanding the Difference Between fork(), exec(), and wait()](#understanding-the-difference-between-fork-exec-and-wait)
  - [Process Management Summary](#process-management-summary)
  - [Best Practices for Process Management](#best-practices-for-process-management)
  - [Practical Exercises](#practical-exercises)
    - [Exercise 1 — Understanding Processes](#exercise-1--understanding-processes)
    - [Exercise 2 — Managing Foreground and Background Processes](#exercise-2--managing-foreground-and-background-processes)
    - [Exercise 3 — Listing and Finding Processes](#exercise-3--listing-and-finding-processes)
    - [Exercise 4 — Understanding Process States](#exercise-4--understanding-process-states)
    - [Exercise 5 — Managing Process Signals](#exercise-5--managing-process-signals)
    - [Exercise 6 — Terminating Processes](#exercise-6--terminating-processes)
    - [Exercise 7 — Managing Process Priorities](#exercise-7--managing-process-priorities)
    - [Exercise 8 — Understanding Process Creation and Forking](#exercise-8--understanding-process-creation-and-forking)
    - [Exercise 9 — Process Management with Shell Scripts](#exercise-9--process-management-with-shell-scripts)
    - [Exercise 10 — Final Challenge: Process Management Investigation](#exercise-10--final-challenge-process-management-investigation)
    - [Knowledge Check](#knowledge-check)

---


## Process Management

Process management is an essential part of Linux system administration. A process is an instance of a running program that requires system resources such as CPU time, memory, and access to files and devices.

Linux provides several tools and mechanisms for creating, monitoring, controlling, prioritizing, and terminating processes. Understanding these mechanisms helps administrators manage system resources, troubleshoot performance problems, and maintain system stability.

### Understanding Processes

#### What Is a Process?

A process is a running instance of a program. When a program is executed, the Linux kernel creates a process and assigns it the resources required to run.

For example, when you execute the following command:

```bash
firefox
```

Linux starts a process that runs the Firefox application.

Each process has its own execution context and resources, including:

- **Process ID (PID):** A unique numerical identifier assigned to a process.
- **Parent Process ID (PPID):** The PID of the process that created it.
- **User ID (UID):** Identifies the user who owns the process.
- **Group ID (GID):** Identifies the primary group associated with the process.
- **Memory Space:** The memory allocated to the process.
- **Process State:** Indicates the current execution state of the process.
- **Open File Descriptors:** References to files, sockets, pipes, and other resources used by the process.
- **CPU Time:** The amount of processor time consumed by the process.

The kernel uses this information to manage processes and allocate system resources.

#### Processes and Programs

A program and a process are related but different concepts.

| Program | Process |
|---|---|
| A program is a file containing executable instructions. | A process is an executing instance of a program. |
| It is stored on a filesystem. | It is managed by the operating system. |
| It does not consume CPU time while inactive. | It can consume CPU time while executing. |
| Multiple processes can execute the same program. | Each process has its own PID and execution context. |

For example, opening multiple terminal windows may create multiple shell processes, even though they all execute the same shell program.

#### Process IDs (PID) and Parent Process IDs (PPID)

Every process running in Linux has a unique Process ID (PID).

The kernel assigns a PID when a process is created. Once a process terminates, its PID can eventually be reused by another process.

The Parent Process ID (PPID) identifies the process that created the current process.

You can display the PID of the current shell using:

```bash
echo $$
```

To display the PID of the most recently executed background command:

```bash
echo $!
```

To display the PID and PPID of the current shell:

```bash
ps -p $$ -o pid,ppid,comm
```

Example output:

```text
    PID    PPID COMMAND
   2450    2301 bash
```

In this example:

- `2450` is the PID of the current Bash process.
- `2301` is its parent process ID.
- `bash` is the command associated with the process.

#### The Init Process and PID 1

During the Linux boot process, the kernel starts the first userspace process, which receives PID 1.

On most modern Linux distributions, this process is `systemd`.

The PID 1 process is responsible for initializing and managing important userspace services and processes.

You can inspect it using:

```bash
ps -p 1 -o pid,ppid,comm,args
```

You can also display the process hierarchy:

```bash
pstree
```

Or display the hierarchy with process IDs:

```bash
pstree -p
```

The process tree helps you understand how processes are related and which processes created other processes.

#### Processes and Threads

A process can contain one or more threads of execution.

A thread is the smallest unit of execution that can be independently scheduled by the operating system.

Threads belonging to the same process generally share:

- Process memory and address space.
- Open file descriptors.
- Working directory.
- Process resources.

However, each thread has its own execution state, registers, stack, and thread identifier.

For example, a web browser may use multiple threads to handle different activities such as rendering pages, processing network requests, and managing user interactions.

You can display threads using:

```bash
ps -eLf
```

To display the threads of a specific process:

```bash
ps -T -p PID
```

Replace `PID` with the actual process ID.

**Important:** A process and a thread are not interchangeable. A process provides an execution environment, while threads provide multiple execution paths within that environment.

#### The `/proc` Filesystem

Linux provides information about running processes through the virtual `/proc` filesystem.

Each running process usually has a directory named after its PID.

For example:

```bash
ls /proc/$$
```

You can inspect different aspects of a process through files inside its directory:

| File | Description |
|---|---|
| `/proc/PID/status` | Displays process information and status. |
| `/proc/PID/cmdline` | Contains the command-line arguments. |
| `/proc/PID/comm` | Contains the process name. |
| `/proc/PID/stat` | Provides detailed process statistics. |
| `/proc/PID/environ` | Contains environment variables. |
| `/proc/PID/fd/` | Contains symbolic links to open file descriptors. |
| `/proc/PID/cwd` | Points to the process's current working directory. |
| `/proc/PID/exe` | Points to the executable associated with the process. |

For example, to inspect the status of a process:

```bash
cat /proc/$$/status
```

To display its command-line arguments:

```bash
cat /proc/$$/cmdline
```

The `/proc` filesystem is a virtual filesystem generated by the kernel. Its contents are not ordinary files stored on disk.

**Note:** Access to some process information may be restricted by permissions, security settings, or system configuration.

---

### Managing Foreground and Background Processes

Linux allows processes to run in the foreground or background.

This capability is especially useful when working with long-running commands because you can continue using the terminal without waiting for a process to finish.

#### Foreground Processes

A foreground process is a process that is currently associated with the terminal and receives input from it.

When you execute a command normally, it usually runs in the foreground.

For example:

```bash
sleep 60
```

The terminal waits until the command finishes.

During this time, you cannot enter another command in the same shell prompt.

You can interrupt a foreground process by pressing:

```text
Ctrl + C
```

This normally sends the `SIGINT` signal to the foreground process group.

#### Background Processes

A background process runs without keeping the shell occupied.

To start a command in the background, add the `&` operator at the end:

```bash
sleep 60 &
```

Example output:

```text
[1] 3250
```

The output usually contains:

- `1`: The job number assigned by the shell.
- `3250`: The PID of the background process.

You can immediately execute other commands:

```bash
date
whoami
pwd
```

The background process continues running while you use the shell.

To display the PID of the most recently started background process:

```bash
echo $!
```

**Important:** A background process started this way is still associated with the shell's job control. It is not automatically independent of the terminal or guaranteed to survive when the shell exits.

#### Suspending a Foreground Process

You can temporarily suspend a foreground process by pressing:

```text
Ctrl + Z
```

This normally sends the `SIGTSTP` signal to the foreground process group.

For example:

```bash
sleep 300
```

Press `Ctrl + Z` to suspend it.

Example output:

```text
[1]+  Stopped    sleep 300
```

The process remains in memory, but it is no longer executing normally.

#### Managing Background Jobs with jobs

The `jobs` command displays jobs managed by the current shell.

To list all current jobs:

```bash
jobs
```

To display additional information, including process IDs:

```bash
jobs -l
```

To display only running jobs:

```bash
jobs -r
```

To display only stopped jobs:

```bash
jobs -s
```

Example output:

```text
[1]-  Running    sleep 120 &
[2]+  Stopped    sleep 300
```

Job numbers are local to the current shell. They are different from process IDs.

#### Resuming a Suspended Process with bg

The `bg` command resumes a stopped job in the background.

For example:

```bash
bg
```

This resumes the most recently suspended job.

To resume a specific job:

```bash
bg %1
```

Here, `%1` refers to job number 1.

You can verify the job status using:

```bash
jobs
```

#### Bringing a Background Process to the Foreground with fg

The `fg` command brings a background or suspended job into the foreground.

To bring the most recently managed job to the foreground:

```bash
fg
```

To bring a specific job to the foreground:

```bash
fg %1
```

The terminal becomes associated with that job again.

#### Using nohup

The `nohup` command allows a process to ignore the hangup signal (`SIGHUP`).

It is useful when you want a command to continue running after the terminal session ends.

For example:

```bash
nohup ./backup.sh &
```

By default, output is written to a file named `nohup.out` in the current directory, unless standard output is redirected.

You can specify an output file:

```bash
nohup ./backup.sh > backup.log 2>&1 &
```

In this example:

- `nohup` prevents the command from being terminated by a hangup signal.
- `> backup.log` redirects standard output.
- `2>&1` redirects standard error to the same destination.
- `&` starts the command in the background.

**Important:** `nohup` does not automatically detach a process from every session or protect it from all termination conditions. For long-running services, consider using `systemd` or another service manager.

#### Using disown

In Bash, the `disown` command removes a job from the shell's job table.

For example:

```bash
sleep 300 &
disown %1
```

After this command, job number 1 is no longer managed by the current shell.

To remove all jobs from the shell's job table:

```bash
disown -a
```

To remove a job and mark it to avoid receiving `SIGHUP` from Bash:

```bash
disown -h %1
```

**Note:** `disown` is a shell built-in available in shells such as Bash. Its behavior is different from `nohup`.

#### Practical Example

The following commands demonstrate basic job control:

```bash
sleep 300 &
jobs
```

Bring the job to the foreground:

```bash
fg %1
```

Suspend it using `Ctrl + Z`, then resume it in the background:

```bash
bg %1
```

Check its status:

```bash
jobs -l
```

Finally, terminate the process:

```bash
kill %1
```

---

### Listing and Finding Processes

Linux provides several commands for displaying, monitoring, and locating processes.

These tools help administrators identify resource-intensive processes, investigate system behavior, and locate specific applications.

#### Using ps

The `ps` command displays information about processes.

To display processes associated with the current terminal:

```bash
ps
```

To display all processes in a detailed format:

```bash
ps -ef
```

To display processes in BSD-style format:

```bash
ps aux
```

Although `ps -ef` and `ps aux` provide similar information, their option syntax differs.

Common `ps` options include:

| Option | Description |
|---|---|
| `-e` | Selects all processes. |
| `-f` | Displays a full-format listing. |
| `-u USER` | Displays processes belonging to a specific user. |
| `-p PID` | Selects a specific process by PID. |
| `-o` | Defines custom output columns. |
| `-T` | Displays threads associated with a process. |
| `-H` | Displays processes in a hierarchical format. |

To display processes belonging to a specific user:

```bash
ps -u username
```

To inspect a specific process:

```bash
ps -p 3250 -f
```

To display selected process information:

```bash
ps -eo pid,ppid,user,stat,%cpu,%mem,comm
```

This command displays:

- `PID`: Process ID.
- `PPID`: Parent Process ID.
- `USER`: Process owner.
- `STAT`: Process state.
- `%CPU`: CPU usage.
- `%MEM`: Memory usage.
- `COMMAND`: Process name.

To display the processes in a tree-like format:

```bash
ps -ef --forest
```

#### Using top

The `top` command provides a real-time view of system processes and resource usage.

Start it using:

```bash
top
```

The interface displays information such as:

- System uptime.
- Load averages.
- Total tasks and process states.
- CPU utilization.
- Memory and swap usage.
- Individual process resource consumption.

Common interactive shortcuts:

| Key | Description |
|---|---|
| `P` | Sorts processes by CPU usage. |
| `M` | Sorts processes by memory usage. |
| `N` | Sorts processes by PID. |
| `T` | Sorts processes by CPU time. |
| `k` | Sends a signal to a selected process. |
| `r` | Changes the nice value of a process. |
| `u` | Filters processes by user. |
| `1` | Displays individual CPU statistics. |
| `q` | Exits the program. |

To refresh the display at a specific interval:

```bash
top -d 2
```

This updates the display every two seconds.

To display information about a specific PID:

```bash
top -p 3250
```

#### Using htop

`htop` is an interactive process monitoring tool that provides a more user-friendly interface than `top`.

It offers features such as:

- Color-coded resource information.
- Interactive process selection.
- Process tree visualization.
- CPU and memory usage indicators.
- Convenient process management.

Start it using:

```bash
htop
```

If it is not installed, you can install it using your distribution's package manager.

Debian and Ubuntu:

```bash
sudo apt install htop
```

Fedora:

```bash
sudo dnf install htop
```

Arch Linux:

```bash
sudo pacman -S htop
```

#### Using pgrep

The `pgrep` command searches for processes based on their names and other attributes.

To find processes matching a name:

```bash
pgrep firefox
```

To display both PID and process name:

```bash
pgrep -a firefox
```

To search for an exact process name:

```bash
pgrep -x firefox
```

To search for processes belonging to a specific user:

```bash
pgrep -u username
```

To search for processes matching a pattern:

```bash
pgrep -f "python script.py"
```

The `-f` option searches the full command line instead of only the process name.

#### Using pidof

The `pidof` command displays the PIDs associated with a running program.

For example:

```bash
pidof sshd
```

It may return multiple PIDs if multiple instances of the program are running.

To display the PID of a specific application:

```bash
pidof nginx
```

Unlike `pgrep`, `pidof` is generally focused on locating processes by executable name.

#### Using pstree

The `pstree` command displays processes in a hierarchical tree.

To display the process tree:

```bash
pstree
```

To include process IDs:

```bash
pstree -p
```

To display a specific process and its descendants:

```bash
pstree -p 3250
```

This is particularly useful when investigating parent-child relationships and identifying processes created by another application.

#### Comparing Process Monitoring Commands

| Command | Main Purpose |
|---|---|
| `ps` | Displays a snapshot of running processes. |
| `top` | Provides real-time process monitoring. |
| `htop` | Provides an interactive process monitoring interface. |
| `pgrep` | Searches for processes by name and attributes. |
| `pidof` | Finds PIDs associated with a program. |
| `pstree` | Displays processes in a hierarchical structure. |

**Important:** `ps` provides a snapshot rather than continuous monitoring. Use `top` or `htop` when you need to observe changing resource usage over time.

---

### Understanding Process States

Every Linux process has a state that describes its current condition.

The kernel changes process states as processes execute, wait for resources, become suspended, or terminate.

Understanding these states is important when troubleshooting performance problems and identifying processes that are not behaving as expected.

#### Common Process States

Linux process states commonly include the following:

| State | Name | Description |
|---|---|---|
| `R` | Running / Runnable | The process is currently executing or waiting to be scheduled on a CPU. |
| `S` | Interruptible Sleep | The process is waiting for an event and can generally be interrupted by signals. |
| `D` | Uninterruptible Sleep | The process is waiting in an uninterruptible kernel sleep, commonly associated with I/O. |
| `T` | Stopped | The process has been stopped, usually by a job-control or stop signal. |
| `t` | Tracing Stop | The process is stopped during tracing or debugging. |
| `Z` | Zombie | The process has terminated, but its parent has not yet collected its exit status. |
| `I` | Idle Kernel Thread | An idle kernel thread is waiting for work. |

Some states are specific to Linux, and their availability or representation may vary depending on the kernel and monitoring tool.

#### Running and Runnable Processes (R)

A process in the `R` state is either:

- Currently executing on a CPU.
- Ready to execute but waiting for CPU scheduling.

A process does not need to be actively using the CPU at every moment to appear as `R`.

#### Sleeping Processes (S)

A process in the `S` state is waiting for an event or resource.

For example, a process waiting for user input or a timer may enter interruptible sleep.

This is a normal state and does not necessarily indicate a problem.

#### Uninterruptible Sleep (D)

A process in the `D` state is waiting for an event that cannot normally be interrupted by signals.

This commonly occurs during certain kernel operations, especially I/O operations.

A process may remain in this state while waiting for a disk, network filesystem, or another kernel-level operation.

You can identify processes in this state using:

```bash
ps -eo pid,stat,comm | awk '$2 ~ /^D/'
```

A process stuck in `D` state for an extended period may indicate an underlying I/O or hardware problem.

**Important:** Sending `SIGKILL` does not immediately terminate a process in uninterruptible sleep. The process generally must first return from the uninterruptible operation.

#### Stopped Processes (T)

A process in the `T` state has been stopped and is not currently executing.

For example, pressing `Ctrl + Z` in a terminal normally suspends the foreground process group.

You can inspect stopped processes using:

```bash
ps -eo pid,stat,comm | awk '$2 ~ /^T/'
```

A stopped process can usually be resumed using:

```bash
kill -CONT PID
```

#### Zombie Processes (Z)

A zombie process is a process that has already terminated, but its parent has not yet collected its exit status.

When a child process exits, the kernel preserves a small amount of information about it, including its PID and exit status.

The parent process must collect this information using a mechanism such as `wait()` or `waitpid()`.

Until this happens, the terminated child remains in the process table as a zombie.

To find zombie processes:

```bash
ps -eo pid,ppid,stat,comm | awk '$3 ~ /^Z/'
```

Zombie processes do not execute and do not consume CPU time. However, they retain process-table entries and PIDs until they are reaped.

To resolve a zombie process:

1. Identify its parent using the PPID column.
2. Investigate why the parent is not collecting the child's exit status.
3. Allow the parent to call `wait()` or `waitpid()`.
4. If necessary, restart or terminate the parent after considering the consequences.

**Important:** You cannot directly terminate a zombie process using `kill` because it has already terminated. The parent must collect its exit status, or the zombie must be adopted and reaped by an appropriate process.

#### Inspecting Process States

You can display process states using:

```bash
ps -eo pid,ppid,stat,comm
```

Example output:

```text
    PID    PPID STAT COMMAND
   1020       1 Ss   systemd
   2450    2301 S    bash
   3250    2450 R    ps
```

The `STAT` column may contain additional characters beyond the primary process state.

Common additional indicators include:

| Indicator | Description |
|---|---|
| `s` | Session leader. |
| `l` | Multithreaded process. |
| `+` | Process belongs to the foreground process group. |
| `<` | High-priority process. |
| `N` | Low-priority process. |

For example, `Ss` indicates a process in interruptible sleep that is also a session leader.

**Note:** The exact formatting of process states can vary slightly between Linux tools and versions.

---

### Managing Process Signals

Signals are software notifications used by Linux to communicate with processes.

They allow the kernel, users, and other processes to request actions such as terminating, suspending, or resuming a process.

Signals are commonly used for process control and system administration.

#### What Are Signals?

A signal is a notification delivered to a process to indicate that a particular event has occurred.

For example:

- A user presses `Ctrl + C`.
- An administrator requests a process to terminate.
- The kernel reports an exceptional condition.
- A terminal session ends.

Each signal has a name and a corresponding number.

You can list available signals using:

```bash
kill -l
```

The exact signal numbers may vary across operating systems and architectures, so using signal names is generally more portable.

#### Common Linux Signals

| Signal | Number | Description |
|---|---:|---|
| `SIGHUP` | 1 | Hangup signal, commonly associated with terminal disconnection. |
| `SIGINT` | 2 | Interrupts a process, commonly generated by `Ctrl + C`. |
| `SIGQUIT` | 3 | Requests termination and may produce a core dump. |
| `SIGKILL` | 9 | Immediately requests forced termination. |
| `SIGTERM` | 15 | Requests graceful process termination. |
| `SIGSTOP` | 19 | Stops a process and cannot be caught or ignored. |
| `SIGTSTP` | 20 | Requests a terminal-related stop, commonly generated by `Ctrl + Z`. |
| `SIGCONT` | 18 | Resumes a stopped process. |
| `SIGCHLD` | 17 | Notifies a parent about changes in a child process. |

**Note:** The numerical values shown are common on Linux architectures such as x86 and x86-64. Use signal names when writing portable commands.

#### Sending Signals with kill

Despite its name, the `kill` command does not always terminate a process.

It sends a specified signal to a process.

The basic syntax is:

```bash
kill [OPTION] PID
```

By default, `kill` sends `SIGTERM`.

For example:

```bash
kill 3250
```

This requests graceful termination of process 3250.

To send a specific signal:

```bash
kill -SIGNAL PID
```

For example, send `SIGTERM`:

```bash
kill -TERM 3250
```

Send `SIGKILL`:

```bash
kill -KILL 3250
```

Send `SIGSTOP`:

```bash
kill -STOP 3250
```

Send `SIGCONT`:

```bash
kill -CONT 3250
```

You can also use signal numbers:

```bash
kill -15 3250
```

However, signal names are generally easier to understand and less error-prone.

#### Sending Signals with pkill

The `pkill` command sends signals to processes selected by name or other matching criteria.

To terminate processes matching a name:

```bash
pkill firefox
```

By default, this sends `SIGTERM`.

To send a specific signal:

```bash
pkill -TERM firefox
```

To send a signal to processes owned by a specific user:

```bash
pkill -u username firefox
```

To match the full command line:

```bash
pkill -f "python script.py"
```

**Warning:** A pattern may match multiple processes. Always verify the matching processes before sending signals.

You can preview matching processes using:

```bash
pgrep -a firefox
```

#### Sending Signals with killall

The `killall` command sends signals to processes based on their names.

For example:

```bash
killall firefox
```

To send a specific signal:

```bash
killall -TERM firefox
```

To force termination:

```bash
killall -KILL firefox
```

**Important:** `killall` behavior and implementation can differ between Unix-like systems. On Linux distributions that use the common `psmisc` implementation, it targets processes by name.

#### Handling Signals in Shell Scripts

Shell scripts can respond to certain signals using the `trap` command.

For example:

```bash
#!/bin/bash

cleanup() {
    echo "Cleaning up temporary files..."
    rm -f /tmp/example.tmp
    echo "Cleanup completed."
}

trap cleanup EXIT

touch /tmp/example.tmp

echo "Script is running..."
sleep 10
```

In this example:

- `trap` registers a command to execute when the shell exits.
- `EXIT` triggers the cleanup function when the script exits.
- The temporary file is removed during cleanup.

You can also handle signals such as `SIGINT`:

```bash
#!/bin/bash

trap 'echo "Interrupt received."' INT

while true; do
    sleep 2
done
```

Pressing `Ctrl + C` causes the shell to execute the registered handler.

**Important:** `SIGKILL` and `SIGSTOP` cannot be caught, blocked, or ignored by a process. They cannot be handled using `trap`.

---

### Terminating Processes

Terminating processes is a fundamental system administration task.

Linux provides several methods for stopping processes, depending on whether they respond normally, become unresponsive, or need to be terminated as part of a larger process group.

#### Graceful Process Termination

The recommended first step is usually to request a process to terminate gracefully.

Use:

```bash
kill PID
```

This sends `SIGTERM` by default.

A process receiving `SIGTERM` can:

- Finish its current operation.
- Save important data.
- Close open files.
- Release resources.
- Perform cleanup operations.
- Exit normally.

For example:

```bash
kill 3250
```

You can verify whether the process is still running:

```bash
ps -p 3250
```

If the process has terminated, it will no longer appear in the output.

#### Forcefully Terminating a Process

If a process does not respond to `SIGTERM`, you may need to send `SIGKILL`.

```bash
kill -KILL 3250
```

Or:

```bash
kill -9 3250
```

`SIGKILL` cannot be caught, ignored, or handled by the target process.

The kernel terminates the process when it can complete the necessary kernel-level operations.

**Important:** Use `SIGKILL` only when necessary. It does not allow the process to perform normal cleanup operations and may result in incomplete writes, temporary files, or inconsistent application state.

It also cannot immediately terminate a process stuck in uninterruptible sleep.

#### Terminating Processes by Name

You can terminate processes by name using `pkill` or `killall`.

Using `pkill`:

```bash
pkill firefox
```

Using `killall`:

```bash
killall firefox
```

Both commands normally send `SIGTERM` unless another signal is specified.

Always verify the target processes before using these commands, especially when multiple instances are running.

#### Terminating a Process Group

Linux processes can belong to process groups.

A process group allows multiple related processes to receive the same signal.

You can display process group IDs using:

```bash
ps -eo pid,ppid,pgid,comm
```

To send a signal to a process group, use a negative process group ID:

```bash
kill -TERM -- -PGID
```

Replace `PGID` with the actual process group ID.

This is useful when you need to terminate a group of related processes.

**Warning:** Sending a signal to a process group can affect multiple processes. Verify the process group before executing the command.

#### Waiting for a Process to Finish

The Bash `wait` built-in waits for a background process or job to complete.

For example:

```bash
sleep 5 &
PID=$!

wait "$PID"

echo "Process completed."
```

In this example:

1. `sleep 5 &` starts a background process.
2. `$!` stores its PID.
3. `wait` pauses the shell until the process finishes.
4. The final message is displayed.

The `wait` command can also collect the exit status of a completed child process.

For example:

```bash
sleep 2 &
PID=$!

wait "$PID"

echo "Exit status: $?"
```

#### Using timeout

The `timeout` command runs another command with a time limit.

For example:

```bash
timeout 10s sleep 30
```

The command is allowed to run for up to 10 seconds. If it has not finished, `timeout` normally sends `SIGTERM`.

To specify a different signal:

```bash
timeout -s KILL 10s sleep 30
```

To send a termination signal first and then force termination after a grace period:

```bash
timeout -k 5s 10s sleep 30
```

In this example:

- `10s` is the initial time limit.
- `-k 5s` specifies an additional five-second period before sending `SIGKILL` if the command has not terminated.

The `timeout` command is useful for preventing commands and scripts from running indefinitely.

#### Recommended Termination Workflow

When terminating an unresponsive process, follow this sequence:

1. Identify the correct process using `ps`, `pgrep`, or `top`.
2. Inspect its PID, owner, and command.
3. Send `SIGTERM` to request graceful termination.
4. Wait for a reasonable amount of time.
5. Check whether the process has terminated.
6. Use `SIGKILL` only if the process remains unresponsive and forceful termination is appropriate.

This approach reduces the risk of data loss and unnecessary system disruption.

---

### Managing Process Priorities

Linux uses process scheduling to determine how CPU time is distributed among runnable processes.

Process priorities help control how processes compete for CPU resources.

For normal scheduling policies, Linux uses a value called the nice value to influence scheduling priority.

#### Understanding Nice Values

The nice value ranges from `-20` to `19`.

| Nice Value | Priority |
|---|---|
| `-20` | Highest scheduling priority among normal nice values. |
| `0` | Default nice value. |
| `19` | Lowest scheduling priority among normal nice values. |

The relationship is inverse:

- Lower nice values generally mean higher CPU scheduling priority.
- Higher nice values generally mean lower CPU scheduling priority.

For example:

- A process with nice value `-10` is generally favored over a comparable process with nice value `10`.
- A process with nice value `10` is generally less favored when competing for CPU time.

Nice values do not represent CPU usage percentages or guaranteed CPU allocations.

**Important:** Nice values affect scheduling behavior, but they do not guarantee that a process will receive a specific amount of CPU time.

#### Viewing Process Priorities

You can inspect process priorities using `ps`.

For example:

```bash
ps -eo pid,ni,pri,comm
```

The output includes:

- `PID`: Process ID.
- `NI`: Nice value.
- `PRI`: Kernel-reported scheduling priority.
- `COMMAND`: Process name.

You can also inspect a specific process:

```bash
ps -p 3250 -o pid,ni,pri,comm
```

#### Starting a Process with nice

The `nice` command starts a program with a specified nice value.

The basic syntax is:

```bash
nice -n VALUE COMMAND
```

For example:

```bash
nice -n 10 tar -czf backup.tar.gz /home/user
```

This starts the archive operation with a nice value of 10.

The process receives a lower scheduling preference than a process with the default nice value of 0, assuming comparable scheduling conditions.

To start a process with a higher scheduling preference:

```bash
sudo nice -n -10 command
```

Increasing scheduling priority by using a negative nice value generally requires elevated privileges.

If you do not specify a nice value, `nice` normally increases the nice value by 10.

For example:

```bash
nice command
```

#### Changing an Existing Process Priority with renice

The `renice` command changes the nice value of an already running process.

The basic syntax is:

```bash
renice -n VALUE -p PID
```

For example:

```bash
renice -n 10 -p 3250
```

This changes the nice value of process 3250 to 10.

To assign a lower nice value:

```bash
sudo renice -n -5 -p 3250
```

Changing a process to a more favorable priority generally requires elevated privileges.

To change the nice value of processes belonging to a user:

```bash
sudo renice -n 10 -u username
```

#### Monitoring Changes in Process Priority

After changing the nice value, verify it using:

```bash
ps -p 3250 -o pid,ni,pri,comm
```

You can also monitor process priority through:

```bash
top
```

The `NI` column displays the nice value of each process.

#### Understanding Scheduling Policies

Nice values primarily affect processes using the normal scheduling policy, `SCHED_OTHER` (also known as `SCHED_NORMAL`).

Linux also supports other scheduling policies, including:

| Scheduling Policy | Description |
|---|---|
| `SCHED_OTHER` | Normal time-sharing scheduling for typical applications. |
| `SCHED_BATCH` | Intended for CPU-intensive batch workloads. |
| `SCHED_IDLE` | Runs tasks only when the system has very little competing work. |
| `SCHED_FIFO` | Real-time first-in, first-out scheduling. |
| `SCHED_RR` | Real-time round-robin scheduling. |
| `SCHED_DEADLINE` | Deadline-based real-time scheduling. |

Real-time scheduling policies require additional care because they can affect the responsiveness of the entire system.

For example, an incorrectly configured real-time process can consume significant CPU time and prevent ordinary processes from running effectively.

**Important:** Nice values do not control all scheduling policies in the same way. Their primary purpose is to influence the relative scheduling preference of normal processes.

---

### Understanding Process Creation and Forking

Linux uses a process creation mechanism to start new processes and execute programs.

One of the most important system calls involved in this mechanism is `fork()`.

Understanding process creation helps explain how Linux starts applications, creates child processes, and manages parent-child relationships.

#### What Is Process Creation?

A process can create another process during execution.

The process that creates a new process is called the parent process, while the newly created process is called the child process.

For example, when a shell executes an external command, it commonly creates a child process to run that command.

A simplified process hierarchy might look like this:

```text
systemd (PID 1)
    |
    +-- sshd
    |     |
    |     +-- bash
    |           |
    |           +-- ls
    |
    +-- cron
```

In this example:

- `systemd` is the initial userspace process.
- `sshd` creates or manages a session-related process.
- `bash` runs within the session.
- `ls` is launched by the shell.

The exact process hierarchy depends on the system and how applications are started.

#### Understanding fork()

The `fork()` system call creates a new process by duplicating the calling process.

After a successful `fork()`:

- The original process continues as the parent.
- A new process continues as the child.
- Both processes execute independently.
- Each process has its own PID.
- The child receives a copy of the parent's execution context.

Modern Linux systems generally use copy-on-write (COW) memory management.

This means that the parent and child initially share memory pages until one of them modifies a page. The kernel then creates separate copies as needed.

This makes process creation more efficient than immediately copying the entire memory space.

#### Return Values of fork()

The `fork()` system call returns different values depending on where it is executed:

| Return Value | Meaning |
|---|---|
| Positive value | The parent receives the PID of the newly created child. |
| `0` | The child process receives zero. |
| `-1` | Process creation failed, and no child was created. |

The parent and child continue executing from the point immediately after the `fork()` call.

#### Example of fork() in C

The following C program demonstrates how `fork()` creates a child process.

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>

int main(void) {
    pid_t pid = fork();

    if (pid < 0) {
        perror("fork failed");
        return 1;
    }

    if (pid == 0) {
        printf("Child process\n");
        printf("Child PID: %d\n", getpid());
        printf("Parent PID: %d\n", getppid());
    } else {
        printf("Parent process\n");
        printf("Parent PID: %d\n", getpid());
        printf("Child PID: %d\n", pid);
    }

    return 0;
}
```

Save the program as:

```text
fork_example.c
```

Compile it using:

```bash
gcc fork_example.c -o fork_example
```

Execute it:

```bash
./fork_example
```

Example output:

```text
Parent process
Parent PID: 2450
Child PID: 2451
Child process
Child PID: 2451
Parent PID: 2450
```

The output order is not guaranteed because the parent and child execute independently and the scheduler determines which runs first.

**Note:** If the parent exits before the child calls `getppid()`, the child's reported PPID may refer to a process that has adopted it instead of the original parent.

#### Understanding exec()

The `exec()` family of system calls replaces the current process image with a new program.

Unlike `fork()`, `exec()` does not create a new process.

It replaces the program being executed by the calling process while preserving the process identity, including its PID.

Common functions in the `exec()` family include:

| Function | Description |
|---|---|
| `execl()` | Executes a program using a list of arguments. |
| `execv()` | Executes a program using an argument array. |
| `execlp()` | Searches the `PATH` for the executable and uses a list of arguments. |
| `execvp()` | Searches the `PATH` for the executable and uses an argument array. |
| `execve()` | Executes a program with explicitly provided arguments and environment variables. |

A successful `exec()` call does not return to the original program because its process image has been replaced.

#### Understanding fork() and exec() Together

Linux shells commonly use `fork()` and `exec()` together when executing external commands.

A simplified execution sequence is:

1. The shell receives a command from the user.
2. The shell creates a child process using `fork()`.
3. The child process calls an `exec()` function.
4. The operating system replaces the child's program image with the requested executable.
5. The child executes the new program.
6. The parent shell may wait for the child to finish or continue executing other work.

For example:

```bash
ls -l
```

The shell may create a child process and use an `exec()` function to run the `ls` program.

The shell itself remains available to execute additional commands after the child process finishes.

This is a simplified model; shells may use additional optimizations and different execution mechanisms for built-in commands and other special cases.

#### Understanding wait() and waitpid()

After creating a child process, a parent may need to wait for it to finish.

The `wait()` and `waitpid()` system calls allow a parent process to collect the termination status of its child processes.

The `wait()` system call waits for a child process to change state, commonly until it terminates.

The `waitpid()` system call provides more control over which child process the parent waits for.

For example:

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>

int main(void) {
    pid_t pid = fork();

    if (pid < 0) {
        perror("fork failed");
        return 1;
    }

    if (pid == 0) {
        printf("Child process is running.\n");
        return 0;
    }

    int status;
    waitpid(pid, &status, 0);

    printf("Child process has finished.\n");

    return 0;
}
```

In this example:

- The parent creates a child using `fork()`.
- The child executes its code and terminates.
- The parent waits for that specific child using `waitpid()`.
- The parent collects the child's termination status.

This prevents the child from remaining as a zombie after termination.

#### Understanding Orphan Processes

An orphan process is a process whose original parent has terminated while the child is still running.

Linux reparents orphaned processes to an appropriate process, commonly PID 1 or another configured child subreaper.

The new parent becomes responsible for collecting the child's termination status.

For example:

```text
Before parent termination:

Parent
   |
   +-- Child


After parent termination:

systemd
   |
   +-- Child
```

The exact adopting process depends on the system's process hierarchy and subreaper configuration.

An orphan process is not necessarily a problem. Many orphaned processes continue operating normally.

#### Understanding Zombie Processes in Process Creation

A zombie process occurs when a child has terminated but its parent has not yet collected its exit status.

For example:

```text
Parent Process
     |
     +-- Child Process (Zombie)
```

The child is no longer executing, but its process-table entry remains until the parent collects its termination status.

This is why `wait()` and `waitpid()` are important when writing programs that create child processes.

#### Understanding the Difference Between fork(), exec(), and wait()

| System Call | Purpose |
|---|---|
| `fork()` | Creates a new child process. |
| `exec()` | Replaces the current process image with another program. |
| `wait()` | Waits for a child process and collects its status. |
| `waitpid()` | Waits for a specific child process or a selected group of children. |

These mechanisms work together to support process creation, program execution, and process termination in Linux.

---

## Process Management Summary

The following table summarizes the most important commands and concepts covered in this section.

| Command / Concept | Description |
|---|---|
| `ps` | Displays information about running processes. |
| `top` | Monitors processes and system resource usage in real time. |
| `htop` | Provides an interactive process monitoring interface. |
| `pgrep` | Searches for processes by name and attributes. |
| `pidof` | Displays PIDs associated with a program. |
| `pstree` | Displays the process hierarchy. |
| `jobs` | Lists jobs managed by the current shell. |
| `bg` | Resumes a stopped job in the background. |
| `fg` | Brings a job to the foreground. |
| `nohup` | Runs a command while ignoring hangup signals. |
| `disown` | Removes jobs from the current shell's job table. |
| `kill` | Sends signals to processes or process groups. |
| `pkill` | Sends signals to processes selected by name or attributes. |
| `killall` | Sends signals to processes selected by name. |
| `nice` | Starts a command with a specified nice value. |
| `renice` | Changes the nice value of existing processes. |
| `timeout` | Runs a command with a time limit. |
| `wait` | Waits for a background child process to finish. |
| `fork()` | Creates a new child process. |
| `exec()` | Replaces the current process image with another program. |
| `waitpid()` | Waits for a specific child process and collects its status. |

## Best Practices for Process Management

When managing processes in Linux, consider the following practices:

1. **Monitor Processes Regularly:** Use `ps`, `top`, and `htop` to identify resource-intensive processes and unexpected activity.
2. **Understand Process Ownership:** Check the user and permissions associated with a process before attempting to control it.
3. **Use Background Execution Carefully:** Use `&`, `bg`, and `fg` for interactive job control, and use `nohup` or a service manager when appropriate for longer-running tasks.
4. **Prefer Graceful Termination:** Send `SIGTERM` before considering `SIGKILL`.
5. **Avoid Unnecessary Forceful Termination:** Forced termination may interrupt important operations and prevent cleanup.
6. **Monitor Process States:** Investigate processes that remain in `D` or `Z` states for extended periods.
7. **Manage CPU Priorities Carefully:** Use `nice` and `renice` to adjust scheduling preferences when necessary.
8. **Understand Parent-Child Relationships:** Use `pstree` and PPID information to investigate process hierarchies.
9. **Use Appropriate Permissions:** Perform administrative process management only when necessary and with the appropriate privileges.
10. **Use Service Managers for System Services:** Manage persistent system services through tools such as `systemctl` rather than relying on manual background execution.

---


## Practical Exercises

The following exercises are designed to reinforce the concepts and commands introduced in this section. Complete each exercise using a Linux terminal and verify the results of your commands.

### Exercise 1 — Understanding Processes

1. Display the PID of your current shell using `echo $$`.
2. Display the PID of the most recently started background process using `echo $!` after launching one.
3. Display the PID and PPID of your current shell using `ps`.
4. Display all running processes using `ps -ef`.
5. Display processes in a hierarchical format using `pstree -p`.
6. Inspect the `/proc` directory associated with your current shell.
7. Read the process information from `/proc/PID/status`, replacing `PID` with your shell's PID.
8. Identify the relationship between a parent process and its child processes.

### Exercise 2 — Managing Foreground and Background Processes

1. Start a command in the foreground using `sleep 300`.
2. Suspend the process using `Ctrl + Z`.
3. Display the suspended job using `jobs`.
4. Resume the job in the background using `bg`.
5. Display all background jobs using `jobs -l`.
6. Bring the process back to the foreground using `fg`.
7. Terminate the process using `Ctrl + C`.
8. Start another background process using `sleep 300 &`.
9. Display its PID using `echo $!`.
10. Terminate the background process using `kill`.

### Exercise 3 — Listing and Finding Processes

1. Display all running processes using `ps -ef`.
2. Display processes with CPU and memory usage using `ps aux`.
3. Display selected process information using `ps -eo pid,ppid,user,stat,%cpu,%mem,comm`.
4. Find a running process by name using `pgrep`.
5. Display matching process IDs and command names using `pgrep -a`.
6. Find the PID of a running application using `pidof`.
7. Monitor system processes using `top`.
8. Sort processes by CPU usage and memory usage.
9. Display the process hierarchy using `pstree -p`.
10. Compare the information provided by `ps`, `top`, `pgrep`, and `pidof`.

### Exercise 4 — Understanding Process States

1. Start a background process using `sleep 300 &`.
2. Display its current state using `ps`.
3. Suspend the process using `kill -STOP PID`.
4. Verify that the process has entered the stopped state.
5. Resume the process using `kill -CONT PID`.
6. Verify that the process is running or waiting for execution.
7. Identify processes in the `R`, `S`, `D`, `T`, and `Z` states using `ps`.
8. Explain the difference between interruptible sleep, uninterruptible sleep, and zombie processes.
9. Investigate why a zombie process cannot be terminated directly using `kill`.

**Note:** Some process states, especially `D` and `Z`, may not appear during normal system activity. Do not intentionally create problematic processes just to reproduce these states.

### Exercise 5 — Managing Process Signals

1. Display the available signals using `kill -l`.
2. Start a background process using `sleep 300 &`.
3. Send `SIGSTOP` to suspend the process.
4. Verify its state using `ps`.
5. Resume the process using `SIGCONT`.
6. Send `SIGTERM` to request graceful termination.
7. Verify that the process has terminated.
8. Start another background process and send `SIGINT` using `kill`.
9. Compare the behavior of `SIGTERM`, `SIGKILL`, `SIGSTOP`, and `SIGCONT`.
10. Explain why `SIGKILL` and `SIGSTOP` cannot be caught or ignored.

### Exercise 6 — Terminating Processes

1. Start multiple background processes using `sleep 300 &`.
2. Display their PIDs using `jobs -l`.
3. Identify a specific process using `ps`.
4. Terminate the process gracefully using `kill -TERM PID`.
5. Verify that it has stopped.
6. Start another process and terminate it using `pkill`.
7. Use `pgrep` to verify whether matching processes are still running.
8. Execute a command with a time limit using `timeout 5s sleep 30`.
9. Observe the command's exit status.
10. Explain when `SIGKILL` should be used instead of `SIGTERM`.

**Important:** Use only processes that you started for these exercises. Avoid terminating system services or processes belonging to other users.

### Exercise 7 — Managing Process Priorities

1. Display running processes with their nice values using `ps -eo pid,ni,pri,comm`.
2. Start a background process with a nice value of 10 using `nice`.
3. Identify the process PID.
4. Verify its nice value using `ps`.
5. Change its nice value to 15 using `renice`.
6. Verify that the priority value has changed.
7. Start another process with the default nice value.
8. Compare the nice values of both processes.
9. Explain the relationship between nice values and CPU scheduling priority.
10. Identify which nice value represents the highest scheduling priority.

**Note:** Increasing a nice value is generally permitted for the process owner. Decreasing it to a more favorable value usually requires elevated privileges.

### Exercise 8 — Understanding Process Creation and Forking

1. Review the purpose of the `fork()` system call.
2. Identify the differences between parent and child processes.
3. Create a C source file named `fork_example.c`.
4. Write a program that creates a child process using `fork()`.
5. Display the PID of the parent process using `getpid()`.
6. Display the PID of the child process using `getpid()`.
7. Display the parent PID using `getppid()`.
8. Compile the program using `gcc`.
9. Execute the program and observe the output.
10. Modify the program to make the parent wait for the child using `waitpid()`.
11. Explain the purpose of `exec()` and how it differs from `fork()`.
12. Explain how `wait()` and `waitpid()` prevent child processes from remaining zombies.

### Exercise 9 — Process Management with Shell Scripts

Create a shell script named `process-monitor.sh` that performs basic process monitoring.

The script should:

1. Display the current date and time.
2. Display the system uptime and load averages.
3. Display the total number of running processes.
4. Display the top five processes by CPU usage.
5. Display the top five processes by memory usage.
6. Display the PID and state of each selected process.
7. Ask the user to enter a PID.
8. Verify whether the specified process exists.
9. Display detailed information about the selected process.
10. Handle invalid PIDs and missing input appropriately.

Example commands that may help:

```bash
date
uptime
ps
ps -eo
ps -p
```

Make sure the script uses valid process IDs and handles cases where a process terminates while the script is running.

### Exercise 10 — Final Challenge: Process Management Investigation

Investigate and manage a group of processes using the commands and concepts covered in this section.

Complete the following tasks:

1. Start three background processes using different `sleep` durations.
2. Display their job numbers and process IDs.
3. Identify their parent process IDs.
4. Display their current process states.
5. Suspend one process using `SIGSTOP`.
6. Resume the suspended process using `SIGCONT`.
7. Change the nice value of one process using `renice`.
8. Monitor all three processes using `top` or `ps`.
9. Terminate one process gracefully using `SIGTERM`.
10. Terminate another process using `SIGKILL`.
11. Wait for the remaining process to finish.
12. Verify that all test processes have terminated.
13. Create a C program that uses `fork()` to create a child process.
14. Use `waitpid()` to collect the child's exit status.
15. Document the commands used and explain the results.

**Requirements:**

- Use only processes created specifically for this challenge.
- Verify process IDs before sending signals.
- Use graceful termination whenever possible.
- Avoid modifying system services or unrelated processes.
- Document any unexpected process states or command errors.

### Knowledge Check

Answer the following questions to evaluate your understanding of Linux process management.

1. What is a process in Linux?
2. What is the difference between a program and a process?
3. What are PID and PPID?
4. What is the purpose of the `/proc` filesystem?
5. What is the difference between foreground and background processes?
6. What is the purpose of the `jobs` command?
7. What is the difference between `bg` and `fg`?
8. What does `nohup` do?
9. What is the difference between `ps` and `top`?
10. What is the difference between `pgrep` and `pidof`?
11. What do the `R`, `S`, `D`, `T`, and `Z` process states represent?
12. What is a zombie process, and how does it occur?
13. What is a process signal?
14. What is the difference between `SIGTERM` and `SIGKILL`?
15. Why can `SIGKILL` not be caught or ignored?
16. What is the purpose of `pkill` and `killall`?
17. What is a nice value, and what is its valid range?
18. What is the difference between `nice` and `renice`?
19. What is the purpose of the `fork()` system call?
20. What is the difference between `fork()`, `exec()`, and `waitpid()`?

---

**Goal:** Develop practical skills in monitoring and managing Linux processes, controlling foreground and background tasks, handling process signals, adjusting process priorities, and understanding process creation and execution.

<p align="right">
  <a href="../Day-06/README.md">← Day 06</a>
  &nbsp; | &nbsp;
  <a href="../Day-08/">Day 08 →</a>
  &nbsp; | &nbsp;
  <a href="#top">Back to top ↑</a>
</p>
