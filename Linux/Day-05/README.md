
<div id="top"></div>

<p align="center">
  <a href="https://github.com/mammaddrik/RootBound">
    <img src="https://i.postimg.cc/mg5Qq4wL/Root-Bound-Day-05.png" alt="RootBound-Day-05">
  </a>
</p>

# Day 05 — Text Processing

## Table of Contents

- [Day 05 — Text Processing](#day-05--text-processing)
  - [Table of Contents](#table-of-contents)
  - [Standard Input, Output, and Error](#standard-input-output-and-error)
    - [Understanding Standard Streams](#understanding-standard-streams)
    - [Redirecting Input and Output](#redirecting-input-and-output)
  - [Text Processing Commands](#text-processing-commands)
    - [Working with cut](#working-with-cut)
    - [Working with paste](#working-with-paste)
    - [Working with sort](#working-with-sort)
    - [Working with tr](#working-with-tr)
    - [Working with head and tail](#working-with-head-and-tail)
    - [Working with join](#working-with-join)
    - [Working with split](#working-with-split)
    - [Working with uniq](#working-with-uniq)
    - [Working with wc](#working-with-wc)
    - [Working with nl](#working-with-nl)
    - [Working with expand and unexpand](#working-with-expand-and-unexpand)
  - [Pipes and Command Chaining](#pipes-and-command-chaining)
    - [Understanding Pipes](#understanding-pipes)
    - [Working with tee](#working-with-tee)
  - [Searching and Processing Text](#searching-and-processing-text)
    - [Working with grep](#working-with-grep)
    - [Working with awk](#working-with-awk)
  - [Practical Exercises](#practical-exercises)

---

## Standard Input, Output, and Error

In Linux, commands interact with the operating system through three standard data streams: standard input, standard output, and standard error.

These streams provide a consistent way for programs to receive input, display results, and report errors.

Understanding how these streams work is essential for using command-line tools effectively, redirecting command output, processing text, and building shell scripts.

### Understanding Standard Streams

Linux processes use three standard streams by default:

1. **Standard Input (stdin):** Receives input data.
2. **Standard Output (stdout):** Displays normal command output.
3. **Standard Error (stderr):** Displays error messages and diagnostic information.

Each standard stream is associated with a file descriptor (FD), which is a non-negative integer used by a process to identify an open file or I/O resource.

The three standard file descriptors are:

| Stream | File Descriptor | Description | Default Destination |
|---|---:|---|---|
| stdin | `0` | Standard input | Keyboard / Terminal |
| stdout | `1` | Standard output | Terminal |
| stderr | `2` | Standard error | Terminal |

**Standard Input (stdin)**

Standard input is the data source from which a command reads information.

By default, stdin receives data from the keyboard through the terminal.

For example:

```bash
cat
```

When you run `cat` without specifying a filename, it reads from standard input and displays the received content through standard output.

Type some text:

```text
Hello Linux
Learning standard input
```

Press `Ctrl + D` to signal the end of input (EOF) and finish the command.

You can also provide input through another command:

```bash
echo "Hello Linux" | cat
```

In this example, `echo` sends its output to `cat` through standard input.

**Standard Output (stdout)**

Standard output is the stream used by commands to display their normal results.

By default, stdout is connected to the terminal.

For example:

```bash
echo "Welcome to Linux"
```

Output:

```text
Welcome to Linux
```

Other commands that commonly produce standard output include:

```bash
pwd
ls
whoami
date
uname -a
```

Their results are normally displayed in the terminal.

**Standard Error (stderr)**

Standard error is used to report errors, warnings, and diagnostic messages.

By default, stderr is also connected to the terminal, but it is separate from standard output.

For example:

```bash
ls existing-file.txt missing-file.txt
```

If one of the files does not exist, `ls` displays an error message.

Example output:

```text
ls: cannot access 'missing-file.txt': No such file or directory
```

The important distinction is that error messages are written to stderr rather than stdout.

This separation allows you to redirect normal output and error messages independently.

**Understanding File Descriptors**

A file descriptor is an integer that identifies an open file, terminal, pipe, or another I/O resource used by a process.

The three standard file descriptors are created automatically when a process starts.

You can inspect the standard file descriptors of the current Bash shell using:

```bash
ls -l /proc/$$/fd
```

The `$$` variable represents the process ID of the current shell.

Example output may look like:

```text
0 -> /dev/pts/0
1 -> /dev/pts/0
2 -> /dev/pts/0
```

All three descriptors point to the same terminal in this example, but they remain separate streams.

You can also inspect the file descriptors of a running process through its `/proc` directory:

```bash
ls -l /proc/PID/fd
```

Replace `PID` with the process ID.

**Separating Standard Output and Standard Error**

Consider the following command:

```bash
ls /etc /nonexistent-directory
```

This command produces two types of output:

- Normal output containing the contents of `/etc`.
- An error message because `/nonexistent-directory` does not exist.

Both appear in the terminal by default, but they are sent through different streams.

You can redirect them independently:

```bash
ls /etc /nonexistent-directory > output.txt 2> errors.txt
```

Now:

- `output.txt` contains the normal output.
- `errors.txt` contains the error message.

This separation is particularly useful when running scripts, troubleshooting commands, and collecting logs.

**Important Concepts**

- `stdin` is used to receive input.
- `stdout` is used to display normal results.
- `stderr` is used to display errors and diagnostic messages.
- File descriptors `0`, `1`, and `2` identify these streams.
- Standard streams can be redirected to files, devices, or other processes.
- Although stdout and stderr usually appear in the same terminal, they are independent streams.

### Redirecting Input and Output

**I/O Redirection** allows you to change where a command receives its input and where it sends its output.

By default, commands usually read from the keyboard and write their output to the terminal.

Using redirection operators, you can send output to files, read input from files, save error messages, or combine multiple streams.

The shell processes redirections before executing the command.

**Redirecting Standard Output (`>`)**

The `>` operator redirects standard output to a file.

Syntax:

```bash
command > filename
```

Example:

```bash
echo "Hello Linux" > output.txt
```

Instead of displaying the text in the terminal, the command writes it into `output.txt`.

To view the file:

```bash
cat output.txt
```

Output:

```text
Hello Linux
```

**Important:** If the destination file already exists, `>` overwrites its previous content.

For example:

```bash
echo "First line" > output.txt
echo "Second line" > output.txt
```

The file now contains only:

```text
Second line
```

If the file does not exist, the shell creates it automatically, provided the directory is writable.

**Appending Standard Output (`>>`)**

The `>>` operator appends output to the end of a file without removing its existing content.

Syntax:

```bash
command >> filename
```

Example:

```bash
echo "First line" > output.txt
echo "Second line" >> output.txt
echo "Third line" >> output.txt
```

Now the file contains:

```text
First line
Second line
Third line
```

This operator is useful for collecting command output, maintaining logs, and adding information to existing files.

**Redirecting Standard Input (`<`)**

The `<` operator redirects standard input from a file instead of reading from the keyboard.

Syntax:

```bash
command < filename
```

Example:

```bash
cat < output.txt
```

The contents of `output.txt` are passed to `cat` through standard input.

Another example:

```bash
wc -l < output.txt
```

This command counts the number of lines received through standard input.

Unlike `wc -l output.txt`, which displays the filename along with the count, using redirected input normally displays only the count.

**Redirecting Standard Error (`2>`)**

The `2>` operator redirects standard error to a file.

Syntax:

```bash
command 2> error.txt
```

Example:

```bash
ls /nonexistent-directory 2> error.txt
```

The error message is written to `error.txt` instead of appearing in the terminal.

Display the saved error:

```bash
cat error.txt
```

If `error.txt` already exists, its previous content is overwritten.

**Appending Standard Error (`2>>`)**

The `2>>` operator appends error messages to an existing file.

Example:

```bash
ls /missing-directory 2>> errors.log
```

Repeated executions add new error messages to the end of `errors.log` without deleting previous entries.

This is useful for maintaining error logs.

**Redirecting Standard Output and Standard Error Together**

Sometimes you want to save both normal output and error messages in the same file.

There are several ways to accomplish this.

Using `> file 2>&1`:

```bash
command > output.log 2>&1
```

This command performs two operations:

1. `> output.log` redirects standard output (FD 1) to `output.log`.
2. `2>&1` redirects standard error (FD 2) to the current destination of standard output (FD 1).

Both streams are therefore written to `output.log`.

Example:

```bash
ls /etc /missing-directory > output.log 2>&1
```

The file contains both the directory listing and the error message.

**Understanding Redirection Order**

The order of redirection operators matters.

Compare these two commands:

```bash
command > output.log 2>&1
```

```bash
command 2>&1 > output.log
```

In the first command:

- Standard output is redirected to `output.log`.
- Standard error is redirected to the current destination of standard output.
- Both streams go to the file.

In the second command:

- Standard error is first redirected to the original destination of standard output, usually the terminal.
- Standard output is then redirected to `output.log`.
- Only standard output goes to the file; standard error remains connected to the terminal.

The shell processes redirections from left to right.

This is an important detail when combining multiple redirections.

**Using `&>` to Redirect Both Streams**

Bash provides the `&>` operator as a shorter way to redirect both standard output and standard error to the same file.

Example:

```bash
command &> output.log
```

This is equivalent to:

```bash
command > output.log 2>&1
```

To append both streams instead of overwriting the file, use:

```bash
command &>> output.log
```

These combined operators are Bash-specific. The `> file 2>&1` form is more portable across common POSIX-style shells.

**Redirecting Output to `/dev/null`**

Linux provides a special device called `/dev/null`.

Anything written to `/dev/null` is discarded.

It is commonly called the null device or a data sink.

You can use it to suppress unwanted command output.

Discard standard output:

```bash
command > /dev/null
```

Discard standard error:

```bash
command 2> /dev/null
```

Discard both standard output and standard error:

```bash
command > /dev/null 2>&1
```

Example:

```bash
ls /etc > /dev/null
```

The directory listing is discarded, so nothing is displayed.

Another example:

```bash
ls /missing-directory 2> /dev/null
```

The error message is discarded.

**Important:** Redirecting output to `/dev/null` does not prevent a command from executing. It only discards the selected output stream.

Also, suppressing errors can make troubleshooting more difficult, so use it carefully.

**Using Here Documents (`<<`)**

A here document allows you to provide multiple lines of input directly to a command.

Syntax:

```bash
command << DELIMITER
line 1
line 2
line 3
DELIMITER
```

The shell passes all lines between the two delimiters to the command through standard input.

Example:

```bash
cat << EOF
Welcome to Linux
Learning shell redirection
Working with standard input
EOF
```

Output:

```text
Welcome to Linux
Learning shell redirection
Working with standard input
```

The terminating delimiter must appear on a line by itself.

You can choose a different delimiter:

```bash
cat << END
First line
Second line
END
```

If the delimiter is quoted, parameter and command substitutions are not performed inside the here document.

For example:

```bash
name="Linux"

cat << 'EOF'
Hello $name
EOF
```

Output:

```text
Hello $name
```

Without quotes around the delimiter, the variable is expanded:

```bash
name="Linux"

cat << EOF
Hello $name
EOF
```

Output:

```text
Hello Linux
```

Here documents are especially useful in shell scripts for supplying configuration data, SQL statements, and multi-line text to commands.

**Using Here Strings (`<<<`)**

A here string provides a single string as standard input to a command.

Syntax:

```bash
command <<< "text"
```

Example:

```bash
wc -w <<< "Learning Linux is interesting"
```

Output:

```text
4
```

Another example:

```bash
cat <<< "Hello from standard input"
```

Output:

```text
Hello from standard input
```

Unlike a here document, a here string is intended for a single string rather than a multi-line block.

Here strings are supported by Bash and several other shells, but are not part of the POSIX shell standard.

**Preventing Accidental File Overwriting**

In Bash, the `noclobber` option can prevent `>` from overwriting an existing regular file.

Enable it:

```bash
set -o noclobber
```

Now, attempting to overwrite an existing file with `>` produces an error:

```bash
echo "New content" > output.txt
```

If `output.txt` already exists, the operation is prevented.

You can still use `>|` to explicitly force an overwrite:

```bash
echo "New content" >| output.txt
```

Disable the option:

```bash
set +o noclobber
```

This option is useful when working with important files that should not be overwritten accidentally.

**Common Redirection Operators**

| Operator | Description |
|---|---|
| `>` | Redirect stdout and overwrite a file |
| `>>` | Redirect stdout and append to a file |
| `<` | Read stdin from a file |
| `2>` | Redirect stderr and overwrite a file |
| `2>>` | Redirect stderr and append to a file |
| `2>&1` | Redirect stderr to the current destination of stdout |
| `&>` | Redirect stdout and stderr to a file (Bash) |
| `&>>` | Append stdout and stderr to a file (Bash) |
| `<<` | Provide multi-line input using a here document |
| `<<<` | Provide a string through a here string |
| `>|` | Force output redirection even when `noclobber` is enabled |

**Practical Examples**

The following examples combine different redirection operators.

1. Save the output of a command:

   ```bash
   date > date.txt
   ```

2. Append command output to an existing file:

   ```bash
   whoami >> date.txt
   ```

3. Read data from a file:

   ```bash
   sort < names.txt
   ```

4. Save only error messages:

   ```bash
   find /root -name "*.conf" 2> errors.log
   ```

5. Save normal output and errors separately:

   ```bash
   find /etc -name "*.conf" > results.txt 2> errors.log
   ```

6. Save normal output and errors together:

   ```bash
   find /etc -name "*.conf" > combined.log 2>&1
   ```

7. Discard normal output while keeping errors visible:

   ```bash
   command > /dev/null
   ```

8. Discard both normal output and errors:

   ```bash
   command > /dev/null 2>&1
   ```

9. Provide multiple lines of input to a command:

   ```bash
   cat << EOF
   Linux
   Shell
   Redirection
   EOF
   ```

**Important Concepts**

- Redirection changes the default input or output destination of a command.
- `>` overwrites a file, while `>>` appends to it.
- `<` allows a command to read input from a file.
- `2>` and `2>>` handle standard error independently.
- `2>&1` redirects standard error to the current destination of standard output.
- Redirection order matters because the shell processes operators from left to right.
- `/dev/null` discards data sent to it.
- Here documents and here strings provide input directly from the shell.
- Bash's `noclobber` option can help prevent accidental overwrites.

**Summary**

Standard streams provide the foundation for communication between Linux processes and their environment.

By understanding stdin, stdout, and stderr, you can control how commands receive input, display results, and report errors.

Redirection operators make it possible to save command output, collect error messages, read data from files, and suppress unwanted output.

These concepts are also essential for understanding pipes, which allow the output of one command to become the input of another.

---

## Text Processing Commands

### Working with cut

### Working with paste

### Working with sort

### Working with tr

### Working with head and tail

### Working with join

### Working with split

### Working with uniq

### Working with wc

### Working with nl

### Working with expand and unexpand

---

## Pipes and Command Chaining

### Understanding Pipes

### Working with tee

---

## Searching and Processing Text

### Working with grep

### Working with awk

---

## Practical Exercises

---

<p align="right">
  <a href="../Day-04/README.md">← Day 04</a>
  &nbsp; | &nbsp;
  <a href="../Day-06/">Day 06 →</a>
  &nbsp; | &nbsp;
  <a href="#top">Back to top ↑</a>
</p>
