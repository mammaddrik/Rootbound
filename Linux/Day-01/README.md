
<div id="top"></div>

<p align="center">
  <a href="https://github.com/mammaddrik/RootBound">
    <img src="https://i.postimg.cc/qBxPZQM8/Root-Bound-Day-01.png" alt="RootBound-Day-01">
  </a>
</p>

# Day 01 — Linux Fundamentals

## Table of Contents

- [History of Linux](./History.md)
- [Navigation Basics](#navigation-basics)
  - [Basic Commands](#basic-commands)
    - [`pwd` — Print Working Directory](#1-pwd--print-working-directory)
    - [`ls` — List Directory Contents](#2-ls--list-directory-contents)
    - [`cd` — Change Directory](#3-cd--change-directory)
    - [`clear` — Clear Terminal](#4-clear--clear-terminal)
    - [`whoami` — Current User](#5-whoami--current-user)
    - [`date` — Display Date and Time](#6-date--display-date-and-time)
    - [`uname` — System Information](#7-uname--system-information)
    - [`hostname` — System Hostname](#8-hostname--system-hostname)
    - [`history` — Command History](#9-history--command-history)
    - [`man` — Manual Pages](#10-man--manual-pages)
    - [`echo` — Display Text](#11-echo--display-text)
    - [`which` — Locate Executables](#12-which--locate-executables)
    - [`type` — Identify Commands](#13-type--identify-commands)
  - [Creating Files](#creating-files)
    - [`touch` — Create Files](#1-touch--create-files)
    - [`mkdir` — Create Directories](#2-mkdir--create-directories)
    - [`rmdir` — Remove Empty Directories](#3-rmdir--remove-empty-directories)
  - [Moving Files](#moving-files)
    - [`cp` — Copy Files and Directories](#1-cp--copy-files-and-directories)
    - [`mv` — Move and Rename Files](#2-mv--move-and-rename-files)
    - [`rm` — Remove Files and Directories](#3-rm--remove-files-and-directories)
  - [Understanding Directory Hierarchy](#understanding-directory-hierarchy)
    - [Root Directory](#root-directory)
    - [Home Directory](#home-directory)
    - [Important System Directories](#important-system-directories)
    - [Absolute and Relative Paths](#absolute-and-relative-paths)
- [Practical Exercises](#practical-exercises)

---

## Navigation Basics

Navigation is one of the fundamental skills required to work with Linux. Since Linux provides a command-line interface for interacting with the filesystem, users need to understand how to navigate directories, locate files, and manage their contents.

In this section, we will explore essential commands and concepts that help users interact with the Linux filesystem through the terminal.

### Basic Commands

Linux commands allow users to interact with the operating system through the terminal and perform various tasks, such as navigating the filesystem, managing files, and inspecting system information.

Learning basic commands helps beginners understand how Linux works and provides a foundation for performing more advanced operations.

#### Basic Command Reference

| Command | Description | Example |
|---|---|---|
| `pwd` | Displays the current working directory. | `pwd` |
| `ls` | Lists files and directories. | `ls` |
| `ls -l` | Displays detailed information about directory contents. | `ls -l` |
| `ls -a` | Displays all files, including hidden files. | `ls -a` |
| `cd` | Changes the current working directory. | `cd /home` |
| `clear` | Clears the terminal screen. | `clear` |
| `whoami` | Displays the current username. | `whoami` |
| `date` | Displays the current system date and time. | `date` |
| `uname` | Displays system information. | `uname -a` |
| `hostname` | Displays the system hostname. | `hostname` |
| `history` | Displays previously executed commands. | `history` |
| `man` | Displays the manual page of a command. | `man ls` |
| `echo` | Prints text or variable values. | `echo "Hello, Linux"` |
| `which` | Locates an executable in the system PATH. | `which ls` |
| `type` | Identifies how a command is interpreted by the shell. | `type cd` |

#### 1. `pwd` — Print Working Directory

The `pwd` command displays the absolute path of the current working directory.

It helps users identify their current location within the filesystem.

**Syntax:**

```bash
pwd
```

**Example:**

```bash
pwd
```

**Output:**

```text
/home/user/Documents
```

The command prints the absolute path of the directory in which the user is currently working.

<p align="center">
    <img src="https://i.postimg.cc/43dQQqxs/pwd.png" alt="pwd">
</p>

#### 2. `ls` — List Directory Contents

The `ls` command lists files and directories within a specified directory. By default, it displays the contents of the current working directory.

**Syntax:**

```bash
ls [OPTIONS] [DIRECTORY]
```

**Example:**

```bash
ls
```

**Output:**

```text
Desktop  Documents  Downloads  Music  Pictures  Videos
```

The `ls` command displays visible files and directories. Hidden files are not included by default.

<p align="center">
    <img src="https://i.postimg.cc/RZqWsBwM/ls.png" alt="ls">
</p>

**Common Options:**

| Option | Description |
|---|---|
| `-l` | Displays detailed information. |
| `-a` | Displays all files, including hidden files. |
| `-h` | Displays file sizes in a human-readable format when used with `-l`. |
| `-R` | Lists directory contents recursively. |

**Additional Examples:**

Display detailed information:

```bash
ls -l
```

<p align="center">
    <img src="https://i.postimg.cc/MG2z72DQ/ls-l.png" alt="Display detailed information">
</p>

Display hidden files:

```bash
ls -a
```

<p align="center">
    <img src="https://i.postimg.cc/DZNjGFQ3/ls-a.png" alt="hidden files">
</p>

Combine multiple options:

```bash
ls -lah
```

<p align="center">
    <img src="https://i.postimg.cc/25KMQ4rR/ls-lah.png" alt="multiple options">
</p>

List the contents of a specific directory:

```bash
ls /home/user/Documents
```

<p align="center">
    <img src="https://i.postimg.cc/MKLB8H6S/sd.png" alt="specific directory">
</p>

#### 3. `cd` — Change Directory

The `cd` command changes the current working directory and allows users to navigate between directories.

If executed without arguments, it moves the user to their home directory.

**Syntax:**

```bash
cd [DIRECTORY]
```

**Example:**

```bash
cd Documents
```

The command moves into the `Documents` directory, assuming it exists in the current location.

<p align="center">
    <img src="https://i.postimg.cc/h4NgWQzc/cd.png" alt="Change Directory">
</p>

**Additional Examples:**

Move to the home directory:

```bash
cd
```

<p align="center">
    <img src="https://i.postimg.cc/7Ltd3jSC/hcd.png" alt="home directory">
</p>

Move to the parent directory:

```bash
cd ..
```

<p align="center">
    <img src="https://i.postimg.cc/qqs15F60/bcd.png" alt="parent directory">
</p>

Move to the root directory:

```bash
cd /
```

<p align="center">
    <img src="https://i.postimg.cc/zGBkCv7b/cdroot.png" alt="root directory">
</p>

Move to a specific absolute path:

```bash
cd /home/user/Documents
```

<p align="center">
    <img src="https://i.postimg.cc/tJBxJdL2/cdpath.png" alt="specific absolute path">
</p>

Move to the previous working directory:

```bash
cd -
```

<p align="center">
    <img src="https://i.postimg.cc/vBqY6PZ3/cdpwd.png" alt="previous working directory">
</p>

#### 4. `clear` — Clear Terminal

The `clear` command clears the visible contents of the terminal screen without deleting files or affecting running processes.

**Syntax:**

```bash
clear
```

**Example:**

```bash
clear
```

After execution, the terminal screen is cleared, providing a clean workspace for further commands.

<p align="center">
    <img src="https://i.postimg.cc/VNtZt5sd/clear.png" alt="Before clear">
</p>

<p align="center">
    <img src="https://i.postimg.cc/B6chhyKj/clear-1.png" alt="After clear">
</p>

#### 5. `whoami` — Current User

The `whoami` command displays the username associated with the current effective user ID.

It is useful for identifying which user account is currently executing commands.

**Syntax:**

```bash
whoami
```

**Example:**

```bash
whoami
```

**Output:**

```text
user
```

The output represents the username of the current effective user.

<p align="center">
    <img src="https://i.postimg.cc/VknXBtMb/whoami.png" alt="whoami">
</p>

#### 6. `date` — Display Date and Time

The `date` command displays the current system date and time.

It can also format the output according to specific requirements.

**Syntax:**

```bash
date [OPTIONS] [+FORMAT]
```

**Example:**

```bash
date
```

**Output:**

```text
Wed Nov 18 12:30:00 UTC 2026
```

The output depends on the system clock and configured timezone.

<p align="center">
    <img src="https://i.postimg.cc/zDHwX4Bx/date.png" alt="date">
</p>

Display the date in a custom format:

```bash
date "+%Y-%m-%d"
```

Example output:

```text
2026-11-18
```

<p align="center">
    <img src="https://i.postimg.cc/DzmJpb4g/fdate.png" alt="custom format date">
</p>

#### 7. `uname` — System Information

The `uname` command displays basic information about the operating system and kernel.

**Syntax:**

```bash
uname [OPTIONS]
```

**Example:**

```bash
uname
```

**Output:**

```text
Linux
```

<p align="center">
    <img src="https://i.postimg.cc/G2Nb42bV/uname.png" alt="uname">
</p>

**Common Options:**

| Option | Description |
|---|---|
| `-a` | Displays all available system information. |
| `-s` | Displays the kernel name. |
| `-r` | Displays the kernel release. |
| `-m` | Displays the machine hardware architecture. |
| `-n` | Displays the network node hostname. |

**Additional Example:**

```bash
uname -a
```

This command provides a more detailed overview of the running kernel and system architecture.

<p align="center">
    <img src="https://i.postimg.cc/26BSFv9p/uname-a.png" alt="uname -a">
</p>

#### 8. `hostname` — System Hostname

The `hostname` command displays the name assigned to the computer on the network.

**Syntax:**

```bash
hostname [OPTIONS]
```

**Example:**

```bash
hostname
```

**Output:**

```text
linux-machine
```

The output represents the configured hostname of the system.

<p align="center">
    <img src="https://i.postimg.cc/VsDVBTMs/hostname.png" alt="hostname">
</p>

#### 9. `history` — Command History

The `history` command displays a list of commands previously entered in the current shell session and, depending on shell configuration, commands saved from previous sessions.

**Syntax:**

```bash
history [OPTIONS]
```

**Example:**

```bash
history
```

**Output:**

```text
1  pwd
2  ls
3  cd Documents
4  whoami
5  uname -a
```

The displayed command numbers can be used to reference commands in the shell history.

<p align="center">
    <img src="https://i.postimg.cc/FKrSZP0H/history.png" alt="history">
</p>

#### 10. `man` — Manual Pages

The `man` command displays the manual page for a specified command or system component.

Manual pages provide detailed information about command syntax, available options, and usage.

**Syntax:**

```bash
man [COMMAND]
```

**Example:**

```bash
man ls
```

This opens the manual page for the `ls` command.

<p align="center">
    <img src="https://i.postimg.cc/KYpNwK7q/manls.png" alt="man ls">
</p>

**Common Navigation Keys:**

| Key | Description |
|---|---|
| `Space` | Moves forward one page. |
| `b` | Moves backward one page. |
| `/<pattern>` | Searches for a specific pattern. |
| `n` | Moves to the next search result. |
| `q` | Exits the manual page. |

#### 11. `echo` — Display Text

The `echo` command displays text or the value of variables in the terminal.

It is commonly used in shell scripts and command-line operations.

**Syntax:**

```bash
echo [OPTIONS] [STRING]
```

**Example:**

```bash
echo "Hello, Linux"
```

**Output:**

```text
Hello, Linux
```

<p align="center">
    <img src="https://i.postimg.cc/05BTDQLf/echo.png" alt="echo">
</p>

Display the value of an environment variable:

```bash
echo $HOME
```

Example output:

```text
/home/user
```

<p align="center">
    <img src="https://i.postimg.cc/7LP4XRXD/echohome.png" alt="echo home">
</p>

#### 12. `which` — Locate Executables

The `which` command searches for executable files in directories listed in the `PATH` environment variable.

It helps users identify the location of executable commands.

**Syntax:**

```bash
which [COMMAND]
```

**Example:**

```bash
which ls
```

**Output:**

```text
/usr/bin/ls
```

The output indicates the location of the executable found through the current `PATH`.

<p align="center">
    <img src="https://i.postimg.cc/L4LWYT34/which.png" alt="which">
</p>

#### 13. `type` — Identify Commands

The `type` command identifies how the current shell interprets a specified command.

It can determine whether a command is an alias, shell keyword, function, builtin, or executable file.

**Syntax:**

```bash
type [COMMAND]
```

**Example:**

```bash
type cd
```

**Output:**

```text
cd is a shell builtin
```

<p align="center">
    <img src="https://i.postimg.cc/JnWb9bDQ/type.png" alt="type">
</p>

---

### Creating Files

Linux provides commands for creating files and directories directly from the terminal.

These commands are useful when organizing projects, creating documents, and preparing directory structures.

#### 1. `touch` — Create Files

The `touch` command creates an empty file if it does not already exist.

If the file already exists, it updates its access and modification timestamps.

**Syntax:**

```bash
touch [OPTIONS] FILE
```

**Example:**

```bash
touch notes.txt
```

This creates an empty file named `notes.txt` in the current working directory.

<p align="center">
    <img src="https://i.postimg.cc/2jqfyhV2/touch.png" alt="touch">
</p>


**Create Multiple Files:**

```bash
touch file1.txt file2.txt file3.txt
```

<p align="center">
    <img src="https://i.postimg.cc/W3g6Y556/touch-multiple.png" alt="touch Multiple Files">
</p>

This creates three empty files in the current directory.

#### 2. `mkdir` — Create Directories

The `mkdir` command creates new directories in the filesystem.

**Syntax:**

```bash
mkdir [OPTIONS] DIRECTORY
```

**Example:**

```bash
mkdir Projects
```

This creates a directory named `Projects`.

**Create Multiple Directories:**

```bash
mkdir Documents Images Videos
```

**Create Nested Directories:**

```bash
mkdir -p Projects/Linux/Day-01
```

The `-p` option creates missing parent directories automatically.

**Common Options:**

| Option | Description |
|---|---|
| `-p` | Creates parent directories when necessary. |
| `-v` | Displays a message for each created directory. |

#### 3. `rmdir` — Remove Empty Directories

The `rmdir` command removes empty directories.

It cannot remove directories that contain files or other directories.

**Syntax:**

```bash
rmdir [OPTIONS] DIRECTORY
```

**Example:**

```bash
rmdir EmptyFolder
```

This removes the `EmptyFolder` directory if it is empty.

**Remove Nested Empty Directories:**

```bash
rmdir -p Projects/Linux/Day-01
```

The `-p` option attempts to remove the specified directory and its empty parent directories.

---

### Moving Files

Linux provides several commands for copying, moving, renaming, and removing files and directories.

These operations are essential for organizing data and managing the filesystem.

#### 1. `cp` — Copy Files and Directories

The `cp` command creates copies of files or directories.

**Syntax:**

```bash
cp [OPTIONS] SOURCE DESTINATION
```

**Example:**

```bash
cp file.txt backup.txt
```

This creates a copy of `file.txt` named `backup.txt` in the current directory.

**Copy a file to another directory:**

```bash
cp file.txt Documents/
```

**Copy a directory and its contents:**

```bash
cp -r source_directory destination_directory
```

The `-r` option enables recursive copying, allowing the command to copy directories and their contents.

**Common Options:**

| Option | Description |
|---|---|
| `-r` | Copies directories recursively. |
| `-i` | Prompts before overwriting an existing file. |
| `-v` | Displays information about copied files. |
| `-u` | Copies only when the source is newer or the destination is missing. |

#### 2. `mv` — Move and Rename Files

The `mv` command moves files and directories from one location to another.

It can also be used to rename files and directories.

**Syntax:**

```bash
mv [OPTIONS] SOURCE DESTINATION
```

**Example — Move a File:**

```bash
mv file.txt Documents/
```

This moves `file.txt` into the `Documents` directory.

**Example — Rename a File:**

```bash
mv old_name.txt new_name.txt
```

This renames `old_name.txt` to `new_name.txt`.

**Common Options:**

| Option | Description |
|---|---|
| `-i` | Prompts before overwriting an existing destination. |
| `-v` | Displays information about moved files. |
| `-n` | Prevents overwriting an existing destination. |

#### 3. `rm` — Remove Files and Directories

The `rm` command removes files and directories from the filesystem.

Unlike graphical file managers, `rm` generally does not move deleted files to a trash or recycle bin.

**Syntax:**

```bash
rm [OPTIONS] FILE
```

**Example — Remove a File:**

```bash
rm file.txt
```

**Remove an Empty or Non-Empty Directory Recursively:**

```bash
rm -r directory_name
```

**Common Options:**

| Option | Description |
|---|---|
| `-r` | Removes directories and their contents recursively. |
| `-f` | Forces removal without prompting for confirmation. |
| `-i` | Prompts before each removal. |
| `-v` | Displays information about removed files. |

**Important:** Be careful when using `rm`, especially with `-r` and `-f`. Incorrect paths or options can cause permanent data loss.

---

### Understanding Directory Hierarchy

Linux uses a hierarchical filesystem structure to organize files and directories.

Unlike systems that use separate drive letters for different storage devices, Linux organizes files under a single directory tree that begins at the root directory.

Understanding this structure helps users navigate the filesystem and identify where different types of system files and user data are stored.

#### Root Directory

The root directory is the starting point of the Linux filesystem hierarchy.

It is represented by a forward slash (`/`).

All files and directories in a standard Linux filesystem are organized beneath this directory.

Example:

```text
/
├── bin
├── boot
├── dev
├── etc
├── home
├── lib
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sbin
├── sys
├── tmp
├── usr
└── var
```

#### Home Directory

The home directory contains personal files and settings belonging to regular users.

It is commonly located under `/home`.

For example:

```text
/home/user/
├── Desktop
├── Documents
├── Downloads
├── Music
├── Pictures
└── Videos
```

The tilde character (`~`) is a shell shortcut that represents the current user's home directory.

Example:

```bash
cd ~
```

The root user has a separate home directory located at `/root`.

#### Important System Directories

Linux contains several important directories, each serving a specific purpose.

| Directory | Description |
|---|---|
| `/` | The root of the entire filesystem hierarchy. |
| `/bin` | Contains essential user command binaries; commonly provided through `/usr/bin` on modern systems. |
| `/boot` | Contains files required for the boot process, such as kernel images and bootloader files. |
| `/dev` | Contains device files representing hardware and virtual devices. |
| `/etc` | Contains system-wide configuration files. |
| `/home` | Contains personal directories belonging to regular users. |
| `/lib` | Contains essential shared libraries and kernel modules; commonly linked to `/usr/lib` on modern systems. |
| `/media` | Commonly used for mounting removable media. |
| `/mnt` | Commonly used as a temporary mount point for filesystems. |
| `/opt` | Used for optional application software. |
| `/proc` | A virtual filesystem providing information about processes and the kernel. |
| `/root` | The home directory of the root user. |
| `/run` | Stores runtime data for system processes and services. |
| `/sbin` | Contains system administration utilities; commonly provided through `/usr/sbin` on modern systems. |
| `/srv` | Contains data served by system services. |
| `/sys` | A virtual filesystem exposing information about devices and the kernel. |
| `/tmp` | Used for temporary files. |
| `/usr` | Contains user applications, libraries, documentation, and other shared resources. |
| `/var` | Stores variable data such as logs, caches, and application data. |

#### Absolute and Relative Paths

Linux supports two main ways of specifying file and directory locations: absolute paths and relative paths.

**Absolute Path**

An absolute path specifies the complete location of a file or directory, starting from the root directory (`/`).

Example:

```text
/home/user/Documents/file.txt
```

An absolute path always starts at the root of the filesystem, regardless of the current working directory.

**Relative Path**

A relative path specifies a location based on the current working directory.

Example:

```text
Documents/file.txt
```

This path refers to `file.txt` inside the `Documents` directory, assuming the current directory contains that directory.

**Special Directory References:**

| Symbol | Description |
|---|---|
| `.` | Represents the current directory. |
| `..` | Represents the parent directory. |
| `~` | Represents the current user's home directory. |
| `/` | Represents the root directory. |

**Examples:**

Move to the parent directory:

```bash
cd ..
```

Access a directory inside the current directory:

```bash
cd ./Documents
```

Move two levels upward:

```bash
cd ../../
```

Navigate to the user's home directory:

```bash
cd ~
```

---

## Practical Exercises

The following exercises are designed to help reinforce the commands and concepts introduced in this section.

### Exercise 1 — Basic Navigation

1. Open a Linux terminal.
2. Display your current working directory using `pwd`.
3. List the contents of the current directory using `ls`.
4. Display hidden files using `ls -a`.
5. Display detailed directory information using `ls -l`.
6. Navigate to your home directory using `cd`.
7. Move to the root directory using `cd /`.
8. Return to your home directory.

### Exercise 2 — Working with Files

1. Create a directory named `LinuxPractice`.
2. Enter the newly created directory.
3. Create three empty files:
   - `notes.txt`
   - `commands.txt`
   - `practice.txt`
4. List the directory contents.
5. Create a copy of `notes.txt` named `notes_backup.txt`.
6. Rename `practice.txt` to `exercises.txt`.
7. Move `commands.txt` to another directory.
8. Remove `notes_backup.txt`.

### Exercise 3 — Working with Directories

1. Create a directory named `Projects`.
2. Inside it, create a directory named `Linux`.
3. Inside `Linux`, create a directory named `Day-01`.
4. Display the current working directory.
5. Navigate to the parent directory.
6. Return to the home directory.
7. Remove an empty directory using `rmdir`.

### Exercise 4 — Exploring the Filesystem

1. Navigate to the root directory.
2. List its contents.
3. Explore the `/home` directory.
4. Locate the `/etc` directory.
5. Display your current username.
6. Display the system hostname.
7. Display basic kernel information using `uname -a`.
8. Open the manual page for `ls`.

### Exercise 5 — Command Practice

Perform the following tasks using the appropriate commands:

1. Display the current date and time.
2. Print a message using `echo`.
3. Display your command history.
4. Find the location of the `ls` executable.
5. Determine whether `cd` is a shell builtin or an external executable.
6. Clear the terminal screen.


### Exercise 6 — Command-Line Challenge

Complete the following tasks using only the commands introduced in this section:

1. Navigate to your home directory.
2. Create a directory named `RootBound`.
3. Inside it, create a directory named `Linux`.
4. Inside `Linux`, create a directory named `Day-01`.
5. Navigate to the `Day-01` directory.
6. Create three files:
   - `commands.txt`
   - `notes.txt`
   - `summary.txt`
7. Create a backup of `notes.txt` named `notes_backup.txt`.
8. Rename `summary.txt` to `day01-summary.txt`.
9. Display all files, including hidden files, using a detailed listing.
10. Display your current working directory.
11. Display your username, hostname, and kernel information.
12. Remove `notes_backup.txt`.
13. Navigate back to your home directory.
14. Verify that the directory structure still exists.


**Goal:** Understand the basic Linux filesystem structure and become comfortable navigating directories, managing files, and using essential command-line tools.


---

<p align="right">
  <a href="./History.md">← History</a>
  &nbsp; | &nbsp;
  <a href="../Day-02/README.md">Day 02 →</a>
  &nbsp; | &nbsp;
  <a href="#top">Back to top ↑</a>
</p>
