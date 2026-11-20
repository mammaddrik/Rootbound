
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
      - [Introduction](#introduction-2)
      - [Manual Pages (man)](#manual-pages-man)
      - [Navigating Manual Pages](#navigating-manual-pages)
      - [Command Help Option (--help)](#command-help-option---help)
      - [Bash Builtin Help](#bash-builtin-help)
      - [Info Documentation](#info-documentation)
      - [Whatis Command](#whatis-command)
      - [Apropos Command](#apropos-command)
      - [Whereis Command](#whereis-command)
      - [Searching Documentation Efficiently](#searching-documentation-efficiently)
      - [Useful Tips](#useful-tips-2)
    - [Redirection](#redirection)
      - [Introduction](#introduction-3)
      - [Standard Input, Output, and Error](#standard-input-output-and-error)
      - [Output Redirection (\>)](#output-redirection-)
      - [Appending Output (\>\>)](#appending-output-)
      - [Input Redirection (\<)](#input-redirection-)
      - [Redirecting Standard Error (2\>)](#redirecting-standard-error-2)
      - [Redirecting Standard Output and Error](#redirecting-standard-output-and-error)
      - [Redirecting Output to /dev/null](#redirecting-output-to-devnull)
      - [Here Documents (\<\<)](#here-documents-)
      - [Here Strings (\<\<\<)](#here-strings-)
      - [Pipes (|)](#pipes-)
      - [The tee Command](#the-tee-command)
      - [Practical Redirection Examples](#practical-redirection-examples)
      - [Useful Tips](#useful-tips-3)
    - [Superuser](#superuser)
      - [Introduction](#introduction-4)
      - [Understanding the Root User](#understanding-the-root-user)
      - [Understanding the Difference Between Root and Regular Users](#understanding-the-difference-between-root-and-regular-users)
      - [The sudo Command](#the-sudo-command)
      - [Running Commands as Another User](#running-commands-as-another-user)
      - [Switching to the Root User](#switching-to-the-root-user)
      - [Difference Between sudo and su](#difference-between-sudo-and-su)
      - [The sudoers File](#the-sudoers-file)
      - [Understanding sudoers Entries](#understanding-sudoers-entries)
      - [Checking sudo Permissions](#checking-sudo-permissions)
      - [Common sudo Options](#common-sudo-options)
      - [The Root Shell](#the-root-shell)
      - [The root Account and Root Directory](#the-root-account-and-root-directory)
      - [Running Graphical Applications with Administrative Privileges](#running-graphical-applications-with-administrative-privileges)
      - [Security Best Practices](#security-best-practices)
      - [Common Errors and Troubleshooting](#common-errors-and-troubleshooting)
      - [Useful Tips](#useful-tips-4)
  - [Practical Exercises](#practical-exercises)
    - [Exercise 1 — Working with Command Paths](#exercise-1--working-with-command-paths)
    - [Exercise 2 — Environment Variables](#exercise-2--environment-variables)
    - [Exercise 3 — Exploring Command Documentation](#exercise-3--exploring-command-documentation)
    - [Exercise 4 — Input and Output Redirection](#exercise-4--input-and-output-redirection)
    - [Exercise 5 — Pipes and the tee Command](#exercise-5--pipes-and-the-tee-command)
    - [Exercise 6 — Working with Superuser Privileges](#exercise-6--working-with-superuser-privileges)
    - [Exercise 7 — Command-Line Challenge](#exercise-7--command-line-challenge)
    - [Exercise 8 — Knowledge Check](#exercise-8--knowledge-check)

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

#### Introduction

Linux provides several built-in tools and documentation systems that help users understand commands, their options, and how they work.

Instead of memorizing every command and its available options, users can access documentation directly from the terminal.

These help systems are particularly useful when learning new commands, troubleshooting problems, and exploring advanced command-line features.

The most commonly used help tools include:

- `man`
- `--help`
- `help`
- `info`
- `whatis`
- `apropos`
- `whereis`

#### Manual Pages (man)

The `man` command displays the manual page of a specified command.

Manual pages provide detailed information about commands, including their purpose, syntax, options, and examples.

**Syntax:**

```bash
man [section] command
```

Example:

```bash
man ls
```

This opens the manual page for the `ls` command.

You can also view documentation for other commands:

```bash
man cp
man mv
man mkdir
man chmod
```

**Common Manual Page Sections**

Linux manual pages are divided into numbered sections. Each section covers a different category of documentation.

| Section | Description |
|---|---|
| 1 | User commands and executable programs |
| 2 | System calls |
| 3 | Library functions |
| 4 | Special files and device files |
| 5 | File formats and configuration files |
| 6 | Games and miscellaneous programs |
| 7 | Miscellaneous topics and conventions |
| 8 | System administration commands |
| 9 | Kernel routines (on some systems) |

For example:

```bash
man 1 passwd
man 5 passwd
```

The first command displays the manual for the `passwd` user command, while the second displays documentation for the `/etc/passwd` file format.

To display the available manual page sections for a topic:

```bash
man -f passwd
```

To search for manual pages related to a specific topic:

```bash
man -k password
```

#### Navigating Manual Pages

Manual pages are commonly displayed using a pager such as `less`.

The following keyboard shortcuts help you navigate documentation:

| Shortcut | Description |
|---|---|
| `Space` | Move forward one page |
| `b` | Move backward one page |
| `Enter` | Move down one line |
| `k` | Move up one line |
| `g` | Go to the beginning |
| `G` | Go to the end |
| `/pattern` | Search forward |
| `?pattern` | Search backward |
| `n` | Find the next match |
| `N` | Find the previous match |
| `q` | Quit the manual page |

These shortcuts are also commonly used in the `less` pager.

#### Command Help Option (--help)

Many Linux commands support the `--help` option.

It displays a brief summary of the command's usage, available options, and arguments.

Example:

```bash
ls --help
```

Other examples:

```bash
cp --help
mv --help
mkdir --help
grep --help
```

The output is usually shorter than a complete manual page and is useful when you need a quick reference.

Some programs use `-h` instead of `--help`, while others assign `-h` a different meaning. Check the command's documentation when unsure.

#### Bash Builtin Help

The `help` command is used to display documentation for Bash built-in commands.

Builtins are commands implemented directly by the shell rather than separate executable files.

Examples include:

```bash
help cd
help echo
help export
help read
```

To display a list of available Bash builtins:

```bash
help
```

You can also display help for shell-specific features and keywords:

```bash
help if
help for
help while
```

**Important:** The `help` command is primarily intended for Bash builtins. For external programs such as `ls` or `cp`, use their manual pages or `--help` options instead.

#### Info Documentation

The `info` command displays documentation using the GNU Info documentation system.

Some GNU programs provide more detailed documentation through Info than through their manual pages.

Example:

```bash
info coreutils
```

You can also open documentation for specific programs:

```bash
info bash
info grep
info find
```

Info documentation is organized into nodes, which can contain related topics and links to other sections.

Common Info navigation shortcuts:

| Shortcut | Description |
|---|---|
| `Space` | Move forward |
| `Backspace` | Move backward |
| `n` | Next node |
| `p` | Previous node |
| `u` | Move to the parent node |
| `Enter` | Follow a selected link |
| `q` | Quit |

#### Whatis Command

The `whatis` command displays a short description of a command based on its manual page.

Syntax:

```bash
whatis command
```

Example:

```bash
whatis ls
```

Example output:

```text
ls (1) - list directory contents
```

You can also search for multiple commands:

```bash
whatis cp mv mkdir
```

This command is useful when you only need a brief explanation rather than the complete documentation.

#### Apropos Command

The `apropos` command searches manual page descriptions for a specific keyword.

It is useful when you know what you want to accomplish but do not know which command to use.

Syntax:

```bash
apropos keyword
```

Example:

```bash
apropos directory
```

This displays manual page entries related to directories.

Other examples:

```bash
apropos network
apropos password
apropos archive
```

The results depend on the manual page database available on your system.

On some distributions, the manual page database may need to be updated before searches return complete results:

```bash
sudo mandb
```

#### Whereis Command

The `whereis` command locates the binary, source files, and manual pages associated with a command.

Syntax:

```bash
whereis command
```

Example:

```bash
whereis ls
```

Example output:

```text
ls: /usr/bin/ls /usr/share/man/man1/ls.1.gz
```

You can search for specific types of files using options:

| Option | Description |
|---|---|
| `-b` | Search for binary files |
| `-m` | Search for manual pages |
| `-s` | Search for source files |

Examples:

```bash
whereis -b ls
whereis -m ls
```

Unlike `which`, which focuses on executable lookup through `PATH`, `whereis` searches standard locations for related files.

#### Searching Documentation Efficiently

Different help tools serve different purposes.

The following examples show which command to use depending on your needs:

| Requirement | Command |
|---|---|
| Read complete command documentation | `man command` |
| Get a quick usage summary | `command --help` |
| Read help for a Bash builtin | `help command` |
| Read GNU documentation | `info command` |
| Get a short command description | `whatis command` |
| Search documentation by keyword | `apropos keyword` |
| Locate binaries and manual pages | `whereis command` |

For example, if you want to learn how to create directories:

```bash
man mkdir
```

If you only need a quick reminder of its options:

```bash
mkdir --help
```

If you do not know which command creates directories:

```bash
apropos directory
```

#### Useful Tips

- Use `man` when you need detailed documentation.
- Use `--help` when you need a quick overview of command options.
- Use `help` for Bash built-in commands.
- Use `info` to explore detailed GNU documentation.
- Use `whatis` to get a short description of a command.
- Use `apropos` when you know the topic but not the command name.
- Use `whereis` to locate command binaries and related documentation.
- Read the `DESCRIPTION`, `OPTIONS`, and `EXAMPLES` sections of manual pages carefully.
- Do not assume that every command supports the same help options.



### Redirection

#### Introduction

Redirection is a feature of the Linux shell that allows users to control where commands receive their input and send their output.

By default, commands receive input from the keyboard and display their output in the terminal. However, redirection makes it possible to:

- Save command output to files.
- Append new output to existing files.
- Read input from files.
- Separate normal output from error messages.
- Combine multiple commands.
- Send command output directly to other programs.

Redirection is commonly used in shell scripting, system administration, log management, and automation.

#### Standard Input, Output, and Error

Linux processes use three standard data streams to communicate with their environment.

Each stream has a file descriptor (FD), which is a numerical identifier.

| Stream | File Descriptor | Description | Default Destination |
|---|---:|---|---|
| `stdin` | `0` | Standard input | Keyboard |
| `stdout` | `1` | Standard output | Terminal |
| `stderr` | `2` | Standard error | Terminal |

**1. Standard Input (stdin)**

Standard input is the data received by a command.

By default, commands read input from the keyboard.

Example:

```bash
cat
```

Type some text and press `Ctrl + D` to indicate the end of input.

The `cat` command displays the text you entered.

**2. Standard Output (stdout)**

Standard output is the normal output produced by a command.

Example:

```bash
ls
```

The directory contents are displayed in the terminal through standard output.

**3. Standard Error (stderr)**

Standard error is used to display error messages generated by a command.

Example:

```bash
ls /directory-that-does-not-exist
```

The command produces an error message because the specified directory does not exist.

Standard output and standard error are separate streams, even though they are normally displayed in the same terminal.

#### Output Redirection (>)

The `>` operator redirects standard output to a file.

Syntax:

```bash
command > filename
```

Example:

```bash
ls > files.txt
```

Instead of displaying the directory contents in the terminal, the output is written to `files.txt`.

If the file does not exist, it is created.

If the file already exists, its previous contents are overwritten.

You can display the saved output using:

```bash
cat files.txt
```

**Important:** The `>` operator only redirects standard output. Error messages are still displayed in the terminal unless they are redirected separately.

#### Appending Output (>>)

The `>>` operator appends standard output to the end of a file without removing its existing contents.

Syntax:

```bash
command >> filename
```

Example:

```bash
echo "First line" > notes.txt
echo "Second line" >> notes.txt
echo "Third line" >> notes.txt
```

Display the file:

```bash
cat notes.txt
```

Output:

```text
First line
Second line
Third line
```

The difference between `>` and `>>` is important:

| Operator | Description |
|---|---|
| `>` | Creates a file or overwrites existing contents. |
| `>>` | Creates a file or appends output to existing contents. |

#### Input Redirection (<)

The `<` operator redirects standard input from a file instead of the keyboard.

Syntax:

```bash
command < filename
```

Example:

```bash
sort < names.txt
```

The `sort` command reads the contents of `names.txt` as input and sorts the lines.

Another example:

```bash
wc -l < notes.txt
```

This counts the number of lines in `notes.txt`.

Input redirection is useful when a command expects data from standard input.

#### Redirecting Standard Error (2>)

The `2>` operator redirects standard error to a file.

Syntax:

```bash
command 2> error.log
```

Example:

```bash
ls /invalid-directory 2> error.log
```

The error message is written to `error.log` instead of being displayed in the terminal.

To inspect the saved error:

```bash
cat error.log
```

If the destination file already exists, its previous contents are overwritten.

To append error messages instead of overwriting the file, use:

```bash
command 2>> error.log
```

#### Redirecting Standard Output and Error

Sometimes you want to save both normal output and error messages in the same file.

There are several ways to do this.

**1. Redirect stdout and stderr separately**

```bash
command > output.log 2> error.log
```

This saves normal output in `output.log` and error messages in `error.log`.

**2. Redirect both streams to the same file**

```bash
command > combined.log 2>&1
```

Here:

- `>` redirects standard output to `combined.log`.
- `2>&1` redirects standard error to the current destination of standard output.

**3. Using the &> Operator**

Bash also supports the following shorthand:

```bash
command &> combined.log
```

This redirects both standard output and standard error to the same file, overwriting its previous contents.

To append both streams:

```bash
command &>> combined.log
```

**Important:** Redirection order matters.

For example:

```bash
command > combined.log 2>&1
```

Both streams go to the file.

However:

```bash
command 2>&1 > combined.log
```

Standard error is first connected to the original standard output, which is usually the terminal. Only afterward is standard output redirected to the file.

As a result, standard error may still appear in the terminal.

#### Redirecting Output to /dev/null

Linux provides a special device file called `/dev/null`.

Anything written to this device is discarded.

It is commonly used when you do not want to display or save command output.

**1. Discard Standard Output**

```bash
command > /dev/null
```

Example:

```bash
ls > /dev/null
```

**2. Discard Standard Error**

```bash
command 2> /dev/null
```

Example:

```bash
ls /invalid-directory 2> /dev/null
```

**3. Discard Both Standard Output and Error**

```bash
command > /dev/null 2>&1
```

This prevents both normal output and error messages from appearing in the terminal.

#### Here Documents (<<)

A here document allows users to provide multiple lines of input directly to a command.

It is useful when passing text to commands without creating a separate input file.

Syntax:

```bash
command << DELIMITER
input line 1
input line 2
DELIMITER
```

Example:

```bash
cat << EOF
Welcome to RootBound.
Learning Linux step by step.
Today we are studying redirection.
EOF
```

Output:

```text
Welcome to RootBound.
Learning Linux step by step.
Today we are studying redirection.
```

The delimiter marks the beginning and end of the input block.

The delimiter is commonly written as `EOF`, but other names can also be used.

**Quoted and Unquoted Delimiters**

When the delimiter is unquoted, parameter expansion, command substitution, and certain shell expansions are performed.

Example:

```bash
name="Linux"

cat << EOF
Welcome to $name.
EOF
```

Output:

```text
Welcome to Linux.
```

When the delimiter is quoted, the input is treated literally:

```bash
cat << 'EOF'
Welcome to $name.
EOF
```

Output:

```text
Welcome to $name.
```

#### Here Strings (<<<)

A here string passes a single string as standard input to a command.

Syntax:

```bash
command <<< "text"
```

Example:

```bash
wc -w <<< "Linux is a powerful operating system"
```

Output:

```text
7
```

Another example:

```bash
grep "Linux" <<< "Learning Linux with RootBound"
```

Output:

```text
Learning Linux with RootBound
```

Here strings are supported by Bash and several other shells, but they are not part of basic POSIX shell syntax.

#### Pipes (|)

A pipe connects the standard output of one command to the standard input of another command.

Instead of saving the output to a file, you can pass it directly to another program.

Syntax:

```bash
command1 | command2
```

Example:

```bash
ls | sort
```

The output of `ls` becomes the input of `sort`, which sorts the resulting lines.

Another example:

```bash
cat notes.txt | grep "Linux"
```

This displays lines containing the word `Linux`.

However, when using `grep` to search a file, it is usually simpler to write:

```bash
grep "Linux" notes.txt
```

**Combining Multiple Commands with Pipes**

You can connect several commands together:

```bash
cat access.log | grep "ERROR" | sort
```

This pipeline:

1. Reads the contents of `access.log`.
2. Filters lines containing `ERROR`.
3. Sorts the matching lines.

A pipeline can also be written without unnecessary `cat` commands:

```bash
grep "ERROR" access.log | sort
```

#### The tee Command

The `tee` command reads data from standard input and writes it to both standard output and one or more files.

It is useful when you want to display command output while also saving it.

Syntax:

```bash
command | tee filename
```

Example:

```bash
ls | tee files.txt
```

The directory contents are displayed in the terminal and saved to `files.txt`.

By default, `tee` overwrites the destination file.

To append instead:

```bash
ls | tee -a files.txt
```

You can also write the output to multiple files:

```bash
echo "RootBound Linux Notes" | tee file1.txt file2.txt
```

#### Practical Redirection Examples

The following examples demonstrate how redirection can be used in everyday Linux tasks.

**1. Save System Information**

```bash
uname -a > system-info.txt
```

This saves kernel and system information to a file.

**2. Create a Log File**

```bash
echo "Starting Linux practice" > practice.log
echo "Learning redirection" >> practice.log
echo "Practice completed" >> practice.log
```

**3. Count Lines in a File**

```bash
wc -l < practice.log
```

**4. Search and Save Results**

```bash
grep "Linux" notes.txt > search-results.txt
```

**5. Save Errors Separately**

```bash
find / -name "*.conf" > found-files.txt 2> search-errors.log
```

This stores matching file paths in `found-files.txt` and error messages in `search-errors.log`.

**6. Display and Save Command Output**

```bash
df -h | tee disk-usage.txt
```

This displays disk usage and saves the same output to a file.

#### Useful Tips

- Use `>` when you want to overwrite a file with new output.
- Use `>>` when you want to append output.
- Use `<` to provide input from a file.
- Use `2>` to redirect error messages.
- Use `2>&1` to redirect standard error to the current standard output destination.
- Use `/dev/null` to discard unwanted output.
- Use `|` to connect commands.
- Use `tee` when you want to display and save output simultaneously.
- Be careful with `>` because it can permanently overwrite existing file contents.
- Remember that pipes normally pass standard output, not standard error.


### Superuser

#### Introduction

Linux is a multi-user operating system that uses permissions and user privileges to control access to files, directories, and system resources.

Some operations require administrative privileges because they can modify important system files, install software, manage users, or change system configurations.

The **superuser** is a special account with extensive administrative privileges. In Linux, this account is traditionally named `root`.

Understanding how administrative privileges work is essential for system administration, security, and managing a Linux environment.

#### Understanding the Root User

The `root` account is the default superuser account in Linux.

Unlike ordinary users, root has permission to perform almost any operation on the system, including:

- Installing and removing software.
- Creating, modifying, and deleting system files.
- Managing users and groups.
- Changing file ownership and permissions.
- Starting and stopping system services.
- Managing disks and filesystems.
- Modifying network configurations.
- Accessing files belonging to other users.

The root user has a User ID (UID) of `0`.

To check the current user:

```bash
whoami
```

To display the current user's UID and group information:

```bash
id
```

Example output for a regular user:

```text
uid=1000(user) gid=1000(user) groups=1000(user),27(sudo)
```

Example output for the root user:

```text
uid=0(root) gid=0(root) groups=0(root)
```

The exact group information depends on the system configuration.

#### Understanding the Difference Between Root and Regular Users

Linux separates ordinary user accounts from administrative accounts to improve system security.

| Feature | Regular User | Root User |
|---|---|---|
| UID | Usually starts from 1000 | 0 |
| Administrative privileges | Limited | Extensive |
| Modify system files | Usually restricted | Allowed |
| Install system packages | Requires authorization | Allowed |
| Manage other users | Usually restricted | Allowed |
| Access protected resources | Limited | Extensive |

Regular users should perform everyday activities using their normal accounts.

Administrative privileges should only be used when necessary.

#### The sudo Command

The `sudo` command allows authorized users to execute commands with the privileges of another user, usually the root user.

The name `sudo` stands for **superuser do**.

Syntax:

```bash
sudo command
```

Example:

```bash
sudo apt update
```

This runs the package update command with administrative privileges on Debian-based distributions.

Another example:

```bash
sudo systemctl restart ssh
```

This restarts the SSH service, assuming the service exists and the user has sufficient privileges.

When using `sudo`, the system may ask for the current user's password.

**Important:** The password requested by `sudo` is normally the password of the user executing the command, not the root password.

#### Running Commands as Another User

By default, `sudo` executes commands as root.

However, you can specify another user with the `-u` option.

Syntax:

```bash
sudo -u username command
```

Example:

```bash
sudo -u nobody whoami
```

This executes `whoami` as the user named `nobody`, provided the account exists and the current user is authorized.

To execute a command with root privileges explicitly:

```bash
sudo -u root command
```

#### Switching to the Root User

The `su` command allows users to switch to another user account.

Syntax:

```bash
su username
```

To switch to the root account:

```bash
su -
```

The hyphen starts a login shell and loads the target user's login environment.

On systems where the root account has an enabled password, `su -` normally asks for the root password.

To verify the current user:

```bash
whoami
```

Example output:

```text
root
```

To leave the root shell and return to the previous user:

```bash
exit
```

**Difference Between `su` and `su -`**

| Command | Description |
|---|---|
| `su` | Switches to another user while generally retaining much of the current environment. |
| `su -` | Starts a login shell with the target user's login environment. |
| `su - root` | Starts a login shell as root. |
| `su - username` | Starts a login shell as the specified user. |

#### Difference Between sudo and su

Both commands allow users to work with another user's privileges, but they operate differently.

| Feature | sudo | su |
|---|---|---|
| Main purpose | Execute commands with another user's privileges | Switch to another user |
| Authentication | Usually the invoking user's password | Usually the target user's password |
| Command execution | Can execute a single command | Opens another user's shell |
| Default target | root | root when no username is specified |
| Authorization | Controlled by sudo policy | Controlled by account authentication and system policy |

Example using `sudo`:

```bash
sudo apt update
```

Example using `su`:

```bash
su -
```

For everyday administrative tasks, using `sudo` for individual commands is generally preferable because it limits the time spent with elevated privileges.

#### The sudoers File

The `sudoers` file controls which users and groups are allowed to execute commands using `sudo`.

Its main configuration file is:

```text
/etc/sudoers
```

Additional configuration files may be stored in:

```text
/etc/sudoers.d/
```

The `sudoers` configuration determines:

- Which users can use `sudo`.
- Which commands they can execute.
- Which users they can execute commands as.
- Whether password authentication is required.
- Other administrative access rules.

**Important:** Never edit `/etc/sudoers` directly with a regular text editor.

Use:

```bash
sudo visudo
```

The `visudo` command checks the configuration for syntax errors before saving it, helping prevent mistakes that could make sudo access unusable.

#### Understanding sudoers Entries

A common sudoers entry follows this structure:

```text
user host=(runas_user) commands
```

For example:

```text
alice ALL=(ALL:ALL) ALL
```

This entry allows the user `alice` to execute commands as any user or group on all hosts, subject to the configured sudo policy.

Another common entry is:

```text
%sudo ALL=(ALL:ALL) ALL
```

The `%` symbol indicates a group.

This rule allows members of the `sudo` group to execute commands with elevated privileges.

On some Linux distributions, the administrative group may be named `wheel` instead.

#### Checking sudo Permissions

To check which commands the current user is allowed to execute through `sudo`:

```bash
sudo -l
```

This displays the user's sudo permissions according to the active policy.

To display the current user's groups:

```bash
groups
```

Example output:

```text
user sudo
```

On Debian-based systems, users who belong to the `sudo` group are commonly granted administrative privileges through the default sudoers configuration.

The exact group and permission rules depend on the Linux distribution.

#### Common sudo Options

The `sudo` command supports several useful options.

| Option | Description |
|---|---|
| `-u username` | Execute a command as another user. |
| `-i` | Start an interactive login shell as the target user. |
| `-s` | Start a shell with elevated privileges. |
| `-l` | List the commands the user is allowed to execute. |
| `-k` | Invalidate cached authentication credentials. |
| `-v` | Update the authentication timestamp without running a command. |
| `-H` | Set the target user's home directory as the `HOME` environment variable. |

Examples:

```bash
sudo -l
sudo -u nobody whoami
sudo -i
sudo -k
```

To leave a shell started with `sudo -i`, use:

```bash
exit
```

#### The Root Shell

Sometimes administrators need to execute several commands that require elevated privileges.

Instead of writing `sudo` before every command, they can start a root shell.

Using sudo:

```bash
sudo -i
```

Or, where root authentication is available:

```bash
su -
```

After entering the root shell, commands are executed with root privileges.

Example:

```bash
whoami
```

Output:

```text
root
```

To return to the regular user account:

```bash
exit
```

**Important:** Always leave the root shell when administrative work is complete.

#### The root Account and Root Directory

The terms `root` and `root directory` are related but refer to different things.

| Term | Description |
|---|---|
| `root` user | The administrative user account with UID 0. |
| `/root` | The home directory of the root user. |
| `/` | The top-level directory of the Linux filesystem. |

For example:

```bash
cd /root
```

Navigates to the root user's home directory, provided you have permission to access it.

However:

```bash
cd /
```

Navigates to the root of the filesystem.

These paths should not be confused.

#### Running Graphical Applications with Administrative Privileges

Graphical applications sometimes require administrative privileges to modify system settings or protected files.

However, running graphical applications directly with `sudo` is generally discouraged because it can cause permission problems and security risks.

Prefer terminal-based administrative tools or desktop environments' supported authentication mechanisms.

For example, use:

```bash
sudo nano /etc/hosts
```

when you need to edit a protected configuration file from the terminal.

#### Security Best Practices

Administrative privileges provide extensive control over a Linux system. Incorrect commands can damage the operating system or cause data loss.

Follow these practices when working as root:

- Use a regular user account for everyday activities.
- Use `sudo` only when administrative privileges are required.
- Avoid running unfamiliar commands with `sudo`.
- Check command syntax before executing administrative operations.
- Never share your root password.
- Avoid using `chmod -R 777` as a general solution to permission problems.
- Do not execute scripts from untrusted sources as root.
- Avoid remaining in a root shell longer than necessary.
- Review sudo permissions periodically.
- Be especially careful when deleting files or modifying system directories.

For example, the following command can permanently remove a large amount of data:

```bash
sudo rm -rf /some/directory
```

Always verify the target path before running destructive commands.

#### Common Errors and Troubleshooting

**1. User Is Not in the Sudoers File**

Example:

```text
user is not in the sudoers file. This incident will be reported.
```

Possible cause:

- The user does not have permission to execute commands through `sudo`.

Possible solution:

- Ask an authorized administrator to grant the appropriate permissions.
- On distributions that use the `sudo` group, an administrator can add the user with:

```bash
sudo usermod -aG sudo username
```

The user may need to log out and log back in for the new group membership to take effect.

**2. Authentication Failure**

Example:

```text
Sorry, try again.
```

Possible causes:

- An incorrect password was entered.
- The account authentication configuration prevents access.

Check the password and ensure that the correct user account is being authenticated.

**3. Command Not Found**

Example:

```text
sudo: command not found
```

Possible cause:

- The `sudo` package is not installed or its executable is not available through the command search path.

An administrator may need to install and configure it using the distribution's package management tools.

#### Useful Tips

- Use `whoami` to identify the current user.
- Use `id` to inspect user and group IDs.
- Use `sudo command` to execute individual administrative commands.
- Use `sudo -l` to inspect your sudo permissions.
- Use `sudo -i` to start a root login shell when necessary.
- Use `su -` to switch to the root account when root authentication is configured.
- Use `visudo` when modifying sudoers configuration.
- Use `exit` to leave a root shell.
- Remember that the root account has UID `0`.
- Prefer the principle of least privilege: use only the permissions required to complete a task.

---

## Practical Exercises

The following exercises are designed to reinforce the concepts and commands introduced in this section. They provide hands-on experience with command lookup, environment variables, documentation tools, input and output redirection, and administrative privileges.

### Exercise 1 — Working with Command Paths

1. Display the current value of the `PATH` environment variable.
2. Display each directory in `PATH` on a separate line.
3. Locate the executable file associated with the `ls` command using `command -v`.
4. Display all available definitions of `ls` using `type -a`.
5. Check whether `cd` is a shell builtin or an external executable.
6. Display the location of the `nano` executable using `whereis`.
7. Create a directory named `MyCommands` inside your home directory.
8. Temporarily add `MyCommands` to the beginning of `PATH`.
9. Verify that the directory has been added successfully.
10. Display the current command lookup cache using `hash -l`.

### Exercise 2 — Environment Variables

1. Display the values of the following environment variables:
   - `HOME`
   - `USER`
   - `SHELL`
   - `PATH`
   - `PWD`
2. Create a shell variable named `PROJECT` with the value `RootBound`.
3. Display its value using `echo`.
4. Export the variable to make it available to child processes.
5. Start a new Bash process and display the value of `PROJECT`.
6. Create another variable named `EDITOR` with the value `nano`.
7. Display all exported environment variables using `printenv`.
8. Temporarily assign a variable named `APP_MODE` the value `development` for a single command using `env`.
9. Remove the `PROJECT` variable using `unset`.
10. Verify that the variable is no longer available in the current shell.

### Exercise 3 — Exploring Command Documentation

1. Open the manual page for `ls`.
2. Navigate through the manual page using the keyboard shortcuts.
3. Display the help information for the `mkdir` command.
4. Display the Bash help documentation for `cd`.
5. Find a short description of the `grep` command using `whatis`.
6. Search manual page descriptions related to networking using `apropos`.
7. Locate the binary and manual page files associated with `bash` using `whereis`.
8. Open the Info documentation for `coreutils`, if available.
9. Identify the manual page section associated with the `passwd` command.
10. Explain the difference between `man`, `help`, and `--help`.

### Exercise 4 — Input and Output Redirection

1. Create a file named `output.txt`.
2. Write a message into the file using `echo` and `>`.
3. Append another message using `>>`.
4. Display the contents of the file using `cat`.
5. Save the output of the `date` command into `date.txt`.
6. Count the number of lines in `output.txt` using input redirection.
7. Redirect the output of `ls` into `directory-list.txt`.
8. Execute a command that produces an error and redirect the error message into `error.log`.
9. Append another error message to `error.log` without overwriting its existing contents.
10. Redirect both standard output and standard error into a single file.
11. Discard command output using `/dev/null`.
12. Use a here document to pass multiple lines of text to `cat`.
13. Use a here string to provide input to `wc -w`.

### Exercise 5 — Pipes and the tee Command

1. List the contents of a directory and sort the output using a pipe.
2. Display only lines containing the word `Linux` from a text file using `grep`.
3. Count the number of lines in a file using a pipeline.
4. Find all lines containing `ERROR` in a log file and save the results into `errors.txt`.
5. Display the output of `df -h` and save it into `disk-usage.txt` using `tee`.
6. Append additional output to the same file using `tee -a`.
7. Connect three commands using multiple pipes.
8. Explain the difference between output redirection and a pipe.

### Exercise 6 — Working with Superuser Privileges

1. Display your current username using `whoami`.
2. Display your UID and group information using `id`.
3. Check whether your account belongs to an administrative group.
4. Display your sudo permissions using `sudo -l`.
5. Update the package information using the appropriate command for your Linux distribution.
6. Display the current username while executing a command through `sudo`.
7. Start a root login shell using `sudo -i`, if permitted.
8. Verify that the current user is `root`.
9. Return to your regular user account using `exit`.
10. Explain the difference between `sudo` and `su`.

### Exercise 7 — Command-Line Challenge

Complete the following tasks using the concepts introduced in Day 03:

1. Create a directory named `Day-03-Practice` inside your home directory.
2. Navigate into the directory.
3. Create a file named `system-info.txt`.
4. Save the output of `uname -a` into the file.
5. Append the current date and time to the same file.
6. Create an environment variable named `PROJECT_NAME` with the value `RootBound`.
7. Export the variable and verify that a child Bash process can access it.
8. Display the value of `PATH`.
9. Search for the `grep` command using the appropriate documentation tool.
10. Create a file named `errors.log` and redirect an error message into it.
11. Use `grep` to search for a specific word in `system-info.txt`.
12. Display the results and save them into `search-results.txt` using `tee`.
13. Check your current user and group information.
14. Inspect your sudo permissions, if available.
15. Display the final contents of all created files.

### Exercise 8 — Knowledge Check

Answer the following questions without referring to the previous sections:

1. What is the purpose of the `PATH` environment variable?
2. What is the difference between an absolute path and a relative path?
3. What is the difference between a shell variable and an environment variable?
4. What does the `export` command do?
5. What is the difference between `printenv` and `set`?
6. What is the purpose of the `unset` command?
7. What is the difference between `man` and `info`?
8. When should you use `apropos` instead of `whatis`?
9. What are the three standard Linux data streams?
10. What is the difference between `>` and `>>`?
11. What does `2>` redirect?
12. What is the purpose of `/dev/null`?
13. What is the difference between a pipe and output redirection?
14. What is the purpose of the `tee` command?
15. What is the difference between `sudo` and `su`?
16. What is the UID of the root user?
17. Why should administrative privileges be used carefully?

**Goal:** Develop practical skills in managing command paths, configuring environment variables, accessing command documentation, controlling input and output streams, and performing administrative operations safely.


<p align="right">
  <a href="../Day-02/README.md">← Day 02</a>
  &nbsp; | &nbsp;
  <a href="../Day-04/">Day 04 →</a>
  &nbsp; | &nbsp;
  <a href="#top">Back to top ↑</a>
</p>
