
<div id="top"></div>

<p align="center">
  <a href="https://github.com/mammaddrik/RootBound">
    <img src="https://i.postimg.cc/QtgF51nn/Root-Bound-Day-03.png" alt="RootBound-Day-03">
  </a>
</p>

# Day 03 — Shell and Other Basics

## Table of Contents

- [Day 03 — Shell and Other Basics](#day-03--shell-and-other-basics)
  - [Table of Contents](#table-of-contents)
  - [Shell and Other Basics](#shell-and-other-basics)
    - [Command Path](#command-path)
      - [Introduction](#introduction)
      - [Understanding Command Paths](#understanding-command-paths)
      - [Understanding the PATH Environment Variable](#understanding-the-path-environment-variable)
      - [How the Shell Searches for Commands](#how-the-shell-searches-for-commands)
      - [Locating Executable Files](#locating-executable-files)
      - [Modifying the PATH Variable](#modifying-the-path-variable)
      - [Command Lookup Cache](#command-lookup-cache)
      - [Executable Permissions](#executable-permissions)
      - [Common Command Path Errors](#common-command-path-errors)
      - [Useful Tips](#useful-tips)
    - [Environment Variables](#environment-variables)
    - [Command Help](#command-help)
    - [Redirection](#redirection)
    - [Superuser](#superuser)
  - [Practical Exercises](#practical-exercises)

---

## Shell and Other Basics


### Command Path

#### Introduction

In Linux, commands are executed through the shell. When you enter a command in the terminal, the shell must determine where the corresponding program is located before it can execute it.

For example, when you type:

```bash
ls
```

The shell searches for the `ls` executable and runs it.

Linux uses several mechanisms to locate commands, including command names, absolute paths, relative paths, and the `PATH` environment variable.

Understanding how command lookup works is important when managing software, executing scripts, and troubleshooting command-related errors.

#### Understanding Command Paths

A command path specifies the location of an executable file in the filesystem.

There are three common ways to reference a command:

**1. Absolute Path**

An absolute path specifies the complete location of a file, starting from the root directory (`/`).

Example:

```bash
/bin/ls
/usr/bin/python3
/usr/bin/nano
```

You can execute a program directly using its absolute path:

```bash
/bin/ls
```

Absolute paths work independently of the current working directory.

**2. Relative Path**

A relative path specifies the location of a file in relation to the current working directory.

For example:

```bash
./script.sh
../scripts/script.sh
```

The `.` represents the current directory, while `..` represents the parent directory.

To execute a script located in the current directory:

```bash
./script.sh
```

Unlike commands entered by name, a relative path containing `/` does not require the shell to search through `PATH`.

**3. Command Name**

You can execute a program by entering its command name:

```bash
ls
nano
python3
```

When a command is entered without a path separator (`/`), the shell searches for it using its command lookup rules, including the directories listed in `PATH`.

#### Understanding the PATH Environment Variable

`PATH` is an environment variable that contains a list of directories where the shell searches for executable programs.

Directories are separated by colons (`:`).

To display the current value of `PATH`:

```bash
echo "$PATH"
```

Example output:

```text
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

Each directory is searched when the shell attempts to locate an executable by its command name.

To display each directory on a separate line:

```bash
printf '%s\n' "$PATH" | tr ':' '\n'
```

Example output:

```text
/usr/local/sbin
/usr/local/bin
/usr/sbin
/usr/bin
/sbin
/bin
```

The directories listed in `PATH` may vary depending on the Linux distribution, user configuration, and installed software.

#### How the Shell Searches for Commands

When you enter a command without specifying its path, the shell performs command lookup according to its rules.

For a typical Bash session, the process includes:

1. Recognizing aliases during command parsing.
2. Checking for shell functions.
3. Checking shell builtins.
4. Searching executable files in the directories listed in `PATH`.

For example:

```bash
ls
```

If the shell finds an executable named `ls` in one of the directories listed in `PATH`, it can execute it.

If multiple executables with the same name exist in different directories, the first matching executable found in the search order is normally used.

You can inspect all matching command definitions and executable locations with:

```bash
type -a ls
```

Example output:

```text
ls is aliased to `ls --color=auto'
ls is /usr/bin/ls
```

The exact output depends on your shell configuration.

#### Locating Executable Files

Linux provides several commands and shell features for identifying where commands are located.

**1. command -v**

Displays how the shell interprets a command name.

```bash
command -v ls
```

Example output:

```text
/usr/bin/ls
```

It can also identify aliases, functions, and builtins.

**2. type**

Displays information about a command, including whether it is an alias, function, builtin, or executable.

```bash
type ls
type cd
type echo
```

Example output:

```text
ls is aliased to `ls --color=auto'
cd is a shell builtin
echo is a shell builtin
```

To display all matching definitions:

```bash
type -a python3
```

**3. which**

Displays the location of an executable by searching directories in `PATH`.

```bash
which ls
```

Example output:

```text
/usr/bin/ls
```

Note that `which` is an external utility and may not accurately describe aliases, functions, or shell builtins. For shell-aware command inspection, prefer `type` or `command -v`.

#### Modifying the PATH Variable

Sometimes a program is installed in a directory that is not included in `PATH`.

In this situation, you can add the directory to `PATH` so that its executables can be launched using their command names.

**1. Temporarily Adding a Directory**

To add a directory to `PATH` for the current shell session:

```bash
export PATH="$PATH:/opt/myapp/bin"
```

This appends `/opt/myapp/bin` to the existing search path.

You can also add a directory to the beginning of `PATH`:

```bash
export PATH="/opt/myapp/bin:$PATH"
```

This gives executables in that directory priority over matching executables in directories listed later.

These changes affect the current shell session and its child processes. They do not permanently modify the system configuration.

**2. Permanently Adding a Directory**

To preserve a `PATH` modification across future Bash sessions, add it to your shell configuration file.

For example, open:

```bash
nano ~/.bashrc
```

Add the following line at the end:

```bash
export PATH="$HOME/.local/bin:$PATH"
```

Save the file and reload the configuration:

```bash
source ~/.bashrc
```

The directory `$HOME/.local/bin` is commonly used for executable programs installed locally by an individual user.

**Important:** Different shells use different configuration files. For example, Zsh commonly uses `~/.zshrc`. Login shells may also read files such as `~/.profile` or `~/.bash_profile`, depending on the shell and system configuration.

#### Command Lookup Cache

Some shells, including Bash, remember the locations of previously executed commands to make future command lookups faster.

This information is stored in a command hash table.

To display cached command locations:

```bash
hash -l
```

To display the cached location of a specific command:

```bash
hash -t ls
```

If a program is moved, renamed, or installed in a different location, the shell may continue using an outdated cached location.

To clear the command lookup cache:

```bash
hash -r
```

After clearing the cache, the shell searches for commands again when needed.

#### Executable Permissions

Finding a program does not necessarily mean that you have permission to execute it.

Linux uses file permissions to control whether a file can be executed.

To inspect the permissions of a file:

```bash
ls -l script.sh
```

Example output:

```text
-rw-r--r-- 1 user user 120 Nov 20 10:00 script.sh
```

In this example, the file does not have execute permission.

To make the script executable for its owner:

```bash
chmod u+x script.sh
```

Now it can be executed using:

```bash
./script.sh
```

Alternatively, you can execute a shell script by passing it to Bash:

```bash
bash script.sh
```

This does not require the file itself to have execute permission, because Bash reads and executes the script.

#### Common Command Path Errors

When working with command paths, you may encounter several common errors.

**1. Command Not Found**

Example:

```text
bash: myapp: command not found
```

Possible causes:

- The program is not installed.
- Its directory is not included in `PATH`.
- The command name is incorrect.
- The executable is located somewhere else.

Useful checks:

```bash
command -v myapp
echo "$PATH"
```

**2. Permission Denied**

Example:

```text
bash: ./script.sh: Permission denied
```

Possible cause:

- The file does not have execute permission.
- You do not have sufficient permissions to access the file or its directory.

Possible solution:

```bash
chmod u+x script.sh
```

**3. No Such File or Directory**

Example:

```text
bash: ./script.sh: No such file or directory
```

Possible causes:

- The file does not exist at the specified location.
- The path is incorrect.
- The script's interpreter, specified by its shebang, cannot be found.

Check the file location:

```bash
pwd
ls -l ./script.sh
```

#### Useful Tips

- Use `command -v` to check how the shell resolves a command.
- Use `type -a` to inspect all matching command definitions and executable locations.
- Use `echo "$PATH"` to inspect the current search path.
- Prefer absolute paths when a specific executable location is required.
- Use `./` when executing a program or script from the current directory.
- Avoid adding untrusted directories to `PATH`.
- Be careful when placing writable directories at the beginning of `PATH`, because they can take priority over system commands.
- Use `hash -r` when Bash may be using outdated command locations.


### Environment Variables

### Command Help

### Redirection

### Superuser

---

## Practical Exercises


<p align="right">
  <a href="../Day-02/README.md">← Day 02</a>
  &nbsp; | &nbsp;
  <a href="../Day-04/">Day 04 →</a>
  &nbsp; | &nbsp;
  <a href="#top">Back to top ↑</a>
</p>
