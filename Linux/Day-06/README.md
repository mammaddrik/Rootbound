<p align="center">
  <img src="../../media/images/Linux/Day-06/image_01.png" alt="image_01">
</p>

# Day 06 — System Monitoring

## Table of Contents

- [Day 06 — System Monitoring](#day-06--system-monitoring)
  - [Table of Contents](#table-of-contents)
  - [System Monitoring](#system-monitoring)
    - [Checking System Uptime and Load](#checking-system-uptime-and-load)
      - [Understanding System Uptime](#understanding-system-uptime)
      - [Understanding Load Average](#understanding-load-average)
      - [Working with the w Command](#working-with-the-w-command)
      - [Monitoring System Activity with top](#monitoring-system-activity-with-top)
      - [Monitoring Processes with ps](#monitoring-processes-with-ps)
      - [Monitoring System Load Through /proc/loadavg](#monitoring-system-load-through-procloadavg)
      - [Practical Examples](#practical-examples)
    - [Monitoring Authentication Logs](#monitoring-authentication-logs)
      - [Understanding Authentication Logs](#understanding-authentication-logs)
      - [Understanding Authentication Log Entries](#understanding-authentication-log-entries)
      - [Monitoring Logs with journalctl](#monitoring-logs-with-journalctl)
      - [Monitoring Successful and Failed Login Attempts](#monitoring-successful-and-failed-login-attempts)
      - [Searching Authentication Logs with grep](#searching-authentication-logs-with-grep)
      - [Monitoring Authentication Activity in Real Time](#monitoring-authentication-activity-in-real-time)
      - [Understanding Log Rotation](#understanding-log-rotation)
      - [Practical Examples](#practical-examples-1)
    - [Checking Running Services](#checking-running-services)
      - [Understanding systemd](#understanding-systemd)
      - [Listing Running Services](#listing-running-services)
      - [Checking the Status of a Service](#checking-the-status-of-a-service)
      - [Checking Whether a Service Is Active](#checking-whether-a-service-is-active)
      - [Checking Whether a Service Starts at Boot](#checking-whether-a-service-starts-at-boot)
      - [Managing Services](#managing-services)
      - [Monitoring Service Logs](#monitoring-service-logs)
      - [Inspecting Failed Services](#inspecting-failed-services)
      - [Understanding Service Unit Files](#understanding-service-unit-files)
      - [Practical Examples](#practical-examples-2)
    - [Monitoring Memory Usage](#monitoring-memory-usage)
      - [Understanding Linux Memory Management](#understanding-linux-memory-management)
      - [Checking Memory Usage with free](#checking-memory-usage-with-free)
      - [Understanding Buffers and Cache](#understanding-buffers-and-cache)
      - [Inspecting Memory Information Through /proc/meminfo](#inspecting-memory-information-through-procmeminfo)
      - [Monitoring Swap Usage](#monitoring-swap-usage)
      - [Identifying Memory-Intensive Processes](#identifying-memory-intensive-processes)
      - [Understanding OOM Killer](#understanding-oom-killer)
      - [Practical Examples](#practical-examples-3)
    - [Monitoring Disk Space](#monitoring-disk-space)
      - [Understanding Disks, Partitions, and Filesystems](#understanding-disks-partitions-and-filesystems)
      - [Checking Filesystem Disk Usage with df](#checking-filesystem-disk-usage-with-df)
      - [Understanding Disk Usage with du](#understanding-disk-usage-with-du)
      - [Finding Large Files](#finding-large-files)
      - [Inspecting Disks and Partitions with lsblk](#inspecting-disks-and-partitions-with-lsblk)
      - [Understanding Mounted Filesystems with findmnt](#understanding-mounted-filesystems-with-findmnt)
      - [Monitoring Inode Usage](#monitoring-inode-usage)
      - [Understanding Disk Usage Differences](#understanding-disk-usage-differences)
      - [Practical Examples](#practical-examples-4)
      - [Best Practices for Disk Monitoring](#best-practices-for-disk-monitoring)
  - [System Monitoring Summary](#system-monitoring-summary)
    - [Additional Resources](#additional-resources)
  - [Practical Exercises](#practical-exercises)
    - [Exercise 1 — Checking System Uptime and Load](#exercise-1--checking-system-uptime-and-load)
    - [Exercise 2 — Monitoring Authentication Logs](#exercise-2--monitoring-authentication-logs)
    - [Exercise 3 — Checking Running Services](#exercise-3--checking-running-services)
    - [Exercise 4 — Monitoring Memory Usage](#exercise-4--monitoring-memory-usage)
    - [Exercise 5 — Monitoring Disk Space](#exercise-5--monitoring-disk-space)
    - [Exercise 6 — System Monitoring with Command Pipelines](#exercise-6--system-monitoring-with-command-pipelines)
    - [Exercise 7 — System Health Investigation](#exercise-7--system-health-investigation)
    - [Exercise 8 — Final Challenge: Linux System Health Report](#exercise-8--final-challenge-linux-system-health-report)
    - [Exercise 9 — Knowledge Check](#exercise-9--knowledge-check)

---


## System Monitoring

System monitoring is the process of observing and analyzing the overall health, performance, and resource usage of a Linux system. It helps administrators understand how the system is operating, identify potential problems, detect unusual activity, and prevent performance issues before they become serious.

Linux provides several command-line tools and system utilities for monitoring different aspects of the operating system, including system uptime, CPU load, authentication activity, running services, memory consumption, and disk space.

In this section, we will explore essential monitoring commands and learn how to use them to inspect system health and troubleshoot common issues.

### Checking System Uptime and Load

System uptime and load provide important information about how long a Linux system has been running and how heavily its CPU resources are being utilized.

Monitoring these values helps administrators identify overloaded systems, investigate performance problems, and understand the overall workload of a machine.

#### Understanding System Uptime

System uptime represents the amount of time a system has been running since its last boot.

The `uptime` command displays the current time, how long the system has been running, the number of logged-in users, and the system load averages.

**Basic Usage:**

```bash
uptime
```

Example output:

```text
18:30:45 up 5 days,  3:42,  2 users,  load average: 0.35, 0.42, 0.38
```

The output contains several important values:

- `18:30:45`: Current system time.
- `up 5 days, 3:42`: The system has been running for five days and three hours and forty-two minutes.
- `2 users`: The number of currently logged-in user sessions.
- `load average`: The average system load over the last 1, 5, and 15 minutes.

**Displaying Human-Readable Uptime:**

The `-p` option displays uptime in a more readable format.

```bash
uptime -p
```

Example output:

```text
up 5 days, 3 hours, 42 minutes
```

**Displaying the Last Boot Time:**

The `-s` option displays the date and time when the system was last started.

```bash
uptime -s
```

Example output:

```text
2026-11-18 14:48:12
```

#### Understanding Load Average

Load average represents the average number of tasks that are either running on the CPU or waiting for CPU time, along with tasks in uninterruptible sleep, typically waiting for I/O operations.

Linux displays three load average values:

| Value | Time Period |
|---|---|
| First value | Last 1 minute |
| Second value | Last 5 minutes |
| Third value | Last 15 minutes |

For example:

```text
load average: 0.35, 0.42, 0.38
```

These values indicate the average system load over three different periods.

**Understanding Load Average Relative to CPU Cores:**

Load average should be interpreted in relation to the number of available logical CPU cores.

For example, consider a system with four logical CPU cores:

| Load Average | General Interpretation |
|---|---|
| 1.00 | Approximately one task's worth of sustained load |
| 2.00 | Approximately half of the four-core capacity |
| 4.00 | Load roughly equal to the system's CPU capacity |
| 8.00 | Load significantly exceeds CPU capacity |

A load average close to the number of logical CPU cores generally indicates that the system can keep up with its workload.

A load average consistently higher than the number of cores may indicate CPU contention or tasks waiting for resources.

However, load average is not a direct CPU utilization percentage. It also includes tasks waiting in uninterruptible sleep, which can indicate disk or other I/O bottlenecks.

**Checking the Number of CPU Cores:**

The `nproc` command displays the number of processing units available to the current process.

```bash
nproc
```

Example output:

```text
4
```

To display detailed CPU information, use:

```bash
lscpu
```

This command displays information such as CPU architecture, processor model, number of CPUs, cores per socket, threads per core, and CPU architecture details.

#### Working with the w Command

The `w` command displays information about currently logged-in users and their activities.

```bash
w
```

Example output:

```text
18:35:10 up 5 days,  3:46,  2 users,  load average: 0.35, 0.42, 0.38
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
root     pts/0    192.168.1.10     14:50    2:10   0.05s  0.02s bash
user     pts/1    192.168.1.15     17:20    0.00s  0.10s  0.03s top
```

The output includes:

- `USER`: Username of the logged-in user.
- `TTY`: Terminal associated with the session.
- `FROM`: Remote host or IP address, if available.
- `LOGIN@`: Login time.
- `IDLE`: Time since the terminal was last used.
- `JCPU`: CPU time consumed by processes associated with the terminal.
- `PCPU`: CPU time consumed by the current process.
- `WHAT`: Current command being executed.

To display a shorter output without the header, use:

```bash
w -h
```

To display information about a specific user:

```bash
w root
```

#### Monitoring System Activity with top

The `top` command provides a real-time view of system processes and resource utilization.

```bash
top
```

The interface displays information about CPU usage, memory consumption, system load, and running processes.

Example output:

```text
top - 18:40:12 up 5 days, 3:51, 2 users, load average: 0.35, 0.42, 0.38
Tasks: 145 total, 2 running, 143 sleeping, 0 stopped, 0 zombie

%Cpu(s): 12.5 us, 3.2 sy, 0.0 ni, 83.8 id, 0.3 wa, 0.0 hi, 0.2 si
MiB Mem :  7980.0 total,  1200.0 free,  4200.0 used,  2580.0 buff/cache
MiB Swap:  2048.0 total,  2048.0 free,     0.0 used

PID   USER   PR  NI   VIRT    RES   SHR S  %CPU %MEM  TIME+ COMMAND
1200  root   20   0  1500m   120m   30m S   8.5  1.5  0:12.50 nginx
1450  user   20   0  1000m    80m   20m R   5.2  1.0  0:08.20 firefox
```

Important fields include:

- `PID`: Process ID.
- `USER`: Process owner.
- `PR`: Process scheduling priority.
- `NI`: Nice value.
- `VIRT`: Total virtual memory used by the process.
- `RES`: Physical memory currently occupied by the process.
- `SHR`: Shared memory.
- `S`: Process state.
- `%CPU`: CPU utilization.
- `%MEM`: Percentage of physical memory used.
- `TIME+`: Total CPU time consumed.
- `COMMAND`: Process name or command.

**Important CPU Metrics:**

The CPU summary contains several values:

- `us`: Time spent executing user-space processes.
- `sy`: Time spent executing kernel processes.
- `ni`: Time spent executing processes with adjusted nice values.
- `id`: Percentage of time the CPU was idle.
- `wa`: Time spent waiting for I/O operations.
- `hi`: Time spent handling hardware interrupts.
- `si`: Time spent handling software interrupts.

**Useful top Keyboard Shortcuts:**

| Shortcut | Description |
|---|---|
| `P` | Sort processes by CPU usage. |
| `M` | Sort processes by memory usage. |
| `T` | Sort processes by CPU time. |
| `k` | Send a signal to a process. |
| `r` | Change a process's nice value. |
| `u` | Filter processes by user. |
| `1` | Display individual CPU core usage. |
| `q` | Exit top. |

**Running top in Batch Mode:**

The `-b` option runs top in batch mode, making it useful for scripts and logging.

```bash
top -b -n 1
```

This command collects one snapshot of system activity and exits.

To display only the first ten lines:

```bash
top -b -n 1 | head -n 10
```

#### Monitoring Processes with ps

The `ps` command displays information about currently running processes.

Unlike `top`, which continuously updates its output, `ps` normally provides a snapshot of process activity.

**Displaying Processes Associated with the Current Terminal:**

```bash
ps
```

**Displaying All Processes:**

```bash
ps aux
```

The command displays processes belonging to all users, including those without an associated terminal.

Important columns include:

- `USER`: Process owner.
- `PID`: Process ID.
- `%CPU`: CPU utilization.
- `%MEM`: Memory utilization.
- `VSZ`: Virtual memory size.
- `RSS`: Resident physical memory.
- `TTY`: Associated terminal.
- `STAT`: Process state.
- `START`: Process start time.
- `TIME`: Accumulated CPU time.
- `COMMAND`: Executed command.

**Displaying Processes in Tree Format:**

```bash
ps auxf
```

This format helps identify parent-child relationships between processes.

**Displaying CPU and Memory Usage:**

```bash
ps -eo pid,ppid,user,%cpu,%mem,stat,comm --sort=-%cpu
```

This command displays selected process information and sorts processes by CPU usage in descending order.

To sort by memory usage:

```bash
ps -eo pid,ppid,user,%cpu,%mem,stat,comm --sort=-%mem
```

#### Monitoring System Load Through /proc/loadavg

Linux provides kernel information through the `/proc` virtual filesystem.

The `/proc/loadavg` file contains system load information.

```bash
cat /proc/loadavg
```

Example output:

```text
0.35 0.42 0.38 2/145 2500
```

The fields represent:

1. Average load over the last minute.
2. Average load over the last five minutes.
3. Average load over the last fifteen minutes.
4. Number of currently runnable tasks divided by the total number of existing tasks.
5. Process ID of the most recently created process.

This file provides a quick way to inspect load information without using additional monitoring tools.

#### Practical Examples

**Check How Long the System Has Been Running:**

```bash
uptime -p
```

**Check the Last Boot Time:**

```bash
uptime -s
```

**Compare Load Average with CPU Cores:**

```bash
uptime
nproc
```

**Find Processes Consuming the Most CPU:**

```bash
ps aux --sort=-%cpu | head -n 10
```

**Find Processes Consuming the Most Memory:**

```bash
ps aux --sort=-%mem | head -n 10
```

**Monitor System Activity Continuously:**

```bash
top
```

**Display Current Logged-in Users:**

```bash
w
```

---

### Monitoring Authentication Logs

Authentication monitoring is an important part of Linux system administration and security.

Authentication logs record activities related to user authentication and access attempts, including successful logins, failed login attempts, SSH connections, and authentication-related errors.

Monitoring these logs helps administrators detect unauthorized access attempts, investigate suspicious activity, and understand who is accessing the system.

#### Understanding Authentication Logs

Linux systems record authentication-related events using different logging mechanisms depending on the distribution and logging configuration.

Common authentication log locations include:

| Distribution | Authentication Log |
|---|---|
| Debian | `/var/log/auth.log` |
| Ubuntu | `/var/log/auth.log` |
| RHEL | `/var/log/secure` |
| CentOS | `/var/log/secure` |
| Fedora | `/var/log/secure` |

These files may require root privileges to read.

For example, on Debian and Ubuntu:

```bash
sudo less /var/log/auth.log
```

On RHEL-based systems:

```bash
sudo less /var/log/secure
```

**Important:** Not every Linux distribution uses the same logging configuration. Some systems primarily store authentication events in the systemd journal instead of traditional log files.

#### Understanding Authentication Log Entries

An authentication log entry generally contains a timestamp, hostname, service or process name, process ID, and an event message.

Example:

```text
Nov 23 18:10:15 server sshd[1200]: Accepted publickey for user from 192.168.1.10 port 52340 ssh2
Nov 23 18:12:30 server sshd[1250]: Failed password for invalid user admin from 192.168.1.20 port 42310 ssh2
Nov 23 18:15:42 server sudo[1300]: user : TTY=pts/0 ; PWD=/home/user ; USER=root ; COMMAND=/usr/bin/apt update
```

These entries describe different authentication-related activities:

- `Accepted publickey`: A successful SSH login using public-key authentication.
- `Failed password`: An unsuccessful password authentication attempt.
- `invalid user`: An attempt to authenticate using a username that does not exist.
- `sudo`: A user executed a command with elevated privileges.

#### Monitoring Logs with journalctl

`journalctl` is the command-line utility used to query and inspect logs stored by systemd-journald.

It is particularly useful on modern Linux systems because it can display authentication events and other system logs in a centralized interface.

**Displaying All Available Journal Entries:**

```bash
sudo journalctl
```

**Displaying the Most Recent Entries:**

```bash
sudo journalctl -n 50
```

This command displays the latest 50 log entries.

**Following Logs in Real Time:**

```bash
sudo journalctl -f
```

The `-f` option continuously displays new log entries as they arrive.

Press `Ctrl+C` to stop following the logs.

**Displaying Logs from the Current Boot:**

```bash
sudo journalctl -b
```

**Displaying Logs from the Previous Boot:**

```bash
sudo journalctl -b -1
```

This is useful when investigating problems that occurred before the most recent restart, provided the journal contains persistent records from that boot.

**Filtering Logs by Time:**

```bash
sudo journalctl --since "1 hour ago"
```

Display logs from a specific date:

```bash
sudo journalctl --since "2026-11-23 00:00:00"
```

Display logs within a specific time range:

```bash
sudo journalctl --since "2026-11-23 10:00:00" --until "2026-11-23 18:00:00"
```

**Filtering SSH Authentication Logs:**

On many systems, SSH authentication events can be inspected using:

```bash
sudo journalctl -u ssh
```

On distributions where the SSH service is named `sshd`, use:

```bash
sudo journalctl -u sshd
```

To follow SSH service logs in real time:

```bash
sudo journalctl -u sshd -f
```

The correct service name depends on the distribution.

#### Monitoring Successful and Failed Login Attempts

Linux provides several commands for examining login history and authentication activity.

**Using last:**

The `last` command reads the `/var/log/wtmp` database and displays information about previous login sessions.

```bash
last
```

Example output:

```text
user     pts/0    192.168.1.10     Mon Nov 23 17:30   still logged in
root     pts/1    192.168.1.15     Mon Nov 23 16:10 - 16:45  (00:35)
reboot   system boot  6.8.0         Wed Nov 18 14:48
```

The output may include:

- Username.
- Terminal.
- Remote hostname or IP address.
- Login time.
- Logout time.
- Session duration.

**Displaying the Most Recent Login Entries:**

```bash
last -n 10
```

**Displaying Failed Login Attempts with lastb:**

The `lastb` command reads the `/var/log/btmp` database, which stores failed login attempts when the system is configured to record them.

```bash
sudo lastb
```

To display the most recent failed login attempts:

```bash
sudo lastb -n 10
```

The `btmp` file may not exist or may be unavailable on every distribution.

**Checking the Last Login of Each User:**

The `lastlog` command displays the most recent login information for user accounts.

```bash
lastlog
```

To inspect a specific user:

```bash
lastlog -u root
```

#### Searching Authentication Logs with grep

The `grep` command can filter authentication logs to locate specific events.

**Finding Failed Password Attempts:**

On Debian and Ubuntu:

```bash
sudo grep "Failed password" /var/log/auth.log
```

On RHEL-based systems:

```bash
sudo grep "Failed password" /var/log/secure
```

**Finding Successful SSH Logins:**

```bash
sudo grep "Accepted" /var/log/auth.log
```

**Finding Invalid User Attempts:**

```bash
sudo grep "Invalid user" /var/log/auth.log
```

**Finding sudo Activity:**

```bash
sudo grep "sudo" /var/log/auth.log
```

**Counting Failed Password Attempts:**

```bash
sudo grep -c "Failed password" /var/log/auth.log
```

**Displaying the Most Recent Failed Attempts:**

```bash
sudo grep "Failed password" /var/log/auth.log | tail -n 10
```

The examples above assume a traditional authentication log file. On systems using the systemd journal, similar searches can be performed with `journalctl` and `grep`.

#### Monitoring Authentication Activity in Real Time

Administrators can monitor authentication events as they occur.

For traditional authentication logs:

```bash
sudo tail -f /var/log/auth.log
```

For systems using the journal:

```bash
sudo journalctl -f
```

To follow SSH service events:

```bash
sudo journalctl -u sshd -f
```

This is useful for observing login attempts while testing SSH access or investigating suspicious activity.

#### Understanding Log Rotation

System logs can grow continuously as new events are recorded.

Linux commonly uses `logrotate` to manage log files by rotating, compressing, and removing older logs according to configured policies.

For example, an authentication log may be rotated into files such as:

```text
auth.log
auth.log.1
auth.log.2.gz
auth.log.3.gz
```

To inspect the logrotate configuration:

```bash
cat /etc/logrotate.conf
```

To inspect additional configuration files:

```bash
ls /etc/logrotate.d/
```

To view a compressed log file:

```bash
sudo zcat /var/log/auth.log.2.gz
```

To search inside a compressed log file:

```bash
sudo zgrep "Failed password" /var/log/auth.log.2.gz
```

#### Practical Examples

**Display the Last Ten Login Sessions:**

```bash
last -n 10
```

**Count Failed SSH Password Attempts:**

```bash
sudo grep -c "Failed password" /var/log/auth.log
```

**Find SSH Login Attempts from a Specific IP Address:**

```bash
sudo grep "192.168.1.20" /var/log/auth.log
```

**Monitor New Authentication Events:**

```bash
sudo tail -f /var/log/auth.log
```

**Inspect Failed Login Attempts:**

```bash
sudo lastb -n 20
```

**Security Note:** A large number of failed login attempts, repeated attempts against invalid usernames, or unexpected successful logins may indicate suspicious activity. However, a failed login alone does not prove that a system has been compromised.

---

### Checking Running Services

Linux services are background processes that provide functionality to the operating system and other applications.

Examples include SSH servers, web servers, database servers, networking services, and scheduled task managers.

Modern Linux distributions commonly use `systemd` as their initialization and service management system.

The `systemctl` command is the primary interface for managing and inspecting systemd services.

#### Understanding systemd

`systemd` is an initialization system and service manager responsible for starting and managing system services during boot and while the system is running.

It manages different types of units, including:

- `service`: Background services and daemons.
- `socket`: IPC and network sockets.
- `target`: Groups of units used to organize system states.
- `mount`: Filesystem mount points.
- `timer`: Scheduled tasks.
- `device`: Devices managed by systemd.

A systemd service is commonly represented by a unit file with the `.service` extension.

For example:

```text
ssh.service
nginx.service
cron.service
```

#### Listing Running Services

The `systemctl` command can display services that are currently active.

```bash
systemctl list-units --type=service
```

This command lists loaded service units.

To display only running services:

```bash
systemctl list-units --type=service --state=running
```

Example output:

```text
UNIT            LOAD   ACTIVE   SUB     DESCRIPTION
ssh.service     loaded active   running OpenBSD Secure Shell server
cron.service    loaded active   running Regular background program processing daemon
nginx.service   loaded active   running A high performance web server
```

Important columns include:

- `UNIT`: Name of the service.
- `LOAD`: Whether the unit configuration has been loaded.
- `ACTIVE`: High-level activation state.
- `SUB`: Detailed state of the unit.
- `DESCRIPTION`: Description of the service.

**Listing All Loaded Service Units:**

```bash
systemctl list-units --type=service --all
```

**Listing Installed Service Unit Files:**

```bash
systemctl list-unit-files --type=service
```

This command displays installed service unit files and their enablement states, including services that are not currently loaded or running.

#### Checking the Status of a Service

The `systemctl status` command displays detailed information about a particular service.

```bash
systemctl status ssh
```

On some distributions, the service may be named `sshd`:

```bash
systemctl status sshd
```

Example output:

```text
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/usr/lib/systemd/system/ssh.service; enabled)
     Active: active (running) since Mon 2026-11-23 14:50:00
       Docs: man:sshd(8)
             man:sshd_config(5)
   Main PID: 1200 (sshd)
      Tasks: 1
     Memory: 2.5M
        CPU: 120ms
     CGroup: /system.slice/ssh.service
             └─1200 /usr/sbin/sshd -D
```

Important fields include:

- `Loaded`: Whether the service unit file is loaded and its location.
- `Active`: Current service state.
- `Main PID`: Main process ID.
- `Tasks`: Number of tasks associated with the service.
- `Memory`: Memory currently used by the service.
- `CPU`: CPU time consumed by the service.
- `CGroup`: Control group associated with the service.

Common service states include:

| State | Description |
|---|---|
| `active (running)` | Service is currently running. |
| `active (exited)` | Service completed its task successfully but has no continuously running process. |
| `inactive` | Service is not currently active. |
| `failed` | Service encountered an error. |
| `activating` | Service is in the process of starting. |
| `deactivating` | Service is in the process of stopping. |

#### Checking Whether a Service Is Active

The `is-active` command checks whether a service is currently active.

```bash
systemctl is-active ssh
```

Possible output:

```text
active
```

Other possible results include:

```text
inactive
failed
unknown
```

This command is particularly useful in shell scripts because its exit status can be used in conditional statements.

Example:

```bash
if systemctl is-active --quiet ssh; then
    echo "SSH service is running."
else
    echo "SSH service is not running."
fi
```

#### Checking Whether a Service Starts at Boot

A service can be running without being configured to start automatically during system boot.

The `is-enabled` command checks whether a service is enabled.

```bash
systemctl is-enabled ssh
```

Possible results include:

- `enabled`: Configured to start automatically according to its unit configuration.
- `disabled`: Not enabled for automatic startup.
- `static`: Cannot be enabled directly because it does not contain the required installation configuration.
- `masked`: Prevented from being started manually or automatically through its normal unit name.
- `indirect`: Enabled indirectly through another unit or alias.

#### Managing Services

Although the primary goal of this section is monitoring, understanding basic service management commands is useful when troubleshooting.

**Starting a Service:**

```bash
sudo systemctl start nginx
```

**Stopping a Service:**

```bash
sudo systemctl stop nginx
```

**Restarting a Service:**

```bash
sudo systemctl restart nginx
```

**Reloading Service Configuration:**

```bash
sudo systemctl reload nginx
```

A reload asks the service to reload its configuration without performing a full restart, if the service supports it.

**Enabling a Service at Boot:**

```bash
sudo systemctl enable nginx
```

**Disabling Automatic Startup:**

```bash
sudo systemctl disable nginx
```

**Enabling and Starting a Service Immediately:**

```bash
sudo systemctl enable --now nginx
```

**Disabling and Stopping a Service:**

```bash
sudo systemctl disable --now nginx
```

**Important:** `enable` and `start` perform different operations. Enabling configures automatic startup, while starting launches the service immediately.

#### Monitoring Service Logs

When a service fails or behaves unexpectedly, its logs can help identify the cause.

The `journalctl` command can display logs associated with a specific service.

**Displaying Service Logs:**

```bash
sudo journalctl -u nginx
```

**Displaying the Most Recent Service Logs:**

```bash
sudo journalctl -u nginx -n 50
```

**Following Service Logs in Real Time:**

```bash
sudo journalctl -u nginx -f
```

**Displaying Logs from the Current Boot:**

```bash
sudo journalctl -u nginx -b
```

**Displaying Only Error-Level Messages:**

```bash
sudo journalctl -u nginx -p err
```

This helps identify errors generated by the service.

#### Inspecting Failed Services

The `systemctl --failed` command displays units that have entered a failed state.

```bash
systemctl --failed
```

Example output:

```text
UNIT             LOAD   ACTIVE   SUB      DESCRIPTION
nginx.service    loaded failed   failed   A high performance web server
```

To investigate a failed service:

```bash
systemctl status nginx
```

Then inspect its logs:

```bash
sudo journalctl -u nginx -n 50
```

This process helps identify problems such as invalid configuration files, unavailable ports, missing dependencies, or permission errors.

#### Understanding Service Unit Files

Systemd unit files define how services are loaded, started, and managed.

Common locations include:

```text
/etc/systemd/system/
/usr/lib/systemd/system/
/lib/systemd/system/
```

The exact layout depends on the distribution.

To inspect a service unit file and its related configuration:

```bash
systemctl cat nginx
```

To display the path of the unit file:

```bash
systemctl show nginx -p FragmentPath
```

After changing a systemd unit file, reload the systemd manager configuration:

```bash
sudo systemctl daemon-reload
```

This command makes systemd reread unit files. It does not automatically restart running services.

#### Practical Examples

**List All Running Services:**

```bash
systemctl list-units --type=service --state=running
```

**Check the Status of SSH:**

```bash
systemctl status ssh
```

**Find Failed Services:**

```bash
systemctl --failed
```

**Check Whether Nginx Is Running:**

```bash
systemctl is-active nginx
```

**Check Whether Nginx Starts at Boot:**

```bash
systemctl is-enabled nginx
```

**Inspect the Last 20 Nginx Log Entries:**

```bash
sudo journalctl -u nginx -n 20
```

---

### Monitoring Memory Usage

Memory monitoring is an essential part of Linux system administration.

Physical memory, commonly called RAM, is used by the kernel, applications, services, and filesystem caches.

Monitoring memory usage helps administrators identify memory-intensive processes, investigate performance problems, and determine whether a system needs additional memory.

Linux also uses swap space as an extension of its virtual memory management system.

#### Understanding Linux Memory Management

Linux uses physical memory for several purposes:

- Running applications and processes.
- Operating system kernel operations.
- Filesystem caching.
- Buffering data during I/O operations.
- Managing shared memory.
- Maintaining internal system structures.

An important characteristic of Linux memory management is that unused RAM is often used for caching to improve system performance.

Therefore, low `free` memory does not automatically mean the system is running out of memory.

Linux can release many cached memory pages when applications need additional RAM.

#### Checking Memory Usage with free

The `free` command displays information about physical memory and swap usage.

```bash
free
```

Example output:

```text
               total        used        free      shared  buff/cache   available
Mem:            7980        3200        1200         250        3580        4200
Swap:           2048           0        2048
```

Important columns include:

- `total`: Total amount of physical memory.
- `used`: Memory currently used, excluding memory counted as available.
- `free`: Memory that is completely unused.
- `shared`: Memory used primarily by tmpfs and shared memory.
- `buff/cache`: Memory used for buffers and caches.
- `available`: An estimate of memory available for starting new applications without swapping.

The `available` column is particularly useful when evaluating whether a system has sufficient memory for additional workloads.

**Displaying Memory in Human-Readable Format:**

```bash
free -h
```

Example output:

```text
               total        used        free      shared  buff/cache   available
Mem:           7.8Gi       3.1Gi       1.2Gi       250Mi       3.5Gi       4.1Gi
Swap:          2.0Gi          0B       2.0Gi
```

**Displaying Memory in Megabytes:**

```bash
free -m
```

**Displaying Memory in Gigabytes:**

```bash
free -g
```

**Continuously Monitoring Memory Usage:**

```bash
free -h -s 2
```

The `-s` option specifies the refresh interval in seconds.

This command updates memory information every two seconds.

Press `Ctrl+C` to stop it.

#### Understanding Buffers and Cache

Linux uses available memory to improve system performance.

Two important concepts are buffers and cache.

**Buffers:**

Buffers are memory areas used by the kernel to manage certain I/O operations and metadata.

**Cache:**

Cache stores recently accessed data, such as filesystem contents, so that future access can be faster.

The kernel can reclaim much of this memory when applications need it.

This explains why the `free` column may be relatively small even when the system has sufficient available memory.

#### Inspecting Memory Information Through /proc/meminfo

The `/proc/meminfo` file provides detailed information about the system's memory usage.

```bash
cat /proc/meminfo
```

Example output:

```text
MemTotal:        8171520 kB
MemFree:         1228800 kB
MemAvailable:    4300800 kB
Buffers:          250000 kB
Cached:          2800000 kB
SwapCached:            0 kB
Active:          2100000 kB
Inactive:        1800000 kB
SwapTotal:       2097152 kB
SwapFree:        2097152 kB
```

Important fields include:

| Field | Description |
|---|---|
| `MemTotal` | Total usable physical RAM. |
| `MemFree` | Completely unused physical memory. |
| `MemAvailable` | Estimated memory available for new applications. |
| `Buffers` | Memory used for kernel buffers. |
| `Cached` | Memory used for filesystem caching. |
| `SwapTotal` | Total configured swap space. |
| `SwapFree` | Currently unused swap space. |
| `Active` | Memory that has been recently used and is less likely to be reclaimed immediately. |
| `Inactive` | Memory that has been less recently used and may be more easily reclaimed. |

To display only selected memory values:

```bash
grep -E 'MemTotal|MemFree|MemAvailable|SwapTotal|SwapFree' /proc/meminfo
```

#### Monitoring Swap Usage

Swap is a portion of storage space used to hold memory pages that are not currently required in physical RAM.

It can help the system handle memory pressure, but accessing data from swap is generally slower than accessing RAM.

Swap can be implemented using a dedicated disk partition or a swap file.

**Displaying Swap Usage:**

```bash
swapon --show
```

Example output:

```text
NAME       TYPE      SIZE   USED PRIO
/swapfile  file      2G     0B   -2
```

**Displaying Swap Information with free:**

```bash
free -h
```

The `Swap` row displays total, used, and free swap space.

**Displaying Swap Statistics:**

```bash
vmstat 1 5
```

The `vmstat` command reports information about processes, memory, paging, block I/O, and CPU activity.

The first report generally reflects averages since boot. Subsequent reports show activity over the specified interval.

Important memory-related columns include:

- `swpd`: Amount of virtual memory currently used.
- `free`: Amount of idle memory.
- `buff`: Memory used as buffers.
- `cache`: Memory used as cache.
- `si`: Memory swapped in per second.
- `so`: Memory swapped out per second.

Consistent swap activity may indicate memory pressure, but swap usage alone does not necessarily indicate a problem.

#### Identifying Memory-Intensive Processes

The `top` command can display memory consumption for running processes.

```bash
top
```

Press `M` to sort processes by memory usage.

You can also use `ps` to list processes by memory consumption.

```bash
ps aux --sort=-%mem | head -n 10
```

This command displays the ten processes with the highest memory utilization.

For a more customized output:

```bash
ps -eo pid,user,%mem,rss,comm --sort=-%mem | head -n 10
```

The `RSS` column represents resident physical memory in kilobytes.

#### Understanding OOM Killer

Linux includes an Out-Of-Memory (OOM) mechanism that can terminate processes when the system cannot satisfy memory allocation requests.

This mechanism helps prevent the entire system from becoming unresponsive due to severe memory exhaustion.

The kernel may record OOM-related events in system logs.

To search for OOM messages:

```bash
sudo journalctl -k | grep -i 'out of memory'
```

You can also search for processes killed by the OOM killer:

```bash
sudo journalctl -k | grep -i 'killed process'
```

These commands are useful when investigating unexpected application termination.

#### Practical Examples

**Display Memory Usage:**

```bash
free -h
```

**Display Detailed Memory Information:**

```bash
cat /proc/meminfo
```

**Display Swap Configuration:**

```bash
swapon --show
```

**Monitor Memory Usage Every Two Seconds:**

```bash
free -h -s 2
```

**Find the Top Ten Memory-Consuming Processes:**

```bash
ps aux --sort=-%mem | head -n 10
```

**Inspect Memory Pressure and Swap Activity:**

```bash
vmstat 1 5
```

**Search for OOM Events:**

```bash
sudo journalctl -k | grep -i 'out of memory'
```

---

### Monitoring Disk Space

Disk monitoring is essential for maintaining a healthy Linux system.

Filesystems require sufficient free space to store application data, system logs, temporary files, packages, and user documents.

When a filesystem becomes full, applications may fail to write data, services may stop working correctly, and the operating system may experience unexpected behavior.

Linux provides several utilities for inspecting disks, partitions, mounted filesystems, and directory sizes.

#### Understanding Disks, Partitions, and Filesystems

Before monitoring disk usage, it is important to understand the difference between a disk, a partition, and a filesystem.

**Disk:**

A physical or virtual storage device used to store data.

Examples include:

```text
/dev/sda
/dev/sdb
/dev/nvme0n1
```

**Partition:**

A logical division of a storage device.

Examples include:

```text
/dev/sda1
/dev/sda2
/dev/nvme0n1p1
```

**Filesystem:**

A structure created on a storage device or partition that organizes and manages files and directories.

Common Linux filesystems include:

- `ext4`
- `XFS`
- `Btrfs`
- `FAT32`
- `NTFS`
- `tmpfs`

A filesystem generally needs to be mounted before its contents can be accessed through the normal directory hierarchy.

#### Checking Filesystem Disk Usage with df

The `df` command displays information about filesystem disk space usage.

```bash
df
```

Example output:

```text
Filesystem      1K-blocks     Used Available Use% Mounted on
/dev/sda2        52428800 20971520  31457280  40% /
/dev/sda1         1048576   200000    848576  20% /boot
/dev/sdb1       104857600 52428800  52428800  50% /home
```

Important columns include:

- `Filesystem`: Storage device or filesystem.
- `1K-blocks`: Total filesystem size in 1 KB units.
- `Used`: Space currently used.
- `Available`: Available space.
- `Use%`: Percentage of filesystem space used.
- `Mounted on`: Mount point.

**Displaying Disk Usage in Human-Readable Format:**

```bash
df -h
```

Example output:

```text
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda2        50G   20G   30G  40% /
/dev/sda1       1.0G  200M  800M  20% /boot
/dev/sdb1       100G   50G   50G  50% /home
```

**Displaying Filesystem Types:**

```bash
df -hT
```

The `-T` option adds a column showing the filesystem type.

Example:

```text
Filesystem     Type  Size  Used Avail Use% Mounted on
/dev/sda2      ext4   50G   20G   30G  40% /
/dev/sda1      ext4  1.0G  200M  800M  20% /boot
```

**Checking a Specific Directory:**

```bash
df -h /home
```

This displays information about the filesystem containing `/home`.

#### Understanding Disk Usage with du

While `df` reports filesystem-level usage, the `du` command estimates the space used by files and directories.

```bash
du
```

This displays disk usage for files and directories in the current location.

**Displaying Human-Readable Sizes:**

```bash
du -h
```

**Displaying the Total Size of a Directory:**

```bash
du -sh /var/log
```

Options:

- `-s`: Display only the total size.
- `-h`: Display sizes in a human-readable format.

Example output:

```text
450M    /var/log
```

**Displaying the Size of Each Subdirectory:**

```bash
du -h --max-depth=1 /var
```

This command displays the size of `/var` and its immediate subdirectories.

**Displaying the Largest Directories:**

```bash
du -h --max-depth=1 /var | sort -hr
```

This command sorts directory sizes in descending order.

To display only the five largest entries:

```bash
du -h --max-depth=1 /var | sort -hr | head -n 5
```

**Important:** `du` estimates the space used by files and directories that it can access. It may not exactly match `df` because of filesystem metadata, reserved blocks, deleted-but-open files, and other filesystem characteristics.

#### Finding Large Files

Large files can consume significant disk space and contribute to filesystem exhaustion.

The `find` command can locate files based on their size.

**Finding Files Larger Than 100 MB:**

```bash
sudo find / -type f -size +100M 2>/dev/null
```

Explanation:

- `/`: Search from the root directory.
- `-type f`: Search only regular files.
- `-size +100M`: Find files larger than 100 MB.
- `2>/dev/null`: Suppress permission and other error messages.

**Finding Files Larger Than 1 GB:**

```bash
sudo find / -type f -size +1G 2>/dev/null
```

**Finding Large Files in a Specific Directory:**

```bash
find /var/log -type f -size +50M
```

**Displaying File Sizes:**

```bash
find /var/log -type f -size +50M -exec ls -lh {} \;
```

This command finds large files and displays their sizes in a readable format.

#### Inspecting Disks and Partitions with lsblk

The `lsblk` command displays information about block devices, including disks, partitions, and their relationships.

```bash
lsblk
```

Example output:

```text
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sda      8:0    0   50G  0 disk
├─sda1   8:1    0    1G  0 part /boot
└─sda2   8:2    0   49G  0 part /
sdb      8:16   0  100G  0 disk
└─sdb1   8:17   0  100G  0 part /home
```

Important columns include:

- `NAME`: Device name.
- `MAJ:MIN`: Major and minor device numbers.
- `RM`: Whether the device is removable.
- `SIZE`: Device or partition size.
- `RO`: Whether the device is read-only.
- `TYPE`: Device type, such as disk or partition.
- `MOUNTPOINTS`: Locations where the device is mounted.

**Displaying Filesystem Information:**

```bash
lsblk -f
```

This command displays additional information, including filesystem type, label, UUID, and mount points.

**Displaying a Specific Device:**

```bash
lsblk /dev/sda
```

#### Understanding Mounted Filesystems with findmnt

The `findmnt` command displays information about mounted filesystems.

```bash
findmnt
```

Example output:

```text
TARGET  SOURCE     FSTYPE OPTIONS
/       /dev/sda2  ext4   rw,relatime
/boot   /dev/sda1  ext4   rw,relatime
/home   /dev/sdb1  ext4   rw,relatime
```

Important columns include:

- `TARGET`: Mount point.
- `SOURCE`: Device or filesystem source.
- `FSTYPE`: Filesystem type.
- `OPTIONS`: Mount options.

**Checking a Specific Mount Point:**

```bash
findmnt /home
```

**Displaying Only Target and Source:**

```bash
findmnt -o TARGET,SOURCE
```

This command is useful for understanding which devices provide the filesystems currently available to the system.

#### Monitoring Inode Usage

Linux filesystems use inodes to store metadata about files and directories.

An inode contains information such as file ownership, permissions, timestamps, and pointers to file data.

A filesystem can run out of available inodes even when it still has free disk space.

This can happen when a filesystem contains a very large number of small files.

**Checking Inode Usage with df:**

```bash
df -i
```

Example output:

```text
Filesystem      Inodes   IUsed   IFree IUse% Mounted on
/dev/sda2      3276800  500000 2776800   16% /
/dev/sdb1      6553600 1200000 5353600   19% /home
```

Important columns include:

- `Inodes`: Total number of available inodes.
- `IUsed`: Number of used inodes.
- `IFree`: Number of free inodes.
- `IUse%`: Percentage of inodes currently used.

**Checking Inode Usage for a Specific Directory:**

```bash
df -i /var
```

If inode usage reaches 100%, the filesystem may be unable to create new files even if `df -h` reports available disk space.

#### Understanding Disk Usage Differences

It is important to understand why `df` and `du` may report different disk usage values.

**df:**

Reports filesystem-level space usage, including space occupied by filesystem metadata and files that may not be visible through the directory hierarchy.

**du:**

Estimates the space used by files and directories that it can access.

Common reasons for differences include:

- Deleted files that are still held open by running processes.
- Filesystem metadata and reserved blocks.
- Files hidden beneath mount points.
- Permission restrictions.
- Filesystem-specific storage behavior.

**Investigating Deleted Files Still in Use:**

A file can be deleted from a directory while a running process continues to hold it open.

In this situation, the file's disk space may remain allocated until the process closes the file.

The `lsof` command can help identify these files.

```bash
sudo lsof +L1
```

This command lists open files that have fewer than one remaining hard link, commonly including deleted files that are still open.

If a deleted log file is held open by a service, restarting or reloading the relevant service may release the space, depending on how the service handles its files.

#### Practical Examples

**Display Filesystem Usage:**

```bash
df -h
```

**Display Filesystem Types:**

```bash
df -hT
```

**Check the Size of the Log Directory:**

```bash
du -sh /var/log
```

**Find the Largest Directories in /var:**

```bash
sudo du -h --max-depth=1 /var | sort -hr | head -n 10
```

**Find Files Larger Than 500 MB:**

```bash
sudo find / -type f -size +500M 2>/dev/null
```

**Display Disk and Partition Information:**

```bash
lsblk -f
```

**Display Mounted Filesystems:**

```bash
findmnt
```

**Check Inode Usage:**

```bash
df -i
```

**Find Deleted Files Still Occupying Disk Space:**

```bash
sudo lsof +L1
```

#### Best Practices for Disk Monitoring

- Monitor filesystem usage regularly using `df -h`.
- Investigate directories consuming unexpected amounts of storage using `du`.
- Check inode usage when applications cannot create new files despite available disk space.
- Monitor log directories because system logs can grow over time.
- Avoid deleting system files without understanding their purpose.
- Investigate deleted-but-open files when `df` and `du` report significantly different usage.
- Maintain sufficient free space for system updates, temporary files, and application operations.

---

## System Monitoring Summary

The following table summarizes the main commands introduced in this section.

| Command | Purpose |
|---|---|
| `uptime` | Display system uptime and load averages. |
| `nproc` | Display available processing units. |
| `lscpu` | Display CPU architecture information. |
| `w` | Display logged-in users and their activities. |
| `top` | Monitor processes and system resource usage in real time. |
| `ps` | Display a snapshot of running processes. |
| `journalctl` | Inspect system and service logs. |
| `last` | Display previous login sessions. |
| `lastb` | Display recorded failed login attempts. |
| `lastlog` | Display the most recent login of user accounts. |
| `grep` | Search authentication logs and other text files. |
| `systemctl` | Inspect and manage systemd services. |
| `free` | Display physical memory and swap usage. |
| `vmstat` | Monitor memory, paging, I/O, and CPU statistics. |
| `df` | Display filesystem disk space usage. |
| `du` | Estimate file and directory disk usage. |
| `find` | Locate files based on specific criteria. |
| `lsblk` | Display block devices and partitions. |
| `findmnt` | Display mounted filesystems. |
| `lsof` | List open files and identify deleted files still in use. |

### Additional Resources

- [Linux Manual Pages](https://man7.org/linux/man-pages/)
- [systemd Documentation](https://www.freedesktop.org/software/systemd/man/)
- [GNU Coreutils Documentation](https://www.gnu.org/software/coreutils/manual/)
- [Linux Kernel Documentation](https://docs.kernel.org/)
- [procps-ng Documentation](https://gitlab.com/procps-ng/procps)

---


## Practical Exercises

The following exercises are designed to reinforce the system monitoring concepts and commands introduced in this section. Complete each exercise in a Linux terminal and use the appropriate commands to inspect system activity, analyze resource usage, and investigate potential issues.

### Exercise 1 — Checking System Uptime and Load

1. Display the current system uptime using `uptime`.
2. Display uptime in a human-readable format using `uptime -p`.
3. Find the date and time of the last system boot using `uptime -s`.
4. Display the number of available CPU processing units using `nproc`.
5. Inspect detailed CPU information using `lscpu`.
6. Display the current load averages and identify their corresponding time intervals.
7. Compare the load average with the number of available CPU cores.
8. Display currently logged-in users and their activities using `w`.
9. Monitor CPU usage and running processes using `top`.
10. Display the ten processes consuming the most CPU resources using `ps`.

### Exercise 2 — Monitoring Authentication Logs

1. Identify the authentication log location used by your Linux distribution.
2. Display the latest authentication log entries using `tail`.
3. Search for failed password attempts using `grep`.
4. Search for successful SSH authentication events.
5. Find authentication attempts involving invalid usernames.
6. Count the number of failed password attempts.
7. Display the most recent login sessions using `last`.
8. Check recorded failed login attempts using `lastb`, if available.
9. Display the most recent login information for user accounts using `lastlog`.
10. Monitor new authentication events in real time using `tail -f` or `journalctl -f`.

### Exercise 3 — Checking Running Services

1. List all loaded service units using `systemctl`.
2. Display only currently running services.
3. List installed service unit files.
4. Check the status of the SSH service.
5. Check whether the SSH service is currently active using `systemctl is-active`.
6. Determine whether the SSH service is enabled at boot using `systemctl is-enabled`.
7. Display all failed systemd units.
8. Inspect the logs of a selected service using `journalctl`.
9. Display the latest 20 log entries of a service.
10. Monitor a service's logs in real time.

### Exercise 4 — Monitoring Memory Usage

1. Display current memory usage using `free`.
2. Display memory information in a human-readable format using `free -h`.
3. Identify total, free, and available memory.
4. Inspect detailed memory information using `/proc/meminfo`.
5. Display the current swap configuration using `swapon --show`.
6. Monitor memory usage at two-second intervals.
7. Display memory and swap statistics using `vmstat`.
8. Identify the ten processes consuming the most memory.
9. Search kernel logs for Out-Of-Memory (OOM) events.
10. Explain the difference between free memory and available memory.

### Exercise 5 — Monitoring Disk Space

1. Display filesystem disk usage using `df`.
2. Display disk usage in a human-readable format using `df -h`.
3. Identify the filesystem type of each mounted filesystem using `df -hT`.
4. Check the available space on the filesystem containing `/home`.
5. Display the total size of `/var/log` using `du`.
6. Find the largest subdirectories inside `/var`.
7. Identify files larger than 100 MB using `find`.
8. Display available disks and partitions using `lsblk`.
9. Inspect filesystem information, UUIDs, and mount points using `lsblk -f`.
10. Display currently mounted filesystems using `findmnt`.
11. Check inode usage using `df -i`.
12. Identify deleted files that are still occupying disk space using `lsof`, if available.

### Exercise 6 — System Monitoring with Command Pipelines

1. Display the ten processes consuming the most CPU resources using `ps` and `head`.
2. Display the ten processes consuming the most memory using `ps` and `sort`.
3. Search authentication logs for failed login attempts and display the latest entries.
4. Count failed password attempts using `grep` and `wc`.
5. Find the largest directories inside `/var` using `du`, `sort`, and `head`.
6. Extract memory-related information from `/proc/meminfo` using `grep`.
7. Display only running services and save the output to a file using `tee`.
8. Combine `journalctl` and `grep` to find error messages generated by a selected service.

### Exercise 7 — System Health Investigation

Perform a basic health inspection of your Linux system.

1. Check system uptime and load averages.
2. Identify the number of available CPU cores.
3. Inspect CPU utilization using `top`.
4. Identify the processes consuming the most CPU and memory.
5. Check available physical memory and swap usage.
6. Inspect filesystem usage and identify partitions with high utilization.
7. Check inode usage across mounted filesystems.
8. Identify failed systemd services.
9. Inspect authentication logs for unusual login activity.
10. Record your findings and identify any potential system issues.

### Exercise 8 — Final Challenge: Linux System Health Report

Create a basic system health report using Linux monitoring commands.

**Objective:** Collect important system information and generate a readable report containing system uptime, CPU load, memory usage, disk space, running services, and authentication activity.

**Requirements:**

1. Create a directory named `system-monitoring`.
2. Create a script named `system-report.sh`.
3. Collect the following information:
   - Current date and time.
   - System hostname.
   - System uptime.
   - Load averages.
   - Number of available CPU cores.
   - Memory and swap usage.
   - Filesystem disk usage.
   - Top five CPU-consuming processes.
   - Top five memory-consuming processes.
   - Number of failed authentication attempts, if accessible.
   - Number of failed systemd services.
4. Organize the collected information into clearly labeled sections.
5. Save the generated report as `system-report.txt`.
6. Display informative messages while the script is running.
7. Handle missing commands, unavailable log files, and permission restrictions gracefully.
8. Make the script executable using `chmod`.
9. Run the script and inspect the generated report.
10. Verify that the report contains the expected system information.

**Expected Directory Structure:**

```text
system-monitoring/
├── system-report.sh
└── system-report.txt
```

**Additional Challenges:**

- Allow the user to specify a custom report filename.
- Include a timestamp in the report.
- Highlight filesystems with more than 80% disk usage.
- Display warnings when available memory becomes unusually low.
- Include the current status of selected services.
- Add error handling and comments to improve script readability.

### Exercise 9 — Knowledge Check

Answer the following questions to evaluate your understanding of Linux system monitoring.

1. What information does the `uptime` command display?
2. What are the three load average values, and what time periods do they represent?
3. Why should load average be compared with the number of CPU cores?
4. What is the difference between `top` and `ps`?
5. Where are authentication logs commonly stored on Debian-based and RHEL-based distributions?
6. What is the purpose of `journalctl`?
7. What is the difference between `last` and `lastb`?
8. What information does `systemctl status` provide?
9. What is the difference between `systemctl start` and `systemctl enable`?
10. How can you identify failed systemd services?
11. What is the difference between free memory and available memory?
12. What is swap space, and why does Linux use it?
13. What information does `/proc/meminfo` provide?
14. What is the purpose of the OOM killer?
15. What is the difference between `df` and `du`?
16. How can you identify files larger than 100 MB?
17. What information does `lsblk` display?
18. What are inodes, and why is monitoring inode usage important?
19. Why might `df` report more disk usage than `du`?
20. How can you identify deleted files that are still occupying disk space?

---

**Goal:** Develop practical skills in monitoring Linux system performance, analyzing authentication logs, inspecting running services, managing memory usage, and identifying disk space issues using essential system monitoring tools.

<p align="right">
  <a href="../Day-05/README.md">← Day 05</a>
  &nbsp; | &nbsp;
  <a href="../Day-07/">Day 07 →</a>
  &nbsp; | &nbsp;
  <a href="#day-06--system-monitoring">Back to top ↑</a>
</p>
