
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
      - [Introduction](#introduction-1)
      - [Understanding Variables in the Shell](#understanding-variables-in-the-shell)
      - [Creating and Assigning Variables](#creating-and-assigning-variables)
      - [Exporting Variables](#exporting-variables)
      - [Viewing Environment Variables](#viewing-environment-variables)
      - [Common Environment Variables](#common-environment-variables)
      - [Removing Variables](#removing-variables)
      - [Temporary and Permanent Environment Variables](#temporary-and-permanent-environment-variables)
      - [Variable Expansion and Quoting](#variable-expansion-and-quoting)
      - [Special Parameter: $?](#special-parameter-)
      - [Useful Tips](#useful-tips-1)
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

#### Introduction

Environment variables are dynamic values used by the operating system and running processes to store configuration information.

They help programs and shell environments share information without requiring configuration values to be hardcoded into applications.

Environment variables are commonly used to define:

- User information.
- Home directory locations.
- Command search paths.
- Default text editors.
- Language and locale settings.
- Temporary file locations.
- Application configuration.

For example, the `HOME` environment variable stores the path to the current user's home directory.

To display its value:

```bash
echo "$HOME"
```

Example output:

```text
/home/user
```

Environment variables are especially useful when configuring applications, writing shell scripts, and managing system environments.

#### Understanding Variables in the Shell

A variable is a named value that can be stored and accessed during a shell session.

In Linux, shell variables are commonly divided into two categories:

**1. Shell Variables**

Shell variables exist within the current shell session and are not automatically passed to child processes.

Example:

```bash
username="LinuxUser"
```

Display the variable:

```bash
echo "$username"
```

Output:

```text
LinuxUser
```

**2. Environment Variables**

Environment variables are variables that are exported to the environment of the current shell and passed to its child processes.

Example:

```bash
export username="LinuxUser"
```

Display the variable:

```bash
echo "$username"
```

Output:

```text
LinuxUser
```

The main difference is that exported variables are inherited by child processes, while ordinary shell variables are not.

#### Creating and Assigning Variables

To create a shell variable, use the following syntax:

```bash
variable_name=value
```

Example:

```bash
name="Alice"
age=25
language="Linux"
```

To access a variable, place `$` before its name:

```bash
echo "$name"
echo "$age"
echo "$language"
```

Output:

```text
Alice
25
Linux
```

**Important Rules:**

- Variable names are case-sensitive.
- Variable names can contain letters, numbers, and underscores.
- Variable names cannot begin with a number.
- Do not use spaces around the assignment operator (`=`).
- Use quotes when assigning values containing spaces or special characters.

Correct:

```bash
username="Linux User"
```

Incorrect:

```bash
username = "Linux User"
```

#### Exporting Variables

The `export` command converts a shell variable into an environment variable.

Syntax:

```bash
export variable_name
```

You can also create and export a variable in one command:

```bash
export APP_NAME="RootBound"
```

To verify its value:

```bash
echo "$APP_NAME"
```

To inspect exported variables:

```bash
export -p
```

Exported variables are available to child processes launched from the current shell.

For example:

```bash
export PROJECT="RootBound"
bash -c 'echo "$PROJECT"'
```

Output:

```text
RootBound
```

The child Bash process inherits the exported variable from its parent shell.

#### Viewing Environment Variables

Linux provides several commands for inspecting environment variables.

**1. printenv**

Displays environment variables.

```bash
printenv
```

To display a specific variable:

```bash
printenv HOME
printenv PATH
printenv USER
```

If the requested variable is not defined in the environment, `printenv` produces no value.

**2. env**

Displays the environment of the current command execution context.

```bash
env
```

You can also temporarily define an environment variable for a single command:

```bash
env APP_MODE=development bash -c 'echo "$APP_MODE"'
```

Output:

```text
development
```

The variable is available to that command and its child processes, but it does not permanently change the current shell environment.

**3. set**

Displays shell variables, environment variables, shell functions, and other shell settings.

```bash
set
```

The output can be extensive because it includes more than environment variables.

**4. echo**

Displays the value of a specific variable.

```bash
echo "$HOME"
echo "$USER"
echo "$SHELL"
```

#### Common Environment Variables

Linux provides several commonly used environment variables.

| Variable | Description |
|---|---|
| `HOME` | Path to the current user's home directory. |
| `USER` | Name of the current user, when set. |
| `LOGNAME` | Login name associated with the session, when set. |
| `SHELL` | Path to the user's configured login shell. |
| `PATH` | Directories searched for executable commands. |
| `PWD` | Current working directory maintained by the shell. |
| `OLDPWD` | Previous working directory maintained by the shell. |
| `TERM` | Terminal type information. |
| `LANG` | Default locale setting. |
| `EDITOR` | Preferred text editor used by some applications. |
| `TMPDIR` | Preferred directory for temporary files, when configured. |

To inspect several common variables:

```bash
echo "$HOME"
echo "$USER"
echo "$SHELL"
echo "$PATH"
echo "$PWD"
echo "$LANG"
```

Note that some variables may be unset or have different values depending on the Linux distribution and shell configuration.

#### Removing Variables

The `unset` command removes a shell variable from the current shell.

Syntax:

```bash
unset variable_name
```

Example:

```bash
export TEMP_NAME="Test"
echo "$TEMP_NAME"
```

Remove it:

```bash
unset TEMP_NAME
```

Verify that it has been removed:

```bash
echo "$TEMP_NAME"
```

The output will be empty if the variable is no longer defined.

Removing a variable from the current shell does not automatically remove copies inherited by processes that were already running.

#### Temporary and Permanent Environment Variables

Environment variables can be configured temporarily or persistently.

**1. Temporary Variables**

Variables created in a terminal session remain available only within that shell and its child processes.

Example:

```bash
export APP_MODE="development"
```

The variable will normally disappear when the shell session ends.

**2. Permanent Variables for a User**

To configure environment variables for future Bash sessions, add them to an appropriate shell configuration file.

For example:

```bash
nano ~/.bashrc
```

Add:

```bash
export EDITOR="nano"
export PROJECT_HOME="$HOME/Projects"
```

Save the file and reload the configuration:

```bash
source ~/.bashrc
```

Verify the variables:

```bash
echo "$EDITOR"
echo "$PROJECT_HOME"
```

**3. System-Wide Environment Variables**

System-wide environment variables can be configured using files such as:

```text
/etc/environment
/etc/profile
/etc/profile.d/
```

These files serve different purposes and are processed according to the login environment and shell configuration.

For example, `/etc/environment` commonly stores system-wide environment variable assignments.

A typical entry looks like:

```text
EDITOR=nano
```

Unlike shell scripts such as `/etc/profile`, `/etc/environment` is not generally interpreted as a shell script. Use its supported assignment format rather than shell commands such as `export`.

Editing system-wide configuration files usually requires administrative privileges.

#### Variable Expansion and Quoting

The shell expands variables before executing a command.

For example:

```bash
name="Linux"
echo "Welcome to $name"
```

Output:

```text
Welcome to Linux
```

There are two important types of quoting.

**1. Double Quotes**

Double quotes allow variable expansion and command substitution.

```bash
name="RootBound"
echo "Welcome to $name"
```

Output:

```text
Welcome to RootBound
```

**2. Single Quotes**

Single quotes preserve the literal contents of the text.

```bash
name="RootBound"
echo 'Welcome to $name'
```

Output:

```text
Welcome to $name
```

This distinction is important when working with variables and writing shell scripts.

#### Special Parameter: $?

The special parameter `$?` stores the exit status of the most recently executed command.

An exit status of `0` usually indicates success, while a non-zero value generally indicates an error or another condition.

Example:

```bash
ls
echo $?
```

If the command succeeds, the output will normally be:

```text
0
```

Another example:

```bash
ls /directory-that-does-not-exist
echo $?
```

The exit status will be non-zero.

You can also use `$?` in conditional statements and shell scripts to check whether a command succeeded.

#### Useful Tips

- Use `printenv` to inspect environment variables.
- Use `export` to make shell variables available to child processes.
- Use `unset` to remove variables from the current shell.
- Use `echo "$VARIABLE"` to display a variable's value.
- Use double quotes to preserve spaces while allowing variable expansion.
- Use single quotes when you want text to remain literal.
- Avoid overwriting important variables such as `PATH` unintentionally.
- Use shell configuration files to configure variables across future sessions.
- Avoid storing passwords, API keys, or other sensitive information in ordinary shell configuration files without appropriate security precautions.


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
