<p align="center">
  <img src="../../media/images/Linux/Day-05/image_01.png" alt="image_01">
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
      - [Introduction to grep](#introduction-to-grep)
      - [Basic Syntax](#basic-syntax)
      - [Searching for Text Patterns](#searching-for-text-patterns)
      - [Case-Insensitive Searches](#case-insensitive-searches)
      - [Searching Multiple Files](#searching-multiple-files)
      - [Recursive Searching](#recursive-searching)
      - [Displaying Matching Line Numbers](#displaying-matching-line-numbers)
      - [Matching Whole Words and Lines](#matching-whole-words-and-lines)
      - [Inverting Search Results](#inverting-search-results)
      - [Counting Matches](#counting-matches)
      - [Displaying Context Around Matches](#displaying-context-around-matches)
      - [Using Regular Expressions](#using-regular-expressions)
      - [Extended Regular Expressions](#extended-regular-expressions)
      - [Searching with Character Classes and Quantifiers](#searching-with-character-classes-and-quantifiers)
      - [Combining grep with Pipes](#combining-grep-with-pipes)
      - [Useful grep Options](#useful-grep-options)
      - [Practical Examples](#practical-examples)
      - [Understanding grep Exit Status](#understanding-grep-exit-status)
    - [Working with awk](#working-with-awk)
      - [Introduction to awk](#introduction-to-awk)
      - [Basic Syntax](#basic-syntax-1)
      - [Understanding Records and Fields](#understanding-records-and-fields)
      - [Built-in Variables](#built-in-variables)
      - [Printing Fields and Records](#printing-fields-and-records)
      - [Using Field Separators](#using-field-separators)
      - [Filtering Records with Patterns](#filtering-records-with-patterns)
      - [Using Comparison and Logical Operators](#using-comparison-and-logical-operators)
      - [Working with Regular Expressions](#working-with-regular-expressions)
      - [Using BEGIN and END Blocks](#using-begin-and-end-blocks)
      - [Performing Calculations](#performing-calculations)
      - [Built-in Functions](#built-in-functions)
      - [Using Conditional Statements](#using-conditional-statements)
      - [Working with Loops](#working-with-loops)
      - [Modifying Fields and Records](#modifying-fields-and-records)
      - [Formatting Output with printf](#formatting-output-with-printf)
      - [Combining awk with Other Commands](#combining-awk-with-other-commands)
      - [Practical Examples](#practical-examples-1)
      - [Common awk Use Cases](#common-awk-use-cases)
      - [Understanding the Difference Between grep and awk](#understanding-the-difference-between-grep-and-awk)
    - [Summary](#summary)
    - [Additional Resources](#additional-resources)
  - [Practical Exercises](#practical-exercises)
    - [Exercise 1 — Understanding Standard Streams](#exercise-1--understanding-standard-streams)
    - [Exercise 2 — Working with Text Processing Commands](#exercise-2--working-with-text-processing-commands)
    - [Exercise 3 — Working with Pipes and tee](#exercise-3--working-with-pipes-and-tee)
    - [Exercise 4 — Searching Text with grep](#exercise-4--searching-text-with-grep)
    - [Exercise 5 — Processing Text with awk](#exercise-5--processing-text-with-awk)
    - [Exercise 6 — Working with System Files](#exercise-6--working-with-system-files)
    - [Exercise 7 — Advanced Text Processing Pipeline](#exercise-7--advanced-text-processing-pipeline)
    - [Exercise 8 — Final Challenge: Linux Log Analyzer](#exercise-8--final-challenge-linux-log-analyzer)

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

Linux provides a collection of command-line utilities for extracting, transforming, sorting, comparing, and analyzing text.

These utilities are particularly useful when working with configuration files, log files, CSV data, command output, and large text documents.

Most text-processing commands read data from standard input or one or more files and write their results to standard output. This makes them especially useful when combined with input/output redirection and pipes.

The following commands are commonly used for text processing:

- `cut` — Extract specific fields or character positions from lines.
- `paste` — Merge lines from multiple files.
- `sort` — Sort lines of text.
- `tr` — Translate, delete, or squeeze characters.
- `head` and `tail` — Display the beginning or end of files.
- `join` — Combine lines from two files using a common field.
- `split` — Divide large files into smaller files.
- `uniq` — Identify or remove adjacent duplicate lines.
- `wc` — Count lines, words, characters, and bytes.
- `nl` — Number lines in a file.
- `expand` and `unexpand` — Convert between tabs and spaces.

### Working with cut

The `cut` command extracts specific sections from each line of a file or standard input.

It is commonly used to extract columns, fields, or individual character positions from structured text.

**Syntax**

```bash
cut OPTION... [FILE...]
```

The `cut` command supports three main selection methods:

- Bytes (`-b`)
- Characters (`-c`)
- Fields (`-f`)

**Extracting Characters by Position**

The `-c` option extracts characters based on their positions in each line.

Example:

```bash
echo "Linux System" | cut -c 1-5
```

Output:

```text
Linux
```

Extract characters from position 7 to the end:

```bash
echo "Linux System" | cut -c 7-
```

Output:

```text
System
```

Extract specific character positions:

```bash
echo "Linux System" | cut -c 1,3,5
```

Output:

```text
Lnx
```

You can use the following position formats:

| Format | Description |
|---|---|
| `-c 1-5` | Characters from position 1 to 5 |
| `-c 3-` | Characters from position 3 to the end |
| `-c -5` | First 5 characters |
| `-c 1,3,5` | Characters at positions 1, 3, and 5 |

**Note:** Character positions and byte positions are not always equivalent. In UTF-8 text, a single character may occupy multiple bytes. Use `-c` when selecting character positions rather than assuming that `-b` behaves the same way.

**Extracting Bytes**

The `-b` option extracts specific byte positions from each line.

Example:

```bash
echo "Linux System" | cut -b 1-5
```

Output:

```text
Linux
```

Extract the first three bytes:

```bash
echo "Linux System" | cut -b 1-3
```

Output:

```text
Lin
```

This option is particularly useful when working with ASCII text or data formats where byte positions are important.

**Extracting Fields**

The `-f` option extracts fields separated by a delimiter.

By default, `cut` uses the TAB character as its field delimiter.

Consider a file named `users.txt`:

```text
alice:developer:25
bob:administrator:30
charlie:analyst:28
```

Extract the first field:

```bash
cut -d ':' -f 1 users.txt
```

Output:

```text
alice
bob
charlie
```

Extract the second field:

```bash
cut -d ':' -f 2 users.txt
```

Output:

```text
developer
administrator
analyst
```

Extract multiple fields:

```bash
cut -d ':' -f 1,3 users.txt
```

Output:

```text
alice:25
bob:30
charlie:28
```

Extract a range of fields:

```bash
cut -d ':' -f 1-2 users.txt
```

Output:

```text
alice:developer
bob:administrator
charlie:analyst
```

**Changing the Output Delimiter**

The `--output-delimiter` option changes the separator between extracted fields.

Example:

```bash
cut -d ':' -f 1,3 --output-delimiter=' | ' users.txt
```

Output:

```text
alice | 25
bob | 30
charlie | 28
```

**Selecting Lines Without the Delimiter**

By default, when using `-f`, lines that do not contain the specified delimiter are printed unchanged.

The `-s` option suppresses these lines.

Example:

```bash
echo -e "alice:admin\nunknown\nbob:user" | cut -d ':' -f 1 -s
```

Output:

```text
alice
bob
```

**Complementing a Selection**

The `--complement` option selects everything except the specified positions or fields.

Example:

```bash
echo "alice:developer:25" | cut -d ':' -f 2 --complement
```

Output:

```text
alice:25
```

This removes the second field from the output.

**Practical Examples**

Extract usernames from a colon-separated file:

```bash
cut -d ':' -f 1 /etc/passwd
```

Extract usernames and login shells:

```bash
cut -d ':' -f 1,7 /etc/passwd
```

Extract the first column of a CSV file:

```bash
cut -d ',' -f 1 data.csv
```

**Important Notes**

- `-d` specifies the field delimiter.
- `-f` selects fields.
- `-c` selects character positions.
- `-b` selects byte positions.
- `-s` suppresses lines without the delimiter when using field selection.
- `--complement` selects everything except the specified fields or positions.
- `cut` is designed for simple delimiter-separated data, not complex CSV files containing quoted fields or delimiters inside quoted values.

### Working with paste

The `paste` command combines corresponding lines from multiple files.

It is useful when you want to merge columns of data or combine separate text files side by side.

Unlike `cat`, which joins file contents sequentially, `paste` combines corresponding lines horizontally by default.

**Syntax**

```bash
paste [OPTION...] [FILE...]
```

**Combining Files Side by Side**

Create two files:

`names.txt`:

```text
Alice
Bob
Charlie
```

`ages.txt`:

```text
25
30
28
```

Combine them:

```bash
paste names.txt ages.txt
```

Output:

```text
Alice   25
Bob     30
Charlie 28
```

By default, `paste` uses a TAB character to separate the columns.

**Changing the Delimiter**

The `-d` option allows you to specify a different delimiter.

Example:

```bash
paste -d ':' names.txt ages.txt
```

Output:

```text
Alice:25
Bob:30
Charlie:28
```

You can also use multiple delimiters:

```bash
paste -d ':,' names.txt ages.txt cities.txt
```

The delimiters are applied cyclically between input files.

**Combining Multiple Files**

Create another file named `cities.txt`:

```text
London
Paris
Berlin
```

Combine all three files:

```bash
paste names.txt ages.txt cities.txt
```

Output:

```text
Alice   25      London
Bob     30      Paris
Charlie 28      Berlin
```

**Serial Combination**

The `-s` option combines lines from each input file serially rather than combining corresponding lines horizontally.

Example:

```bash
paste -s names.txt
```

Output:

```text
Alice   Bob     Charlie
```

Use a comma as the delimiter:

```bash
paste -s -d ',' names.txt
```

Output:

```text
Alice,Bob,Charlie
```

When multiple files are provided with `-s`, each file is processed separately.

**Using Standard Input**

The `paste` command can also read data from standard input.

The `-` symbol represents standard input.

Example:

```bash
echo -e "Alice\nBob" | paste - -
```

Output:

```text
Alice   Bob
```

Here, the same standard input stream is used for both input positions, allowing `paste` to combine consecutive lines into columns.

**Handling Files with Different Numbers of Lines**

If input files contain different numbers of lines, `paste` continues until all input files reach the end.

Missing fields are represented by empty values.

For example:

`names.txt`:

```text
Alice
Bob
Charlie
```

`ages.txt`:

```text
25
30
```

Command:

```bash
paste names.txt ages.txt
```

Output:

```text
Alice   25
Bob     30
Charlie
```

**Important Options**

| Option | Description |
|---|---|
| `-d` | Specify one or more delimiters |
| `-s` | Combine lines serially instead of horizontally |
| `-` | Read from standard input |

**Practical Example**

Combine usernames and their login shells from `/etc/passwd`:

```bash
cut -d ':' -f 1 /etc/passwd > usernames.txt
cut -d ':' -f 7 /etc/passwd > shells.txt

paste -d ' | ' usernames.txt shells.txt
```

This produces a two-column result containing usernames and their corresponding shells.

### Working with sort

The `sort` command arranges lines of text in a specific order.

It is commonly used to organize lists, sort numerical values, process structured data, and prepare files for other text-processing commands.

By default, `sort` arranges lines in ascending lexicographical order according to the current locale.

**Syntax**

```bash
sort [OPTION...] [FILE...]
```

**Basic Sorting**

Create a file named `names.txt`:

```text
Charlie
Alice
Bob
David
```

Sort the file:

```bash
sort names.txt
```

Output:

```text
Alice
Bob
Charlie
David
```

The original file remains unchanged.

**Reverse Sorting**

The `-r` option reverses the sorting order.

Example:

```bash
sort -r names.txt
```

Output:

```text
David
Charlie
Bob
Alice
```

**Numeric Sorting**

By default, `sort` compares lines lexicographically rather than numerically.

Consider a file named `numbers.txt`:

```text
10
2
30
1
20
```

Default sorting:

```bash
sort numbers.txt
```

Output:

```text
1
10
2
20
30
```

To sort numbers according to their numerical values, use `-n`:

```bash
sort -n numbers.txt
```

Output:

```text
1
2
10
20
30
```

For reverse numerical sorting:

```bash
sort -nr numbers.txt
```

Output:

```text
30
20
10
2
1
```

**Sorting in Human-Readable Format**

The `-h` option sorts human-readable numerical values containing suffixes such as K, M, and G.

Example:

```text
1K
20M
500K
2G
```

Command:

```bash
sort -h sizes.txt
```

This sorts the values according to their approximate numerical sizes.

**Removing Duplicate Lines**

The `-u` option sorts the input and removes duplicate lines.

Example:

```bash
sort -u names.txt
```

This is useful when you want a sorted list containing only unique lines.

Unlike `uniq`, which normally removes only adjacent duplicates, `sort -u` can eliminate duplicates that were originally separated in the input.

**Sorting by a Specific Field**

The `-k` option specifies a field or key used for sorting.

Consider a file named `users.txt`:

```text
alice 25 developer
bob 30 administrator
charlie 22 analyst
```

Sort by the second field numerically:

```bash
sort -k2,2n users.txt
```

Output:

```text
charlie 22 analyst
alice 25 developer
bob 30 administrator
```

The `-k2,2n` option means:

- `-k2,2` selects the second field as the sorting key.
- `n` enables numerical sorting.

Sort by the third field:

```bash
sort -k3,3 users.txt
```

**Changing the Field Separator**

By default, `sort` uses whitespace to identify fields.

The `-t` option allows you to specify a different field separator.

Consider a colon-separated file:

```text
alice:developer:25
bob:administrator:30
charlie:analyst:22
```

Sort by the third field numerically:

```bash
sort -t ':' -k3,3n users.txt
```

Output:

```text
charlie:analyst:22
alice:developer:25
bob:administrator:30
```

**Sorting by Multiple Fields**

You can provide multiple sorting keys.

Example:

```bash
sort -k2,2n -k1,1 users.txt
```

This sorts records primarily by the second field numerically and then by the first field alphabetically when the primary keys are equal.

**Case-Insensitive Sorting**

The `-f` option ignores case differences during sorting.

Example:

```bash
sort -f names.txt
```

**Checking Whether a File Is Already Sorted**

The `-c` option checks whether a file is sorted.

Example:

```bash
sort -c names.txt
```

If the file is sorted, no output is produced and the command exits successfully.

If it is not sorted, `sort` reports the first disorder it encounters and returns a non-zero exit status.

**Saving Sorted Output**

Use redirection to save the sorted result:

```bash
sort names.txt > sorted-names.txt
```

You can also sort a file and replace its contents using:

```bash
sort -o names.txt names.txt
```

The `-o` option specifies the output file.

**Important Options**

| Option | Description |
|---|---|
| `-r` | Reverse the sorting order |
| `-n` | Sort numerically |
| `-h` | Sort human-readable numbers |
| `-u` | Remove duplicate lines |
| `-k` | Sort by a specific field or key |
| `-t` | Specify the field separator |
| `-f` | Ignore case differences |
| `-c` | Check whether input is sorted |
| `-o` | Write output to a specified file |
| `-M` | Sort by month names |
| `-V` | Perform version sorting |

**Practical Examples**

Sort a list alphabetically:

```bash
sort names.txt
```

Sort numbers in descending order:

```bash
sort -nr numbers.txt
```

Sort a CSV file by its second column:

```bash
sort -t ',' -k2,2 data.csv
```

Sort and remove duplicates:

```bash
sort -u names.txt
```

Sort file sizes:

```bash
du -h /var/log/* | sort -h
```

**Important Notes**

- Sorting order can depend on the current locale.
- Use `LC_ALL=C sort` when you need predictable byte-oriented ordering across different environments.
- `sort` does not normally modify the original file unless you explicitly write the result back to it.
- For reliable field-based sorting, specify both the beginning and ending positions of the key when appropriate.

### Working with tr

The `tr` command translates, deletes, or squeezes characters from standard input.

It is particularly useful for changing letter case, replacing characters, removing unwanted characters, and normalizing text.

Unlike many other text-processing commands, `tr` does not normally accept filenames as positional input arguments. It reads from standard input and writes the result to standard output.

**Syntax**

```bash
tr [OPTION...] SET1 [SET2]
```

The command supports three main operations:

- Translating characters.
- Deleting characters.
- Squeezing repeated characters.

**Translating Characters**

The `tr` command replaces characters from `SET1` with corresponding characters from `SET2`.

Example:

```bash
echo "hello linux" | tr 'a-z' 'A-Z'
```

Output:

```text
HELLO LINUX
```

The lowercase letters are converted to uppercase.

Convert uppercase letters to lowercase:

```bash
echo "HELLO LINUX" | tr 'A-Z' 'a-z'
```

Output:

```text
hello linux
```

Replace spaces with underscores:

```bash
echo "Linux Text Processing" | tr ' ' '_'
```

Output:

```text
Linux_Text_Processing
```

**Translating Specific Characters**

You can specify individual characters instead of character ranges.

Example:

```bash
echo "Linux System" | tr 'iu' '12'
```

Output:

```text
L1n2x System
```

Each occurrence of `i` is replaced with `1`, and each occurrence of `u` is replaced with `2`.

**Deleting Characters**

The `-d` option deletes all characters belonging to the specified set.

Example:

```bash
echo "Linux 123 System 456" | tr -d '0-9'
```

Output:

```text
Linux  System
```

All digits are removed.

Remove spaces:

```bash
echo "Linux Text Processing" | tr -d ' '
```

Output:

```text
LinuxTextProcessing
```

Remove punctuation:

```bash
echo "Hello, Linux!" | tr -d '[:punct:]'
```

Output:

```text
Hello Linux
```

**Using Character Classes**

`tr` supports several predefined character classes.

| Character Class | Description |
|---|---|
| `[:alnum:]` | Letters and digits |
| `[:alpha:]` | Alphabetic characters |
| `[:blank:]` | Space and TAB characters |
| `[:cntrl:]` | Control characters |
| `[:digit:]` | Numeric digits |
| `[:lower:]` | Lowercase letters |
| `[:upper:]` | Uppercase letters |
| `[:space:]` | Whitespace characters |
| `[:punct:]` | Punctuation characters |
| `[:print:]` | Printable characters |
| `[:graph:]` | Printable non-space characters |
| `[:xdigit:]` | Hexadecimal digits |

Example:

Remove all digits:

```bash
echo "User123Linux456" | tr -d '[:digit:]'
```

Output:

```text
UserLinux
```

Keep only alphabetic characters:

```bash
echo "Linux123 System!" | tr -cd '[:alpha:]'
```

Output:

```text
LinuxSystem
```

The `-c` option complements the specified character set, while `-d` deletes the selected characters.

**Squeezing Repeated Characters**

The `-s` option replaces consecutive repeated characters with a single occurrence.

Example:

```bash
echo "Helloooo    Linux" | tr -s 'o '
```

Output:

```text
Helo Linux
```

Repeated `o` characters and consecutive spaces are squeezed.

You can also normalize multiple spaces:

```bash
echo "Linux     Text       Processing" | tr -s ' '
```

Output:

```text
Linux Text Processing
```

**Combining Options**

The `tr` command allows you to combine operations.

For example, convert text to uppercase and remove digits:

```bash
echo "linux123 system456" | tr -d '0-9' | tr 'a-z' 'A-Z'
```

Output:

```text
LINUX SYSTEM
```

Another example: remove all characters except letters and spaces:

```bash
echo "Linux123, Text! Processing." | tr -cd '[:alpha:][:space:]'
```

Output:

```text
Linux Text Processing
```

**Translating Characters into New Lines**

You can replace delimiters with newline characters.

Example:

```bash
echo "apple,banana,orange" | tr ',' '\n'
```

Output:

```text
apple
banana
orange
```

This is useful for converting simple delimiter-separated values into separate lines.

**Using tr with Files**

Because `tr` reads from standard input, use input redirection to process files.

Example:

```bash
tr 'a-z' 'A-Z' < input.txt
```

Save the result:

```bash
tr 'a-z' 'A-Z' < input.txt > output.txt
```

**Important Options**

| Option | Description |
|---|---|
| `-d` | Delete characters from the specified set |
| `-s` | Squeeze repeated characters |
| `-c` | Complement the character set |
| `-t` | Truncate `SET1` to the length of `SET2` |

**Important Notes**

- `tr` processes characters from standard input.
- It does not understand regular expressions like `grep` or `sed`.
- Character classes must use the special `[:class:]` notation inside a character set.
- `-c` complements the character set; it does not mean case conversion.
- Use pipes or redirection when processing files.

### Working with head and tail

The `head` and `tail` commands display specific portions of a file.

- `head` displays the beginning of a file.
- `tail` displays the end of a file.

These commands are useful for inspecting large files, reviewing logs, checking configuration files, and monitoring continuously updated data.

**Working with head**

The `head` command displays the first lines of a file.

Syntax:

```bash
head [OPTION...] [FILE...]
```

By default, it displays the first 10 lines.

Example:

```bash
head file.txt
```

Display the first 5 lines:

```bash
head -n 5 file.txt
```

The same operation can be written as:

```bash
head -5 file.txt
```

Display the first 20 lines:

```bash
head -n 20 file.txt
```

**Displaying a Specific Number of Bytes**

The `-c` option displays a specified number of bytes instead of lines.

Example:

```bash
head -c 15 file.txt
```

This displays the first 15 bytes of the file.

To display all but the last 10 lines:

```bash
head -n -10 file.txt
```

This is a GNU extension and may not be available in every implementation.

**Working with tail**

The `tail` command displays the last lines of a file.

Syntax:

```bash
tail [OPTION...] [FILE...]
```

By default, it displays the last 10 lines.

Example:

```bash
tail file.txt
```

Display the last 5 lines:

```bash
tail -n 5 file.txt
```

Display the last 20 lines:

```bash
tail -n 20 file.txt
```

**Displaying Bytes from the End**

The `-c` option displays a specified number of bytes from the end of a file.

Example:

```bash
tail -c 20 file.txt
```

This displays the final 20 bytes.

**Displaying Everything After a Specific Line**

The `-n +N` syntax starts displaying output from line N.

Example:

```bash
tail -n +5 file.txt
```

This displays the file starting from line 5 through the end.

**Monitoring Files in Real Time**

One of the most useful features of `tail` is the `-f` option.

It continuously monitors a file and displays new data as it is appended.

Example:

```bash
tail -f /var/log/syslog
```

This is useful for monitoring system logs while applications or services are running.

Press `Ctrl + C` to stop monitoring.

On systems using systemd, a different log file may be used, and `journalctl -f` is often more appropriate for monitoring the system journal.

**Following Files by Name**

The `-F` option is available in GNU `tail`.

Example:

```bash
tail -F /var/log/syslog
```

Unlike `-f`, which normally follows the opened file descriptor, `-F` follows the filename and retries if the file is replaced or temporarily unavailable.

This is useful when log rotation replaces an existing log file.

**Displaying Multiple Files**

Both commands can process multiple files.

Example:

```bash
head -n 3 file1.txt file2.txt
```

Display the last five lines of multiple files:

```bash
tail -n 5 file1.txt file2.txt
```

The output includes headers identifying each file.

**Using head and tail Together**

Display lines 5 through 10 of a file:

```bash
head -n 10 file.txt | tail -n 6
```

Explanation:

1. `head -n 10` extracts the first 10 lines.
2. `tail -n 6` displays the last 6 lines of that result.

Another approach is to use `sed`:

```bash
sed -n '5,10p' file.txt
```

**Important Options**

| Command | Option | Description |
|---|---|---|
| `head` | `-n N` | Display the first N lines |
| `head` | `-c N` | Display the first N bytes |
| `head` | `-n -N` | Display all but the last N lines (GNU) |
| `tail` | `-n N` | Display the last N lines |
| `tail` | `-c N` | Display the last N bytes |
| `tail` | `-n +N` | Display from line N onward |
| `tail` | `-f` | Follow a file as it grows |
| `tail` | `-F` | Follow a filename and retry (GNU) |

**Practical Examples**

Display the first 10 lines of a system log:

```bash
head -n 10 /var/log/syslog
```

Display the last 20 lines:

```bash
tail -n 20 /var/log/syslog
```

Monitor a log file continuously:

```bash
tail -f application.log
```

Display lines 11 through 20:

```bash
head -n 20 file.txt | tail -n 10
```

### Working with join

The `join` command combines lines from two files based on a common field.

It is similar to a database join operation because it matches records using a shared key.

Unlike `paste`, which combines lines based on their positions, `join` combines lines based on matching field values.

**Syntax**

```bash
join [OPTION...] FILE1 FILE2
```

By default:

- Both files must be sorted according to the join field.
- The first field is used as the matching key.
- Fields are separated by whitespace.
- Matching records are combined into a single output line.

**Basic Joining**

Create two files:

`users.txt`:

```text
1 Alice
2 Bob
3 Charlie
```

`departments.txt`:

```text
1 Development
2 Administration
3 Security
```

Join the files:

```bash
join users.txt departments.txt
```

Output:

```text
1 Alice Development
2 Bob Administration
3 Charlie Security
```

The first field in each file is used as the matching key.

**Joining by Specific Fields**

The `-1` and `-2` options specify which fields should be used as join keys.

- `-1` selects the join field in the first file.
- `-2` selects the join field in the second file.

Example:

`users.txt`:

```text
Alice 101
Bob 102
Charlie 103
```

`departments.txt`:

```text
101 Development
102 Administration
103 Security
```

Join using the second field of the first file and the first field of the second file:

```bash
join -1 2 -2 1 users.txt departments.txt
```

Output:

```text
101 Alice Development
102 Bob Administration
103 Charlie Security
```

**Changing the Field Separator**

The `-t` option specifies the input field separator.

Example:

`users.csv`:

```text
1,Alice
2,Bob
3,Charlie
```

`departments.csv`:

```text
1,Development
2,Administration
3,Security
```

Join the files using commas:

```bash
join -t ',' users.csv departments.csv
```

Output:

```text
1,Alice,Development
2,Bob,Administration
3,Charlie,Security
```

**Including Unmatched Records**

By default, `join` only displays records with matching keys.

The `-a` option includes unpaired lines from a specified file.

Include unmatched records from the first file:

```bash
join -a 1 users.txt departments.txt
```

Include unmatched records from both files:

```bash
join -a 1 -a 2 users.txt departments.txt
```

This is useful when you need to identify records that do not have corresponding entries in another file.

**Replacing Missing Fields**

The `-e` option specifies a replacement string for empty output fields.

It is commonly used together with `-a` and `-o`.

Example:

```bash
join -a 1 -a 2 -e "N/A" -o auto users.txt departments.txt
```

Unmatched records can be displayed with `N/A` in missing fields.

**Selecting Output Fields**

The `-o` option specifies which fields should appear in the output.

Example:

```bash
join -o 1.2,2.2 users.txt departments.txt
```

This selects:

- Field 2 from the first file.
- Field 2 from the second file.

The join key is not included unless you explicitly request it.

**Understanding Sorted Input**

The `join` command expects both input files to be sorted according to their respective join fields.

For example:

```bash
sort -k1,1 users.txt -o users.txt
sort -k1,1 departments.txt -o departments.txt

join users.txt departments.txt
```

When using a custom delimiter, make sure the sorting operation uses the same field structure.

For example:

```bash
sort -t ',' -k1,1 users.csv -o users.csv
sort -t ',' -k1,1 departments.csv -o departments.csv

join -t ',' users.csv departments.csv
```

**Important Options**

| Option | Description |
|---|---|
| `-1 N` | Use field N from the first file as the join key |
| `-2 N` | Use field N from the second file as the join key |
| `-t CHAR` | Specify the field separator |
| `-a N` | Include unpaired lines from file N |
| `-v N` | Display only unpaired lines from file N |
| `-e STRING` | Replace empty output fields |
| `-o FORMAT` | Specify output fields |
| `-i` | Ignore case when comparing fields |

**Practical Examples**

Find records that exist in the first file but not the second:

```bash
join -v 1 users.txt departments.txt
```

Find records that exist in the second file but not the first:

```bash
join -v 2 users.txt departments.txt
```

Display all matching records and unmatched records:

```bash
join -a 1 -a 2 users.txt departments.txt
```

**Important Notes**

- `join` is designed for structured text with identifiable fields.
- Input files should be sorted by the join key.
- The join keys must use compatible formats and separators.
- For complex CSV files containing quoted fields, use a CSV-aware tool instead of relying on `join`.

### Working with split

The `split` command divides a large file into smaller files.

It is useful when working with large logs, datasets, backups, and files that need to be transferred or processed in smaller parts.

**Syntax**

```bash
split [OPTION...] [INPUT [PREFIX]]
```

If no input file is specified, `split` reads from standard input.

If no prefix is specified, the default prefix is `x`.

**Splitting by Number of Lines**

By default, `split` divides a file into smaller files containing 1000 lines each.

The `-l` option specifies the number of lines per output file.

Example:

```bash
split -l 100 file.txt
```

This creates multiple files, each containing up to 100 lines.

The output files are named:

```text
xaa
xab
xac
...
```

**Specifying an Output Prefix**

You can specify a custom prefix.

Example:

```bash
split -l 100 file.txt part_
```

Output files:

```text
part_aa
part_ab
part_ac
...
```

**Splitting by File Size**

The `-b` option splits a file into smaller files based on size.

Example:

```bash
split -b 1M large-file.txt part_
```

This creates output files of approximately 1 MB each.

Common size suffixes include:

| Suffix | Meaning |
|---|---|
| `K` | Kilobytes (1024 bytes) |
| `M` | Megabytes |
| `G` | Gigabytes |
| `T` | Terabytes |

For example:

```bash
split -b 500K large-file.txt part_
```

**Splitting by Maximum Line Length**

The `-C` option creates output files that do not exceed a specified size while keeping complete lines together.

Example:

```bash
split -C 1M large-file.txt part_
```

Unlike `-b`, which can split data at arbitrary byte boundaries, `-C` attempts to keep lines intact.

A single line larger than the specified size may still be placed in its own output file.

**Using Numeric Suffixes**

By default, `split` uses alphabetic suffixes.

The `-d` option uses numeric suffixes instead.

Example:

```bash
split -d -l 100 file.txt part_
```

Output:

```text
part_00
part_01
part_02
...
```

**Changing the Suffix Length**

The `-a` option specifies the number of characters used for output suffixes.

Example:

```bash
split -a 3 -l 100 file.txt part_
```

Output:

```text
part_aaa
part_aab
part_aac
...
```

You can combine it with numeric suffixes:

```bash
split -d -a 4 -l 100 file.txt part_
```

Output:

```text
part_0000
part_0001
part_0002
...
```

**Creating Output Files with a Custom Suffix**

The `--additional-suffix` option appends an additional suffix to generated filenames.

Example:

```bash
split -l 100 --additional-suffix=.txt file.txt part_
```

Output:

```text
part_aa.txt
part_ab.txt
part_ac.txt
...
```

**Reassembling Split Files**

Files created by `split` can be combined again using `cat`.

Example:

```bash
cat part_* > restored.txt
```

If the original file was split into multiple parts using the same prefix, concatenating them in their original order reconstructs the original content.

For numeric suffixes, lexical sorting generally preserves the correct order when all suffixes have the same width.

**Important Options**

| Option | Description |
|---|---|
| `-l N` | Split into files containing N lines |
| `-b SIZE` | Split according to file size |
| `-C SIZE` | Split while keeping complete lines together |
| `-d` | Use numeric suffixes |
| `-a N` | Set suffix length |
| `--additional-suffix` | Append an extra suffix to output filenames |
| `-n N` | Split input into N chunks |

**Practical Examples**

Split a file into groups of 500 lines:

```bash
split -l 500 access.log logs_
```

Split a file into approximately 10 MB parts:

```bash
split -b 10M backup.tar backup_
```

Reassemble the parts:

```bash
cat backup_* > restored-backup.tar
```

**Important Notes**

- The final output file may contain fewer lines or bytes than the specified limit.
- `-b` may split a line across two output files.
- `-C` attempts to preserve complete lines.
- When reassembling files, use the correct original order.
- The original input file is not modified.

### Working with uniq

The `uniq` command identifies, reports, or removes repeated adjacent lines in a text file.

It is commonly used to remove duplicate entries, count occurrences, and analyze repeated data.

An important characteristic of `uniq` is that it normally compares only adjacent lines.

**Syntax**

```bash
uniq [OPTION...] [INPUT [OUTPUT]]
```

If no input file is specified, `uniq` reads from standard input.

**Removing Duplicate Lines**

Create a file named `names.txt`:

```text
Alice
Bob
Alice
Charlie
Bob
```

Run:

```bash
uniq names.txt
```

Output:

```text
Alice
Bob
Alice
Charlie
Bob
```

Notice that repeated lines remain because they are not adjacent.

Now sort the file first:

```bash
sort names.txt | uniq
```

Output:

```text
Alice
Bob
Charlie
```

Sorting groups identical lines together, allowing `uniq` to remove adjacent duplicates.

**Counting Repeated Lines**

The `-c` option displays the number of occurrences of each adjacent line.

Example:

```bash
sort names.txt | uniq -c
```

Output:

```text
2 Alice
2 Bob
1 Charlie
```

This is useful for counting repeated values in logs and datasets.

**Displaying Only Duplicate Lines**

The `-d` option displays only lines that appear more than once in adjacent groups.

Example:

```bash
sort names.txt | uniq -d
```

Output:

```text
Alice
Bob
```

**Displaying Only Unique Lines**

The `-u` option displays only lines that occur exactly once.

Example:

```bash
sort names.txt | uniq -u
```

Output:

```text
Charlie
```

**Ignoring Case Differences**

The `-i` option performs case-insensitive comparisons.

Example:

```bash
printf "Linux\nlinux\nLINUX\nWindows\n" | sort -f | uniq -i
```

Output:

```text
Linux
Windows
```

**Skipping Fields**

The `-f` option skips a specified number of leading fields before comparing lines.

Example:

```text
user1 alice
user2 alice
user3 bob
```

Command:

```bash
uniq -f 1 users.txt
```

The first field is ignored during comparison, so the first two lines are considered equivalent.

**Skipping Characters**

The `-s` option skips a specified number of leading characters before comparing lines.

Example:

```bash
uniq -s 3 file.txt
```

This compares lines starting after their first three characters.

**Comparing a Limited Number of Characters**

The `-w` option limits comparisons to a specified number of characters.

Example:

```bash
uniq -w 5 file.txt
```

Only the first five characters of each line are considered when determining whether adjacent lines are duplicates.

**Writing Output to Another File**

You can specify an output file:

```bash
sort names.txt | uniq > unique-names.txt
```

Alternatively:

```bash
uniq sorted-names.txt unique-names.txt
```

The input file must already have adjacent duplicate lines for `uniq` to remove them effectively.

**Important Options**

| Option | Description |
|---|---|
| `-c` | Count occurrences of adjacent lines |
| `-d` | Display only duplicate lines |
| `-u` | Display only lines that occur once |
| `-i` | Ignore case differences |
| `-f N` | Skip the first N fields |
| `-s N` | Skip the first N characters |
| `-w N` | Compare only the first N characters |

**Practical Examples**

Count unique IP addresses in a log file:

```bash
cut -d ' ' -f 1 access.log | sort | uniq -c
```

Find the most frequently occurring IP addresses:

```bash
cut -d ' ' -f 1 access.log | sort | uniq -c | sort -nr
```

Display only duplicate lines:

```bash
sort file.txt | uniq -d
```

**Important Notes**

- `uniq` compares adjacent lines only.
- Use `sort` before `uniq` when you want to remove duplicates regardless of their original positions.
- `uniq -u` means lines occurring exactly once, not all lines that are different from their neighbors.
- `sort -u` can be a shorter alternative when you need sorted unique lines.

### Working with wc

The `wc` command counts lines, words, bytes, and characters in files or standard input.

It is useful for analyzing text files, measuring content size, checking the number of records, and obtaining basic statistics.

**Syntax**

```bash
wc [OPTION...] [FILE...]
```

**Basic Usage**

Create a file named `sample.txt`:

```text
Linux is powerful.
Text processing is useful.
Learning commands takes practice.
```

Run:

```bash
wc sample.txt
```

Example output:

```text
3 11 79 sample.txt
```

The output contains:

1. Number of lines.
2. Number of words.
3. Number of bytes.
4. Filename.

**Counting Lines**

The `-l` option counts newline characters.

Example:

```bash
wc -l sample.txt
```

Output:

```text
3 sample.txt
```

This is commonly used to count records or lines in log files.

**Counting Words**

The `-w` option counts words.

Example:

```bash
wc -w sample.txt
```

Output:

```text
11 sample.txt
```

Words are generally defined as sequences of non-whitespace characters separated by whitespace.

**Counting Bytes**

The `-c` option counts bytes.

Example:

```bash
wc -c sample.txt
```

This is useful for measuring the actual byte size of text data.

**Counting Characters**

The `-m` option counts characters instead of bytes.

Example:

```bash
wc -m sample.txt
```

This distinction is important when working with UTF-8 text.

For example, a non-ASCII character may occupy multiple bytes but still count as one character.

**Finding the Maximum Line Length**

The `-L` option displays the length of the longest line.

Example:

```bash
wc -L sample.txt
```

This can help identify unusually long lines in configuration files or logs.

**Counting Multiple Files**

You can pass multiple files to `wc`.

Example:

```bash
wc file1.txt file2.txt file3.txt
```

The output displays statistics for each file and a final total.

**Reading from Standard Input**

The `wc` command can also read data through a pipe.

Example:

```bash
echo "Linux is powerful" | wc -w
```

Output:

```text
3
```

Count files in the current directory:

```bash
ls -1 | wc -l
```

This counts the lines produced by `ls -1`, which normally correspond to directory entries.

**Combining Options**

You can combine multiple options:

```bash
wc -lwm sample.txt
```

This displays the line, word, and character counts.

**Important Options**

| Option | Description |
|---|---|
| `-l` | Count lines |
| `-w` | Count words |
| `-c` | Count bytes |
| `-m` | Count characters |
| `-L` | Display maximum line length |

**Practical Examples**

Count the number of users listed in `/etc/passwd`:

```bash
wc -l /etc/passwd
```

Count the number of words in a document:

```bash
wc -w document.txt
```

Count the total number of bytes:

```bash
wc -c document.txt
```

Count lines containing a specific word:

```bash
grep -i "error" application.log | wc -l
```

**Important Notes**

- `-c` counts bytes, while `-m` counts characters.
- `-l` counts newline characters, so a final line without a terminating newline may not be counted as an additional line.
- `-L` reports the maximum line length, not the number of lines.
- When using pipes, `wc` receives data through standard input.

### Working with nl

The `nl` command numbers lines in a text file.

It is useful for adding line numbers to source code, configuration files, documentation, and command output.

Unlike simply adding numbers to every line, `nl` provides options for controlling which lines receive numbers and how those numbers are formatted.

**Syntax**

```bash
nl [OPTION...] [FILE...]
```

**Basic Line Numbering**

Create a file named `sample.txt`:

```text
Linux
Shell
Text Processing
Commands
```

Run:

```bash
nl sample.txt
```

Output:

```text
     1  Linux
     2  Shell
     3  Text Processing
     4  Commands
```

By default, non-empty lines are numbered.

**Numbering All Lines**

The `-b a` option numbers all lines, including empty lines.

Example:

```bash
nl -b a sample.txt
```

**Numbering Non-Empty Lines**

The `-b t` option numbers only non-empty lines.

Example:

```bash
nl -b t sample.txt
```

This is the default behavior.

**Customizing the Starting Number**

The `-v` option specifies the initial line number.

Example:

```bash
nl -v 10 sample.txt
```

Output:

```text
    10  Linux
    11  Shell
    12  Text Processing
    13  Commands
```

**Changing the Numbering Increment**

The `-i` option specifies how much the line number increases each time.

Example:

```bash
nl -i 5 sample.txt
```

Output:

```text
     1  Linux
     6  Shell
    11  Text Processing
    16  Commands
```

**Changing the Number Width**

The `-w` option specifies the width reserved for line numbers.

Example:

```bash
nl -w 3 sample.txt
```

This reserves three character positions for each line number.

**Changing the Number Separator**

The `-s` option specifies the string placed between the line number and the line content.

Example:

```bash
nl -s '. ' sample.txt
```

Output:

```text
     1. Linux
     2. Shell
     3. Text Processing
     4. Commands
```

**Changing Number Alignment**

The `-n` option controls how line numbers are aligned.

Available values include:

| Option | Description |
|---|---|
| `-n ln` | Left-align line numbers |
| `-n rn` | Right-align line numbers |
| `-n rz` | Right-align line numbers with leading zeros |

Example:

```bash
nl -n rz -w 3 sample.txt
```

Output:

```text
001  Linux
002  Shell
003  Text Processing
004  Commands
```

**Using Custom Numbering Rules**

The `-b` option controls which lines receive numbers.

Common numbering styles include:

| Option | Description |
|---|---|
| `-b a` | Number all lines |
| `-b t` | Number non-empty lines |
| `-b pREGEXP` | Number only lines matching a regular expression |

Example:

```bash
nl -b pLinux sample.txt
```

This numbers only lines containing the pattern `Linux`.

**Important Options**

| Option | Description |
|---|---|
| `-b a` | Number all lines |
| `-b t` | Number non-empty lines |
| `-b pREGEXP` | Number lines matching a regular expression |
| `-v N` | Set the starting number |
| `-i N` | Set the numbering increment |
| `-w N` | Set the number field width |
| `-s STRING` | Set the separator |
| `-n FORMAT` | Set number alignment and formatting |

**Practical Examples**

Number all lines in a file:

```bash
nl -b a configuration.conf
```

Start numbering from 100:

```bash
nl -v 100 file.txt
```

Number lines with leading zeros:

```bash
nl -n rz -w 4 file.txt
```

Number only non-empty lines:

```bash
nl -b t document.txt
```

**Important Notes**

- `nl` does not modify the original file.
- By default, empty lines are not numbered.
- Use `-b a` when every line needs a number.
- The numbering format can be customized without changing the original text.

### Working with expand and unexpand

The `expand` and `unexpand` commands convert between TAB characters and spaces.

They are useful when formatting text files, normalizing indentation, and preparing files for environments where consistent spacing is important.

**Working with expand**

The `expand` command converts TAB characters into spaces.

Syntax:

```bash
expand [OPTION...] [FILE...]
```

Create a file containing TAB characters:

```bash
printf 'Linux\tShell\nText\tProcessing\n' > tabs.txt
```

Convert tabs to spaces:

```bash
expand tabs.txt
```

The output contains spaces instead of TAB characters.

**Understanding Tab Stops**

By default, tab stops occur every eight character positions.

The `-t` option allows you to specify a different tab width.

Example:

```bash
expand -t 4 tabs.txt
```

This uses tab stops every four character positions.

You can also specify multiple tab-stop positions:

```bash
expand -t 4,8,12 tabs.txt
```

This defines explicit tab stops at columns 4, 8, and 12. After the last specified position, the final interval is repeated.

**Converting Only Initial Tabs**

The `-i` option converts only TAB characters at the beginning of lines.

Example:

```bash
expand -i tabs.txt
```

This is useful when you want to normalize indentation without changing tabs inside the rest of the text.

**Working with unexpand**

The `unexpand` command performs the opposite operation.

It converts sequences of spaces into TAB characters where possible.

Syntax:

```bash
unexpand [OPTION...] [FILE...]
```

Example:

```bash
unexpand spaces.txt
```

This replaces eligible groups of spaces with TAB characters.

**Using Custom Tab Stops**

The `-t` option specifies tab-stop positions.

Example:

```bash
unexpand -t 4 spaces.txt
```

This uses tab stops every four character positions.

**Converting Only Initial Spaces**

By default, `unexpand` converts only sequences of spaces at the beginning of lines.

This behavior is useful for converting indentation while leaving spaces inside sentences unchanged.

The `-a` option allows it to convert spaces throughout lines, not just at their beginnings.

Example:

```bash
unexpand -a spaces.txt
```

**Using Both Commands Together**

You can convert tabs to spaces and then convert eligible spaces back to tabs.

Example:

```bash
expand -t 4 tabs.txt > spaces.txt
unexpand -t 4 spaces.txt > restored.txt
```

The resulting file may use TAB characters again for eligible sequences.

**Important Options**

| Command | Option | Description |
|---|---|---|
| `expand` | `-t N` | Set tab stops every N columns |
| `expand` | `-t LIST` | Specify explicit tab-stop positions |
| `expand` | `-i` | Convert only initial tabs |
| `unexpand` | `-t N` | Set tab stops every N columns |
| `unexpand` | `-a` | Convert spaces throughout lines |
| `unexpand` | `--first-only` | Convert only initial spaces |

**Practical Examples**

Convert all tabs to spaces:

```bash
expand -t 4 source.txt > formatted.txt
```

Convert initial indentation spaces to tabs:

```bash
unexpand -t 4 formatted.txt > indented.txt
```

Convert only initial TAB characters:

```bash
expand -i -t 4 source.txt
```

**Important Notes**

- `expand` converts TAB characters into spaces.
- `unexpand` converts eligible spaces into TAB characters.
- Tab-stop positions determine how many spaces are required for each conversion.
- `unexpand` normally focuses on leading indentation; use `-a` to process spaces throughout lines.
- The appearance of text may remain similar even though the underlying TAB and space characters are different.
- Use these commands when consistent indentation or whitespace formatting is required.

**Summary**

Text-processing commands provide powerful ways to manipulate and analyze text directly from the Linux terminal.

| Command | Main Purpose |
|---|---|
| `cut` | Extract fields, characters, or bytes |
| `paste` | Merge corresponding lines from files |
| `sort` | Sort lines of text |
| `tr` | Translate, delete, or squeeze characters |
| `head` | Display the beginning of a file |
| `tail` | Display the end of a file |
| `join` | Combine records using a common field |
| `split` | Divide a file into smaller files |
| `uniq` | Identify or remove adjacent duplicates |
| `wc` | Count lines, words, bytes, and characters |
| `nl` | Number lines |
| `expand` | Convert tabs into spaces |
| `unexpand` | Convert eligible spaces into tabs |

**Useful Resources**

- [GNU Coreutils Manual](https://www.gnu.org/software/coreutils/manual/coreutils.html)
- [GNU Coreutils — cut](https://www.gnu.org/software/coreutils/manual/html_node/cut-invocation.html)
- [GNU Coreutils — sort](https://www.gnu.org/software/coreutils/manual/html_node/sort-invocation.html)
- [GNU Coreutils — tr](https://www.gnu.org/software/coreutils/manual/html_node/tr-invocation.html)
- [GNU Coreutils — head and tail](https://www.gnu.org/software/coreutils/manual/html_node/head-invocation.html)
- [GNU Coreutils — split](https://www.gnu.org/software/coreutils/manual/html_node/split-invocation.html)
- [GNU Coreutils — wc](https://www.gnu.org/software/coreutils/manual/html_node/wc-invocation.html)

---

## Pipes and Command Chaining

Linux provides powerful mechanisms for connecting commands and controlling how data flows between them.

Instead of executing every command separately and manually transferring its output to another command, you can combine multiple commands into a single pipeline.

Two important concepts used for this purpose are:

- **Pipes (`|`)** — Transfer the standard output of one command to the standard input of another command.
- **tee** — Read data from standard input, write it to standard output, and save a copy to one or more files.

These features are fundamental to shell scripting, log analysis, automation, and everyday Linux administration.

### Understanding Pipes

A pipe is a mechanism that connects the standard output (`stdout`) of one command to the standard input (`stdin`) of another command.

The pipe operator is represented by the vertical bar:

```bash
|
```

When commands are connected using pipes, the output of one command becomes the input of the next command.

**Basic Syntax**

```bash
command1 | command2
```

In this example:

1. `command1` executes.
2. Its standard output is sent through the pipe.
3. `command2` receives that data through standard input.
4. `command2` processes the data and produces its own output.

You can connect multiple commands:

```bash
command1 | command2 | command3 | command4
```

Each command processes the data received from the previous command.

**A Simple Example**

Display the contents of a file:

```bash
cat names.txt
```

Output:

```text
Charlie
Alice
Bob
David
```

Now sort the contents using a pipe:

```bash
cat names.txt | sort
```

Output:

```text
Alice
Bob
Charlie
David
```

The `cat` command sends the file contents to standard output, and `sort` receives that output through standard input.

**Using Pipes with Text Processing Commands**

Pipes become particularly useful when combining multiple text-processing utilities.

For example, remove duplicate lines from a file:

```bash
cat names.txt | sort | uniq
```

The commands work as follows:

1. `cat` reads the file.
2. `sort` arranges the lines alphabetically.
3. `uniq` removes adjacent duplicate lines.

Another example: count the number of lines in a file:

```bash
cat names.txt | wc -l
```

However, when a command accepts a filename directly, using it without `cat` is usually simpler:

```bash
wc -l names.txt
```

**Combining Multiple Commands**

You can build longer pipelines by connecting several commands.

Example: Find the most frequently occurring IP addresses in a log file.

```bash
cut -d ' ' -f 1 access.log | sort | uniq -c | sort -nr | head -n 10
```

Explanation:

1. `cut -d ' ' -f 1` extracts the first field from each line.
2. `sort` groups identical IP addresses together.
3. `uniq -c` counts the occurrences of each address.
4. `sort -nr` sorts the results by numerical frequency in descending order.
5. `head -n 10` displays the ten most frequent results.

This example assumes that the first whitespace-separated field in the log contains the IP address.

**Using Pipes with grep**

The `grep` command searches for lines matching a specified pattern.

Example:

```bash
cat /var/log/syslog | grep "error"
```

This displays lines containing the word `error`.

A simpler approach is:

```bash
grep "error" /var/log/syslog
```

You can also combine `grep` with other commands.

Find all running processes containing the word `ssh`:

```bash
ps aux | grep ssh
```

This searches the output of `ps aux` for matching lines.

**Using Pipes with head and tail**

You can use pipes to extract specific portions of command output.

Display the first five processes:

```bash
ps aux | head -n 5
```

Display the last five lines of a file:

```bash
cat file.txt | tail -n 5
```

A shorter alternative is:

```bash
tail -n 5 file.txt
```

Display lines 11 through 20:

```bash
cat file.txt | head -n 20 | tail -n 10
```

**Understanding Standard Error and Pipes**

By default, a pipe transfers only standard output (`stdout`).

Standard error (`stderr`) is not automatically sent through the pipe.

Consider:

```bash
find / -name "config.txt" | grep "/etc"
```

The `find` command searches the filesystem, but permission errors are written to standard error and do not pass through the pipe.

To redirect standard error into standard output before piping:

```bash
find / -name "config.txt" 2>&1 | grep "/etc"
```

Now both standard output and standard error are sent through the pipe.

However, this also means that error messages may be processed by `grep` as ordinary input.

**Piping Only Standard Error**

You can redirect standard output away and send standard error through a pipe.

Example:

```bash
find /root -name "config.txt" 2>&1 1>/dev/null | grep "Permission denied"
```

Explanation:

- `2>&1` initially redirects standard error to the current standard output.
- `1>/dev/null` then redirects standard output to `/dev/null`.
- Standard error continues to point to the original standard output, which is the pipe.

This allows the error messages to reach `grep` while normal output is discarded.

**Using Pipes with Redirection**

You can combine pipes and output redirection.

Example:

```bash
sort names.txt | uniq > unique-names.txt
```

This sorts the file, removes adjacent duplicate lines, and saves the result in `unique-names.txt`.

You can also append the output:

```bash
sort names.txt | uniq >> unique-names.txt
```

The `>` operator overwrites the destination file, while `>>` appends to it.

**Understanding Pipeline Exit Status**

Normally, the exit status of a pipeline is the exit status of its last command.

Example:

```bash
false | true
echo $?
```

Output:

```text
0
```

Although `false` failed, the final command, `true`, succeeded.

In Bash, the `pipefail` option changes this behavior.

Enable it:

```bash
set -o pipefail
```

Now a pipeline returns a non-zero status if at least one command fails. The returned status is generally that of the rightmost command that failed.

Example:

```bash
false | true
echo $?
```

Output:

```text
1
```

Disable the option:

```bash
set +o pipefail
```

You can also inspect the exit status of every command in the most recent pipeline using the Bash array `PIPESTATUS`:

```bash
false | true
echo "${PIPESTATUS[@]}"
```

Output:

```text
1 0
```

The first value belongs to `false`, and the second belongs to `true`.

**Using Pipes in Bash Scripts**

Pipes are frequently used in shell scripts to process data efficiently.

Example:

```bash
#!/bin/bash

LOG_FILE="application.log"

grep -i "error" "$LOG_FILE" | sort | uniq -c
```

This script:

1. Searches for lines containing `error`, ignoring case.
2. Sorts the matching lines.
3. Counts repeated adjacent lines.

For safer pipeline error handling in Bash scripts, you can enable:

```bash
set -o pipefail
```

**Important Notes**

- A pipe connects the standard output of one command to the standard input of another.
- Standard error is not included in a normal pipe.
- You can connect multiple commands using several pipe operators.
- Pipes allow commands to process data without creating intermediate files.
- The output of one command can be redirected to a file after the final pipeline stage.
- By default, a pipeline's exit status is determined by its last command.
- Use `set -o pipefail` when pipeline failures must be detected.
- Shell pipelines normally run connected commands concurrently, allowing data to flow between them as it becomes available.

### Working with tee

The `tee` command reads data from standard input, writes it to standard output, and saves a copy to one or more files.

It is especially useful when you want to view command output in the terminal while also saving it for later use.

The name `tee` comes from the T-shaped pipe fitting, which divides one input stream into two outputs.

**Syntax**

```bash
tee [OPTION...] [FILE...]
```

The command performs three operations:

1. Reads data from standard input.
2. Writes the data to standard output.
3. Saves the data to the specified file or files.

**Basic Usage**

Consider the following command:

```bash
ls -l | tee output.txt
```

This command:

- Executes `ls -l`.
- Sends its output to `tee`.
- Displays the output in the terminal.
- Saves the same output in `output.txt`.

You can verify the saved content:

```bash
cat output.txt
```

Unlike ordinary output redirection, the `tee` command allows you to see the output while saving it.

**Comparing tee with Output Redirection**

Consider these two commands:

Using redirection:

```bash
ls -l > output.txt
```

Using `tee`:

```bash
ls -l | tee output.txt
```

The difference is:

| Method | Displays Output | Saves to File |
|---|---|---|
| `>` | No | Yes |
| `tee` | Yes | Yes |

The `>` operator redirects standard output directly to a file, while `tee` duplicates the input stream to both standard output and the specified file.

**Appending Instead of Overwriting**

By default, `tee` overwrites the destination file if it already exists.

The `-a` option appends new data instead.

Example:

```bash
date | tee -a output.txt
```

This adds the current date and time to the end of `output.txt` without deleting its existing contents.

You can append multiple command outputs:

```bash
whoami | tee -a output.txt
hostname | tee -a output.txt
date | tee -a output.txt
```

**Writing to Multiple Files**

You can provide multiple filenames to `tee`.

Example:

```bash
ls -l | tee output1.txt output2.txt
```

The output is displayed in the terminal and saved to both files.

Verify the files:

```bash
cat output1.txt
cat output2.txt
```

**Using tee in a Pipeline**

The `tee` command is particularly useful when placed in the middle of a pipeline.

Example:

```bash
cat names.txt | tee original.txt | sort
```

The data flow is:

1. `cat` reads `names.txt`.
2. `tee` displays the original content and saves it to `original.txt`.
3. `sort` receives the same content and sorts it.

The final output is sorted, while `original.txt` contains the original input order.

You can also save intermediate results:

```bash
cat access.log | grep "error" | tee errors.txt | sort | uniq
```

This command:

1. Reads the log file.
2. Filters lines containing `error`.
3. Saves the matching lines in `errors.txt`.
4. Sorts the matching lines.
5. Removes adjacent duplicate lines.

This approach is useful when debugging complex pipelines because you can inspect intermediate results.

**Discarding Standard Output**

The `-` filename represents standard output in GNU `tee`.

You can use `/dev/null` to discard standard output while still saving the data to a file.

Example:

```bash
ls -l | tee output.txt > /dev/null
```

The output is saved in `output.txt` but is not displayed in the terminal.

**Ignoring Interrupt Signals**

The `-i` option tells `tee` to ignore interrupt signals.

Example:

```bash
long-running-command | tee output.txt
```

You can use:

```bash
long-running-command | tee -i output.txt
```

This option is useful in certain situations where the pipeline should continue writing data despite an interrupt signal being received.

**Using tee with sudo**

A common Linux problem occurs when trying to write to a protected file using `sudo` with output redirection.

For example:

```bash
sudo echo "Example configuration" > /etc/example.conf
```

This may fail with a permission error.

The reason is that the shell performs the redirection before executing `sudo`. The shell itself does not have permission to write to the protected file.

The `tee` command provides a solution:

```bash
echo "Example configuration" | sudo tee /etc/example.conf
```

Here:

1. `echo` sends the text to standard output.
2. The shell pipes the text to `tee`.
3. `sudo` executes `tee` with elevated privileges.
4. `tee` writes the content to the protected file.

To append instead of overwrite:

```bash
echo "Example configuration" | sudo tee -a /etc/example.conf
```

**Important:** The `tee` command writes to the destination file with the permissions of the process executing it. When used with `sudo`, it can overwrite important system files. Always verify the destination path before running privileged commands.

**Important Options**

| Option | Description |
|---|---|
| `-a` | Append to files instead of overwriting |
| `-i` | Ignore interrupt signals |
| `-p` | Diagnose errors when writing to pipes (GNU coreutils) |

**Practical Examples**

Save command output while displaying it:

```bash
df -h | tee disk-usage.txt
```

Append system information to a log:

```bash
uname -a | tee -a system-info.txt
date | tee -a system-info.txt
```

Save output to multiple files:

```bash
who | tee users1.txt users2.txt
```

Save filtered log entries and continue processing:

```bash
grep -i "error" application.log | tee errors.txt | sort
```

Write to a protected configuration file:

```bash
echo "Example configuration" | sudo tee /etc/example.conf
```

**Important Notes**

- `tee` reads from standard input.
- It writes the received data to standard output and one or more files.
- By default, it overwrites existing destination files.
- Use `-a` to append data.
- It is particularly useful for inspecting intermediate pipeline results.
- `sudo tee` is useful when writing to protected files.
- `tee` does not automatically capture standard error. Redirect standard error into standard output before passing it to `tee` if you need to save both.

**Summary**

Pipes and `tee` are essential tools for connecting commands and managing data streams in Linux.

| Feature | Main Purpose |
|---|---|
| `\|` | Connect the standard output of one command to the standard input of another |
| `set -o pipefail` | Detect failures in pipeline commands |
| `PIPESTATUS` | Inspect individual pipeline exit statuses in Bash |
| `tee` | Display and save data simultaneously |
| `tee -a` | Append data to files |
| `sudo tee` | Write to protected files with elevated privileges |

**Useful Resources**

- [GNU Bash Manual — Pipelines](https://www.gnu.org/software/bash/manual/html_node/Pipelines.html)
- [GNU Bash Manual — Redirections](https://www.gnu.org/software/bash/manual/html_node/Redirections.html)
- [GNU Coreutils Manual — tee](https://www.gnu.org/software/coreutils/manual/html_node/tee-invocation.html)

---

## Searching and Processing Text

Text searching and processing are essential skills when working with Linux systems. System administrators and developers frequently need to search configuration files, inspect log files, filter command output, extract specific information, and analyze structured text data.

Linux provides several tools for these tasks, but two of the most powerful and widely used utilities are `grep` and `awk`.

- **grep:** Searches files and input streams for lines matching specified patterns. It is particularly useful for finding text, filtering command output, and searching with regular expressions.
- **awk:** Processes text as structured records and fields. It can filter data, extract columns, perform calculations, format output, and execute more advanced text-processing operations.

Although both tools can search and filter text, they serve different purposes. `grep` primarily identifies matching lines, while `awk` provides a programming language for processing and analyzing structured text.

---

### Working with grep

#### Introduction to grep

`grep` is a command-line utility used to search for patterns in files or standard input. It examines text line by line and displays lines that match a specified pattern.

The name `grep` comes from the historical `ed` editor command:

```text
g/re/p
```

This stands for **Global Regular Expression Print**.

The command is particularly useful for:

- Searching for specific words or phrases in files.
- Finding errors and warnings in system logs.
- Filtering command output.
- Searching multiple files and directories.
- Matching patterns using regular expressions.
- Finding configuration entries.
- Identifying lines that do not match a specified pattern.

**Basic Syntax:**

```bash
grep [OPTIONS] PATTERN [FILE...]
```

| Component | Description |
|---|---|
| `grep` | The command used to search text. |
| `OPTIONS` | Optional flags that control search behavior. |
| `PATTERN` | The text or regular expression to search for. |
| `FILE...` | One or more files to search. |

If no file is specified, `grep` reads from standard input.

**Example:**

```bash
grep "error" application.log
```

This command searches `application.log` for lines containing the word `error`.

#### Basic Syntax

The simplest way to use `grep` is to provide a search pattern and a filename.

```bash
grep "Linux" notes.txt
```

This displays every line containing the exact case-sensitive text `Linux`.

You can also search for multiple words independently:

```bash
grep "Linux" notes.txt
grep "Ubuntu" notes.txt
grep "Server" notes.txt
```

To search for a phrase containing spaces, enclose the pattern in quotation marks:

```bash
grep "system administration" notes.txt
```

You can search multiple files in a single command:

```bash
grep "error" app.log system.log
```

When searching multiple files, `grep` normally includes the filename before each matching line.

#### Searching for Text Patterns

By default, `grep` searches for a pattern anywhere within a line.

For example, suppose `users.txt` contains:

```text
root
administrator
linuxuser
developer
guest
```

The following command:

```bash
grep "user" users.txt
```

Produces:

```text
linuxuser
```

The pattern does not need to match the entire line. It only needs to appear somewhere in the line.

You can also search for text at the beginning or end of a line using regular expression anchors.

**Matching the beginning of a line:**

```bash
grep "^root" users.txt
```

The `^` character matches the beginning of a line.

**Matching the end of a line:**

```bash
grep "admin$" users.txt
```

The `$` character matches the end of a line.

**Matching an entire line:**

```bash
grep "^root$" users.txt
```

This matches only a line containing exactly `root`.

#### Case-Insensitive Searches

By default, `grep` distinguishes between uppercase and lowercase characters.

For example:

```bash
grep "linux" notes.txt
```

Does not match `Linux` or `LINUX`.

To perform a case-insensitive search, use the `-i` option.

```bash
grep -i "linux" notes.txt
```

This matches all of the following:

```text
linux
Linux
LINUX
LiNuX
```

Case-insensitive searching is particularly useful when searching logs and configuration files where capitalization may vary.

#### Searching Multiple Files

`grep` can search several files at the same time.

```bash
grep "failed" auth.log syslog application.log
```

This searches all three files for lines containing `failed`.

You can also use wildcard patterns to search files with similar names.

```bash
grep "error" *.log
```

This searches all files in the current directory whose names end with `.log`.

To display only the filenames containing matches, use `-l`.

```bash
grep -l "error" *.log
```

To display filenames that do not contain any matches, use `-L`.

```bash
grep -L "error" *.log
```

These options are useful when checking large collections of log or configuration files.

#### Recursive Searching

The `-r` or `-R` option allows `grep` to search through directories recursively.

```bash
grep -r "password" /etc/
```

This searches files under `/etc/` and its subdirectories.

The difference between the two options is important:

- `-r`: Recursively searches directories and generally follows symbolic links encountered on the command line.
- `-R`: Recursively searches directories and follows symbolic links encountered during traversal.

To display filenames and line numbers:

```bash
grep -rn "PermitRootLogin" /etc/ssh/
```

To search only files with a specific extension, use `--include`.

```bash
grep -rn --include="*.conf" "listen" /etc/
```

To exclude certain files:

```bash
grep -rn --exclude="*.log" "error" /var/log/
```

To exclude entire directories:

```bash
grep -rn --exclude-dir="cache" "error" /var/
```

Recursive searches may produce permission errors when accessing protected directories. You can use appropriate privileges when necessary:

```bash
sudo grep -rn "PermitRootLogin" /etc/ssh/
```

#### Displaying Matching Line Numbers

The `-n` option displays the line number of each matching line.

```bash
grep -n "error" application.log
```

Example output:

```text
12:Connection error detected
25:Database error occurred
41:Unexpected error during startup
```

This makes it easier to locate matching content within large files.

To display only the number of matching lines, use `-c`.

```bash
grep -c "error" application.log
```

**Important:** `grep -c` counts matching lines, not the total number of occurrences of a pattern within those lines.

For example, if a line contains the word `error` three times, it still contributes only one to the count.

#### Matching Whole Words and Lines

Sometimes you need to find a complete word rather than a sequence of characters inside another word.

Use `-w` to match whole words.

```bash
grep -w "root" users.txt
```

This matches `root` but does not match words such as `rootuser` or `superroot`.

To match an entire line exactly, use `-x`.

```bash
grep -x "root" users.txt
```

This matches only lines whose complete content is `root`.

These options are particularly useful when searching usernames, command names, configuration values, and other identifiers.

#### Inverting Search Results

The `-v` option reverses the matching behavior.

Instead of displaying lines that match the pattern, it displays lines that do not match it.

```bash
grep -v "error" application.log
```

This displays every line that does not contain `error`.

You can combine it with other options:

```bash
grep -vn "debug" application.log
```

This displays line numbers and excludes lines containing `debug`.

Inverted searches are useful for removing unwanted entries from logs or filtering out irrelevant information.

#### Counting Matches

The `-c` option displays the number of matching lines.

```bash
grep -c "failed" auth.log
```

To count matching lines across multiple files:

```bash
grep -c "failed" *.log
```

To count the total number of matching lines across all files, use `-h` to suppress filenames and pipe the result into `awk` or another counting utility.

```bash
grep -ch "failed" *.log | awk '{sum += $1} END {print sum}'
```

To count individual occurrences rather than matching lines, use `-o` with `wc -l`.

```bash
grep -o "error" application.log | wc -l
```

The `-o` option prints each matching occurrence separately, allowing `wc -l` to count them.

#### Displaying Context Around Matches

When investigating logs, it is often useful to see the lines surrounding a match.

`grep` provides three useful options:

**1. Display lines before a match with `-B`:**

```bash
grep -B 3 "error" application.log
```

Displays three lines before each matching line.

**2. Display lines after a match with `-A`:**

```bash
grep -A 3 "error" application.log
```

Displays three lines after each matching line.

**3. Display lines before and after a match with `-C`:**

```bash
grep -C 3 "error" application.log
```

Displays three lines before and after each matching line.

These options are especially useful when examining application errors and system events because the surrounding lines may explain what happened before or after the event.

#### Using Regular Expressions

Regular expressions allow `grep` to search for more complex patterns rather than only literal text.

By default, `grep` uses Basic Regular Expressions (BRE).

Some important regular expression characters include:

| Character | Meaning | Example |
|---|---|---|
| `^` | Beginning of a line | `^root` |
| `$` | End of a line | `admin$` |
| `.` | Any single character | `l.nux` |
| `*` | Zero or more occurrences of the preceding element | `ab*c` |
| `[]` | Character class | `[abc]` |
| `[^]` | Negated character class | `[^0-9]` |
| `\` | Escapes a special character or introduces certain BRE operators | `\.` |
| `\{n\}` | Exactly n repetitions | `a\{3\}` |
| `\{n,m\}` | Between n and m repetitions | `a\{2,4\}` |

**Examples:**

```bash
grep "^root" users.txt
```

Finds lines beginning with `root`.

```bash
grep "error$" application.log
```

Finds lines ending with `error`.

```bash
grep "l.nux" notes.txt
```

Matches patterns such as `linux`, `lanux`, and `l.nux`.

```bash
grep "^[0-9]" numbers.txt
```

Finds lines beginning with a digit.

```bash
grep "^[^0-9]" data.txt
```

Finds lines that do not begin with a digit.

To search for a literal dot, escape it:

```bash
grep "192\.168\.1\.10" network.log
```

Without escaping, the dot matches any single character.

#### Extended Regular Expressions

The `-E` option enables Extended Regular Expressions (ERE).

ERE provides additional operators that make complex patterns easier to write.

Important ERE operators include:

| Operator | Meaning | Example |
|---|---|---|
| `+` | One or more occurrences | `[0-9]+` |
| `?` | Zero or one occurrence | `colou?r` |
| `|` | Alternative patterns | `error|warning` |
| `()` | Groups expressions | `(ab)+` |
| `{n}` | Exactly n repetitions | `[0-9]{4}` |
| `{n,m}` | Between n and m repetitions | `[0-9]{2,4}` |

**Examples:**

Search for either `error` or `warning`:

```bash
grep -E "error|warning" application.log
```

Search for one or more digits:

```bash
grep -E "[0-9]+" numbers.txt
```

Search for lines containing a four-digit number:

```bash
grep -E "[0-9]{4}" data.txt
```

Search for either `color` or `colour`:

```bash
grep -E "colou?r" text.txt
```

Search for IPv4-like sequences:

```bash
grep -E "[0-9]{1,3}(\.[0-9]{1,3}){3}" network.log
```

This last expression identifies strings with four dot-separated numeric groups, but it does not validate whether the address is a valid IPv4 address.

**Important:** The `-E` option is useful when working with complex patterns because it avoids the need to escape many operators required by Basic Regular Expressions.

#### Searching with Character Classes and Quantifiers

Character classes allow you to match one character from a specified set.

**Examples:**

```bash
grep "[aeiou]" words.txt
```

Matches lines containing at least one lowercase vowel.

```bash
grep "[A-Z]" words.txt
```

Matches lines containing at least one uppercase English letter.

```bash
grep "[0-9]" data.txt
```

Matches lines containing at least one digit.

```bash
grep "[^0-9]" data.txt
```

Matches lines containing at least one character that is not a digit.

You can also use predefined character classes:

| Character Class | Description |
|---|---|
| `[[:digit:]]` | Numeric digits |
| `[[:alpha:]]` | Alphabetic characters |
| `[[:alnum:]]` | Alphanumeric characters |
| `[[:lower:]]` | Lowercase characters |
| `[[:upper:]]` | Uppercase characters |
| `[[:space:]]` | Whitespace characters |
| `[[:blank:]]` | Space and tab characters |
| `[[:punct:]]` | Punctuation characters |
| `[[:xdigit:]]` | Hexadecimal digits |

**Examples:**

```bash
grep "[[:digit:]]" data.txt
grep "[[:alpha:]]" data.txt
grep "[[:space:]]" data.txt
```

These classes are useful for searching text based on character types rather than specific characters.

#### Combining grep with Pipes

One of the most common uses of `grep` is filtering the output of other commands through pipes.

**Searching running processes:**

```bash
ps aux | grep "nginx"
```

This searches the process listing for lines containing `nginx`.

Be aware that this command may also match the `grep` process itself.

An alternative is:

```bash
pgrep -a nginx
```

**Filtering system information:**

```bash
ls -l /etc | grep "\.conf$"
```

Displays entries whose names end with `.conf`.

**Searching command output:**

```bash
ip address | grep "inet "
```

Displays lines containing IPv4 address information.

**Finding errors in logs:**

```bash
journalctl -b | grep -i "error"
```

Searches the current boot's journal output for lines containing `error`, regardless of capitalization.

**Combining multiple filters:**

```bash
cat application.log | grep -i "error" | grep -v "temporary"
```

Displays lines containing `error` but excludes lines containing `temporary`.

For a single file, using `grep` directly is usually simpler than passing it through `cat`.

#### Useful grep Options

The following table summarizes commonly used `grep` options:

| Option | Description |
|---|---|
| `-i` | Ignore uppercase and lowercase differences. |
| `-v` | Display lines that do not match. |
| `-n` | Display matching line numbers. |
| `-c` | Count matching lines. |
| `-l` | Display filenames containing matches. |
| `-L` | Display filenames without matches. |
| `-r` | Search directories recursively. |
| `-R` | Search recursively and follow symbolic links encountered during traversal. |
| `-w` | Match whole words. |
| `-x` | Match entire lines. |
| `-o` | Display only the matching parts. |
| `-E` | Use Extended Regular Expressions. |
| `-F` | Treat the pattern as fixed strings rather than regular expressions. |
| `-e` | Specify a search pattern. |
| `-f` | Read search patterns from a file. |
| `-A N` | Display N lines after a match. |
| `-B N` | Display N lines before a match. |
| `-C N` | Display N lines before and after a match. |
| `-q` | Suppress normal output and use the exit status to indicate whether a match exists. |
| `-h` | Suppress filenames in output. |
| `-H` | Always display filenames. |

You can combine multiple options:

```bash
grep -in "error" application.log
```

This performs a case-insensitive search and displays matching line numbers.

Another example:

```bash
grep -rni "password" /etc/
```

This recursively searches `/etc/`, ignores case, and displays matching line numbers.

#### Practical Examples

**Example 1 — Find failed login attempts:**

```bash
grep "Failed password" /var/log/auth.log
```

Searches authentication logs for failed SSH or other authentication attempts recorded in that file.

The exact log location and message format depend on the Linux distribution and logging configuration.

**Example 2 — Find SSH configuration settings:**

```bash
grep -n "PermitRootLogin" /etc/ssh/sshd_config
```

Displays matching SSH server configuration lines and their line numbers.

**Example 3 — Find all error messages:**

```bash
grep -in "error" /var/log/syslog
```

Searches the system log for error messages without considering capitalization.

**Example 4 — Exclude comments and empty lines:**

```bash
grep -Ev "^[[:space:]]*(#|$)" /etc/ssh/sshd_config
```

Displays configuration lines that are neither comments nor empty or whitespace-only lines.

**Example 5 — Search for multiple patterns:**

```bash
grep -E "error|warning|critical" application.log
```

Displays lines containing any of the specified words.

**Example 6 — Check whether a user exists:**

```bash
grep -w "root" /etc/passwd
```

Searches the user account database for a line containing the username `root` as a complete word.

For a more precise username lookup, you can use:

```bash
awk -F: '$1 == "root" {print}' /etc/passwd
```

**Example 7 — Search for a pattern stored in another file:**

Suppose `patterns.txt` contains:

```text
error
warning
critical
```

Run:

```bash
grep -f patterns.txt application.log
```

This searches `application.log` for any pattern listed in `patterns.txt`.

**Example 8 — Search for exact text:**

```bash
grep -F "192.168.1.10" network.log
```

The `-F` option treats the search pattern literally, so special regular expression characters have no special meaning.

#### Understanding grep Exit Status

`grep` uses its exit status to communicate the result of a search.

| Exit Status | Meaning |
|---|---|
| `0` | At least one matching line was found. |
| `1` | No matching lines were found. |
| `2` | An error occurred. |

You can inspect the exit status using `$?`:

```bash
grep "error" application.log
echo $?
```

This is useful in shell scripts and conditional statements.

For example:

```bash
if grep -q "error" application.log; then
    echo "Errors were found."
else
    echo "No errors were found."
fi
```

The `-q` option suppresses normal output, making it particularly useful for checking whether a pattern exists.

**Important:** A search that returns no matches is not necessarily an error. It simply means the requested pattern was not found.

---

### Working with awk

#### Introduction to awk

`awk` is a powerful text-processing programming language designed for working with structured text data.

It reads input line by line, divides each line into fields, and executes instructions based on specified patterns and actions.

Unlike `grep`, which primarily searches for matching lines, `awk` can:

- Extract specific columns from text files.
- Filter records based on conditions.
- Perform mathematical calculations.
- Count records and summarize information.
- Modify fields and construct new output.
- Format data into readable reports.
- Process structured files such as CSV-like data and system configuration files.
- Automate repetitive text-processing tasks.

The name `awk` comes from the surnames of its original creators:

- Alfred Aho
- Peter Weinberger
- Brian Kernighan

The original `awk` language was developed at Bell Labs in the 1970s.

Modern Linux systems commonly provide GNU `awk`, also known as `gawk`.

**Basic Syntax:**

```bash
awk 'pattern { action }' file
```

| Component | Description |
|---|---|
| `awk` | The text-processing utility. |
| `pattern` | An optional condition that determines when an action runs. |
| `{ action }` | Instructions executed when the pattern matches. |
| `file` | The input file to process. |

If no pattern is specified, the action is executed for every input record.

If no action is specified, matching records are printed.

#### Basic Syntax

The simplest `awk` command prints every line of a file:

```bash
awk '{ print }' file.txt
```

This is equivalent to:

```bash
cat file.txt
```

You can also use:

```bash
awk '1' file.txt
```

In this example, `1` is a condition that always evaluates to true, so every record is printed.

To print a specific message:

```bash
awk '{ print "Hello, Linux!" }' file.txt
```

This prints the message once for every input record.

You can also execute `awk` directly on command output:

```bash
ps aux | awk '{ print $1, $2, $11 }'
```

This extracts selected fields from the process listing.

#### Understanding Records and Fields

Understanding records and fields is fundamental to working with `awk`.

By default:

- A **record** is one input line.
- A **field** is a section of a record separated by whitespace.
- `$0` represents the entire current record.
- `$1` represents the first field.
- `$2` represents the second field.
- `$3` represents the third field.
- `$NF` represents the last field.
- `NF` stores the number of fields in the current record.
- `NR` stores the total number of records read so far.

Consider the following file named `employees.txt`:

```text
Alice Developer 5000
Bob Administrator 4500
Charlie Engineer 6000
David Designer 4000
```

The following command:

```bash
awk '{ print $1 }' employees.txt
```

Produces:

```text
Alice
Bob
Charlie
David
```

To display the second field:

```bash
awk '{ print $2 }' employees.txt
```

To display the first and third fields:

```bash
awk '{ print $1, $3 }' employees.txt
```

To display the entire record:

```bash
awk '{ print $0 }' employees.txt
```

To display the last field:

```bash
awk '{ print $NF }' employees.txt
```

To display the number of fields in each record:

```bash
awk '{ print NF }' employees.txt
```

This produces:

```text
3
3
3
3
```

These features make it easy to extract information from structured text without manually splitting each line.

#### Built-in Variables

`awk` provides several built-in variables that help control input processing and output formatting.

The most important variables include:

| Variable | Description |
|---|---|
| `$0` | The entire current record. |
| `$1`, `$2`, ... | Individual fields in the current record. |
| `NF` | Number of fields in the current record. |
| `NR` | Total number of records read across all input files. |
| `FNR` | Record number within the current input file. |
| `FS` | Input field separator. |
| `OFS` | Output field separator. |
| `RS` | Input record separator. |
| `ORS` | Output record separator. |
| `FILENAME` | Name of the current input file. |
| `ARGC` | Number of command-line arguments. |
| `ARGV` | Array containing command-line arguments. |

**Examples:**

Display the current record number:

```bash
awk '{ print NR, $0 }' employees.txt
```

Output:

```text
1 Alice Developer 5000
2 Bob Administrator 4500
3 Charlie Engineer 6000
4 David Designer 4000
```

Display the filename and record number:

```bash
awk '{ print FILENAME, NR, $0 }' employees.txt
```

Display the number of fields in each record:

```bash
awk '{ print NF }' employees.txt
```

Display the first and last fields:

```bash
awk '{ print $1, $NF }' employees.txt
```

#### Printing Fields and Records

The `print` statement is used to display data.

You can print individual fields:

```bash
awk '{ print $1 }' employees.txt
```

You can print multiple fields:

```bash
awk '{ print $1, $2, $3 }' employees.txt
```

By default, `awk` separates multiple `print` arguments using a space.

You can also combine text with fields:

```bash
awk '{ print "Employee:", $1, "| Role:", $2 }' employees.txt
```

To print a custom message at the beginning of each record:

```bash
awk '{ print "Record:", $0 }' employees.txt
```

To print only selected information:

```bash
awk '{ print "Name:", $1, "Salary:", $3 }' employees.txt
```

The `printf` statement, discussed later, provides more control over output formatting.

#### Using Field Separators

By default, `awk` separates fields using whitespace, including spaces and tabs.

However, many files use different delimiters, such as colons or commas.

The `-F` option allows you to specify a custom input field separator.

**Example 1 — Working with `/etc/passwd`:**

The `/etc/passwd` file uses colons as field separators.

```text
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
```

To display usernames:

```bash
awk -F: '{ print $1 }' /etc/passwd
```

To display usernames and home directories:

```bash
awk -F: '{ print $1, $6 }' /etc/passwd
```

To display usernames and login shells:

```bash
awk -F: '{ print $1, $7 }' /etc/passwd
```

**Example 2 — Working with CSV-like data:**

Suppose `employees.csv` contains:

```text
Name,Department,Salary
Alice,IT,5000
Bob,HR,4500
Charlie,Engineering,6000
```

Display employee names:

```bash
awk -F, '{ print $1 }' employees.csv
```

Display names and salaries:

```bash
awk -F, '{ print $1, $3 }' employees.csv
```

**Important:** Using `-F,` is suitable for simple comma-separated data, but it does not implement the full CSV format. CSV files may contain quoted fields, embedded commas, and escaped quotation marks. For complex CSV files, use a dedicated CSV parser.

#### Filtering Records with Patterns

One of the most powerful features of `awk` is the ability to execute actions only when records match specific conditions.

For example:

```bash
awk '$3 > 5000 { print $1, $3 }' employees.txt
```

This displays employees whose third field is greater than `5000`.

Output:

```text
Charlie 6000
```

You can use different comparison operators:

| Operator | Meaning |
|---|---|
| `==` | Equal to |
| `!=` | Not equal to |
| `>` | Greater than |
| `<` | Less than |
| `>=` | Greater than or equal to |
| `<=` | Less than or equal to |

**Examples:**

Display employees with a salary equal to `5000`:

```bash
awk '$3 == 5000 { print $1 }' employees.txt
```

Display employees with a salary below `5000`:

```bash
awk '$3 < 5000 { print $1, $3 }' employees.txt
```

Display employees whose role is `Developer`:

```bash
awk '$2 == "Developer" { print $1 }' employees.txt
```

Display employees whose salary is not `4500`:

```bash
awk '$3 != 4500 { print $1 }' employees.txt
```

These conditions allow you to filter large datasets without displaying every record.

#### Using Comparison and Logical Operators

`awk` supports logical operators that allow you to combine multiple conditions.

| Operator | Meaning |
|---|---|
| `&&` | Logical AND |
| `||` | Logical OR |
| `!` | Logical NOT |

**Logical AND:**

```bash
awk '$3 > 4000 && $3 < 6000 { print $1, $3 }' employees.txt
```

Displays employees whose salaries are greater than `4000` and less than `6000`.

**Logical OR:**

```bash
awk '$2 == "Developer" || $2 == "Engineer" { print $1, $2 }' employees.txt
```

Displays employees whose roles are either `Developer` or `Engineer`.

**Logical NOT:**

```bash
awk '!($2 == "Administrator") { print $1, $2 }' employees.txt
```

Displays all employees except administrators.

You can combine several conditions to build more advanced filters.

#### Working with Regular Expressions

`awk` supports regular expressions for searching text patterns.

A regular expression can be used directly as a pattern:

```bash
awk '/error/ { print }' application.log
```

This prints records containing `error`.

You can also match a regular expression against a specific field using the `~` operator.

```bash
awk '$2 ~ /Developer/ { print $1, $2 }' employees.txt
```

This displays records where the second field contains `Developer`.

To find fields that do not match a pattern, use `!~`.

```bash
awk '$2 !~ /Admin/ { print $1, $2 }' employees.txt
```

**Examples:**

Find records beginning with a specific word:

```bash
awk '/^root/ { print }' users.txt
```

Find records ending with a specific value:

```bash
awk '/admin$/ { print }' users.txt
```

Find records containing digits:

```bash
awk '/[0-9]/ { print }' data.txt
```

Find records where the first field begins with an uppercase letter:

```bash
awk '$1 ~ /^[A-Z]/ { print $1 }' employees.txt
```

Regular expressions make `awk` useful for more advanced filtering tasks.

#### Using BEGIN and END Blocks

`awk` provides three main execution stages:

1. `BEGIN`: Executes before input records are processed.
2. Main processing block: Executes for each input record.
3. `END`: Executes after all input records have been processed.

**Basic Structure:**

```bash
awk 'BEGIN { ... } { ... } END { ... }' file.txt
```

**BEGIN Block:**

The `BEGIN` block is commonly used to initialize variables or print headers.

```bash
awk 'BEGIN {
    print "Employee Report"
    print "---------------"
}' employees.txt
```

**Main Processing Block:**

The main block processes each record.

```bash
awk '{
    print $1, $2, $3
}' employees.txt
```

**END Block:**

The `END` block is commonly used to display summaries or calculated results.

```bash
awk 'END {
    print "Processing completed."
}' employees.txt
```

**Combining All Three:**

```bash
awk '
BEGIN {
    print "Employee Report"
    print "----------------"
}
{
    print "Name:", $1, "| Salary:", $3
}
END {
    print "----------------"
    print "Report completed."
}
' employees.txt
```

This produces a formatted report containing every employee.

#### Performing Calculations

`awk` supports arithmetic operations and can perform calculations directly on fields.

Common arithmetic operators include:

| Operator | Description |
|---|---|
| `+` | Addition |
| `-` | Subtraction |
| `*` | Multiplication |
| `/` | Division |
| `%` | Modulus |
| `^` | Exponentiation |
| `++` | Increment |
| `--` | Decrement |

**Example 1 — Calculate total salary:**

```bash
awk '{ total += $3 } END { print "Total Salary:", total }' employees.txt
```

This adds the salary field from every record and displays the final result.

**Example 2 — Calculate average salary:**

```bash
awk '
{
    total += $3
    count++
}
END {
    if (count > 0)
        print "Average Salary:", total / count
}
' employees.txt
```

**Example 3 — Find the highest salary:**

```bash
awk '
NR == 1 || $3 > max {
    max = $3
    employee = $1
}
END {
    print "Highest Paid:", employee
    print "Salary:", max
}
' employees.txt
```

**Example 4 — Find the lowest salary:**

```bash
awk '
NR == 1 || $3 < min {
    min = $3
    employee = $1
}
END {
    print "Lowest Paid:", employee
    print "Salary:", min
}
' employees.txt
```

**Example 5 — Count records:**

```bash
awk 'END { print "Total Employees:", NR }' employees.txt
```

These operations are particularly useful for analyzing reports, log files, and structured datasets.

#### Built-in Functions

`awk` includes several built-in functions for working with strings, numbers, and other data.

**1. String Functions**

| Function | Description |
|---|---|
| `length(string)` | Returns the number of characters in a string. |
| `tolower(string)` | Converts text to lowercase. |
| `toupper(string)` | Converts text to uppercase. |
| `substr(string, start, length)` | Extracts part of a string. |
| `index(string, search)` | Returns the position of a substring or zero if not found. |
| `split(string, array, separator)` | Splits a string into an array. |
| `sub(regex, replacement, target)` | Replaces the first matching occurrence. |
| `gsub(regex, replacement, target)` | Replaces all matching occurrences. |

**Examples:**

Convert names to uppercase:

```bash
awk '{ print toupper($1) }' employees.txt
```

Convert names to lowercase:

```bash
awk '{ print tolower($1) }' employees.txt
```

Display the length of each name:

```bash
awk '{ print $1, length($1) }' employees.txt
```

Extract the first three characters:

```bash
awk '{ print substr($1, 1, 3) }' employees.txt
```

Replace the word `Developer` with `Programmer`:

```bash
awk '{ gsub(/Developer/, "Programmer"); print }' employees.txt
```

**2. Numeric Functions**

| Function | Description |
|---|---|
| `int(number)` | Returns the integer portion of a number, truncating toward zero. |
| `sqrt(number)` | Calculates the square root. |
| `log(number)` | Calculates the natural logarithm. |
| `exp(number)` | Calculates e raised to the specified power. |
| `sin(number)` | Calculates the sine. |
| `cos(number)` | Calculates the cosine. |
| `rand()` | Generates a pseudo-random number between 0 and 1. |
| `srand(seed)` | Sets the random number generator seed. |

**Examples:**

```bash
awk 'BEGIN { print int(12.75) }'
awk 'BEGIN { print sqrt(64) }'
awk 'BEGIN { print log(10) }'
awk 'BEGIN { print rand() }'
```

The `BEGIN` block allows you to execute these calculations without requiring an input file.

#### Using Conditional Statements

`awk` supports conditional statements such as `if`, `else if`, and `else`.

**Basic Syntax:**

```awk
if (condition) {
    statements
} else if (another_condition) {
    statements
} else {
    statements
}
```

**Example:**

```bash
awk '{
    if ($3 >= 5500)
        print $1, "High Salary"
    else if ($3 >= 4500)
        print $1, "Medium Salary"
    else
        print $1, "Low Salary"
}' employees.txt
```

This classifies employees according to their salaries.

You can also use conditional statements to validate input:

```bash
awk -F: '{
    if ($7 == "/bin/bash")
        print $1, "has Bash access"
}' /etc/passwd
```

This displays users whose configured login shell is `/bin/bash`.

#### Working with Loops

`awk` supports several types of loops for repeating operations.

**1. For Loop**

A traditional `for` loop is useful when you know how many iterations are required.

```bash
awk 'BEGIN {
    for (i = 1; i <= 5; i++)
        print i
}'
```

Output:

```text
1
2
3
4
5
```

**2. While Loop**

A `while` loop continues while a condition remains true.

```bash
awk 'BEGIN {
    i = 1

    while (i <= 5) {
        print i
        i++
    }
}'
```

**3. For-In Loop**

The `for-in` loop is used to iterate over the elements of an associative array.

```bash
awk 'BEGIN {
    users["alice"] = 5000
    users["bob"] = 4500
    users["charlie"] = 6000

    for (name in users)
        print name, users[name]
}'
```

The order of elements in an associative array is not guaranteed unless explicitly controlled.

Loops are useful when processing arrays, counting occurrences, and performing repeated calculations.

#### Modifying Fields and Records

`awk` allows you to modify fields and rebuild records.

For example, you can change a field value:

```bash
awk '{$3 = 7000; print}' employees.txt
```

This changes the third field of every record to `7000` in the output.

You can also update fields conditionally:

```bash
awk '{
    if ($2 == "Developer")
        $3 = $3 + 500

    print
}' employees.txt
```

This increases the salary of developers by `500`.

To change the output field separator, assign a value to `OFS`:

```bash
awk 'BEGIN { OFS = ":" } { print $1, $2, $3 }' employees.txt
```

The output becomes colon-separated:

```text
Alice:Developer:5000
Bob:Administrator:4500
Charlie:Engineer:6000
David:Designer:4000
```

**Important:** When a field is modified, `awk` rebuilds the record. The original spacing between fields may not be preserved.

#### Formatting Output with printf

The `printf` statement provides precise control over output formatting.

Unlike `print`, `printf` does not automatically append a newline.

**Basic Syntax:**

```awk
printf "format", value1, value2
```

Common format specifiers include:

| Specifier | Description |
|---|---|
| `%s` | String |
| `%d` | Integer |
| `%f` | Floating-point number |
| `%.2f` | Floating-point number with two decimal places |
| `%c` | Character |
| `%x` | Hexadecimal number |
| `%o` | Octal number |
| `%%` | Literal percent sign |

**Example 1 — Basic formatting:**

```bash
awk '{ printf "Name: %s, Salary: %d\n", $1, $3 }' employees.txt
```

**Example 2 — Display decimal values:**

```bash
awk 'BEGIN {
    salary = 5234.567
    printf "Salary: %.2f\n", salary
}'
```

Output:

```text
Salary: 5234.57
```

**Example 3 — Create a formatted table:**

```bash
awk '
BEGIN {
    printf "%-15s %-20s %10s\n", "Name", "Role", "Salary"
    printf "%-15s %-20s %10s\n", "---------------", "--------------------", "----------"
}
{
    printf "%-15s %-20s %10d\n", $1, $2, $3
}
' employees.txt
```

The formatting options allow you to align columns and generate readable reports.

#### Combining awk with Other Commands

`awk` becomes even more powerful when combined with other Linux utilities.

**Example 1 — Extract IP addresses from command output:**

```bash
ip -4 addr show | awk '/inet / { print $2 }'
```

Displays IPv4 addresses with their network prefixes.

**Example 2 — Display usernames from `/etc/passwd`:**

```bash
awk -F: '{ print $1 }' /etc/passwd
```

**Example 3 — Display usernames and login shells:**

```bash
awk -F: '{ print $1, $7 }' /etc/passwd
```

**Example 4 — Count processes by user:**

```bash
ps -eo user= | awk '{ count[$1]++ } END {
    for (user in count)
        print user, count[user]
}'
```

This counts the number of processes associated with each user.

**Example 5 — Calculate total disk usage:**

```bash
df -h | awk 'NR > 1 { print $1, $5 }'
```

Displays filesystem names and their reported usage percentages.

**Example 6 — Filter processes by memory usage:**

```bash
ps aux | awk 'NR == 1 || $4 > 10'
```

Displays the header and processes whose memory usage percentage is greater than `10`.

#### Practical Examples

**Example 1 — Generate an Employee Report**

Suppose `employees.txt` contains:

```text
Alice Developer 5000
Bob Administrator 4500
Charlie Engineer 6000
David Designer 4000
```

Generate a report with employee names and salaries:

```bash
awk '
BEGIN {
    print "Employee Salary Report"
    print "----------------------"
}
{
    printf "%-15s %10d\n", $1, $3
    total += $3
    count++
}
END {
    print "----------------------"
    printf "Total Employees: %d\n", count
    printf "Total Salary: %d\n", total

    if (count > 0)
        printf "Average Salary: %.2f\n", total / count
}
' employees.txt
```

**Example 2 — Analyze Authentication Records**

Display usernames and login shells:

```bash
awk -F: '{ printf "%-20s %s\n", $1, $7 }' /etc/passwd
```

Find users configured with a Bash login shell:

```bash
awk -F: '$7 == "/bin/bash" { print $1 }' /etc/passwd
```

**Example 3 — Find Large Files**

Display files larger than 100 MB using `du`:

```bash
du -ah /var/log 2>/dev/null | awk '$1 ~ /M$/ || $1 ~ /G$/'
```

This is only a rough filter based on human-readable size suffixes. It does not reliably compare actual sizes because `du -h` produces human-readable values. For accurate numeric filtering, use `du -k`:

```bash
du -ak /var/log 2>/dev/null | awk '$1 > 102400'
```

This displays entries larger than approximately 100 MiB, based on the reported size in KiB.

**Example 4 — Count Log Entries by Severity**

Suppose `application.log` contains:

```text
INFO Application started
ERROR Database connection failed
WARNING High memory usage
INFO User logged in
ERROR Request failed
```

Count occurrences of each severity:

```bash
awk '{ count[$1]++ } END {
    for (level in count)
        print level, count[level]
}' application.log
```

This produces a summary of the number of records associated with each severity level.

**Example 5 — Find Users with UID Greater Than 1000**

```bash
awk -F: '$3 > 1000 { print $1, $3 }' /etc/passwd
```

This displays usernames and UIDs greater than `1000`.

The meaning of UID ranges depends on the distribution and local account configuration, so this is a filtering example rather than a universal definition of regular users.

**Example 6 — Extract and Sort Selected Fields**

```bash
awk '{ print $2, $3 }' employees.txt | sort
```

Extracts employee roles and salaries, then sorts the resulting output.

**Example 7 — Count Lines Containing a Specific Pattern**

```bash
awk '/error/ { count++ } END { print count+0 }' application.log
```

Counts records containing `error`.

The `count+0` expression ensures that the result is printed as zero when no matching records exist.

#### Common awk Use Cases

`awk` is frequently used in Linux administration, automation, and data analysis.

| Use Case | Example |
|---|---|
| Extract usernames | `awk -F: '{print $1}' /etc/passwd` |
| Extract IP information | `ip addr \| awk '/inet / {print $2}'` |
| Filter numeric values | `awk '$3 > 5000' file.txt` |
| Calculate totals | `awk '{sum += $3} END {print sum}' file.txt` |
| Calculate averages | `awk '{sum += $3; n++} END {if(n) print sum/n}' file.txt` |
| Count records | `awk 'END {print NR}' file.txt` |
| Count occurrences | `awk '{count[$1]++} END {for (x in count) print x, count[x]}' file.txt` |
| Format reports | `awk '{printf "%-15s %10d\n", $1, $3}' file.txt` |
| Filter log entries | `awk '/error/ {print}' application.log` |
| Process delimited files | `awk -F: '{print $1, $7}' /etc/passwd` |

#### Understanding the Difference Between grep and awk

Although both tools can search and filter text, their primary purposes are different.

| Feature | grep | awk |
|---|---|---|
| Primary purpose | Searching for text patterns | Processing structured text |
| Pattern matching | Yes | Yes |
| Regular expressions | Yes | Yes |
| Extracting columns | Limited | Yes |
| Mathematical calculations | No built-in arithmetic language | Yes |
| Conditional statements | Limited to search behavior | Yes |
| Processing records and fields | No dedicated field-processing model | Yes |
| Formatting reports | Limited | Yes |
| Programming capabilities | Limited | Extensive |

**When to use grep:**

Use `grep` when you need to find matching lines quickly.

```bash
grep -i "error" application.log
```

**When to use awk:**

Use `awk` when you need to extract fields, filter records based on values, or perform calculations.

```bash
awk '$3 > 5000 { print $1, $3 }' employees.txt
```

**Using both together:**

```bash
grep -i "error" application.log | awk '{ print $1, $2, $NF }'
```

This first filters lines containing `error`, then extracts selected fields from the matching records.

---

### Summary

In this section, we explored two essential Linux text-processing utilities: `grep` and `awk`.

| Command | Main Purpose |
|---|---|
| `grep` | Search for text patterns in files and input streams. |
| `grep -i` | Perform case-insensitive searches. |
| `grep -r` | Search directories recursively. |
| `grep -n` | Display matching line numbers. |
| `grep -v` | Exclude matching lines. |
| `grep -E` | Search using Extended Regular Expressions. |
| `grep -A`, `-B`, `-C` | Display context around matches. |
| `awk` | Process text records and fields. |
| `awk -F` | Specify a custom field separator. |
| `awk '{print $1}'` | Extract the first field. |
| `awk 'NR > 1'` | Skip the first record. |
| `awk 'BEGIN {...} END {...}'` | Execute initialization and final processing instructions. |
| `awk '{sum += $1}'` | Perform calculations on fields. |
| `awk 'printf'` | Format output. |

Understanding these tools makes it easier to investigate system logs, inspect configuration files, extract information, and automate common Linux administration tasks.

### Additional Resources

- [GNU grep Manual](https://www.gnu.org/software/grep/manual/grep.html)
- [GNU awk User's Guide](https://www.gnu.org/software/gawk/manual/gawk.html)
- [GNU awk Documentation](https://www.gnu.org/software/gawk/)
- [Regular Expressions — GNU grep](https://www.gnu.org/software/grep/manual/html_node/Regular-Expressions.html)
- [The Linux man-pages Project](https://man7.org/linux/man-pages/)

---


## Practical Exercises

The following exercises are designed to reinforce the concepts and commands introduced throughout Day 05. They focus on standard streams, input and output redirection, text-processing utilities, pipelines, text searching, and data analysis using `grep` and `awk`.

### Exercise 1 — Understanding Standard Streams

**Objective:** Understand standard input, standard output, and standard error.

1. Display a simple message using `echo`.
2. Display the contents of `/etc/passwd`.
3. Run a command that produces an error by attempting to list a nonexistent directory.
4. Redirect standard output to a file named `output.txt`.
5. Redirect standard error to a file named `errors.txt`.
6. Redirect both standard output and standard error to a single file.
7. Append command output to an existing file without overwriting its contents.
8. Redirect standard output to `/dev/null` while allowing error messages to remain visible.
9. Use input redirection to provide the contents of a file to a command.
10. Create a small text file using a here-document.

**Challenge:**

Run a command that produces both normal output and error messages. Save standard output and standard error into two separate files.

### Exercise 2 — Working with Text Processing Commands

**Objective:** Practice extracting, sorting, combining, and modifying text.

1. Create a file named `employees.txt` containing the following data:

   ```text
   Alice Developer 5000
   Bob Administrator 4500
   Charlie Engineer 6000
   David Designer 4000
   Emma Developer 5500
   ```

2. Use `cut` to extract the first field from a colon-separated file.
3. Use `paste` to combine two text files side by side.
4. Use `sort` to sort employee names alphabetically.
5. Use `sort -n` to sort a file containing numeric values.
6. Use `sort -r` to reverse the sorting order.
7. Use `tr` to convert lowercase letters to uppercase.
8. Use `tr` to remove all numeric characters from a text file.
9. Use `head` to display the first three lines of `employees.txt`.
10. Use `tail` to display the last two lines.
11. Use `tail -f` to monitor a log file while another terminal appends new entries.
12. Use `join` to combine two sorted files using a common field.
13. Use `split` to divide a large text file into smaller files.
14. Use `uniq` to identify repeated adjacent lines.
15. Use `uniq -c` to count consecutive duplicate lines after sorting.
16. Use `wc -l` to count the number of lines in a file.
17. Use `wc -w` to count the number of words.
18. Use `nl` to display a file with numbered lines.
19. Use `expand` to convert tabs into spaces.
20. Use `unexpand` to convert appropriate leading spaces back into tabs.

**Challenge:**

Create a pipeline that sorts a list of names, removes consecutive duplicates, and displays the total number of unique names.

### Exercise 3 — Working with Pipes and tee

**Objective:** Combine commands and save intermediate results.

1. Display the contents of a text file using `cat` and pipe the output to `grep`.
2. Use a pipeline to search for lines containing `error` in a log file.
3. Combine `grep`, `sort`, and `uniq` to identify frequently occurring error messages.
4. Use `wc -l` to count matching records in a pipeline.
5. Use `head` to display only the first five results of a command.
6. Use `tee` to display command output while saving it to a file.
7. Use `tee -a` to append output to an existing file.
8. Use `tee` to save the output of an intermediate command in a pipeline.
9. Redirect the output of `tee` to `/dev/null` when only the saved file is needed.
10. Explain why `sudo echo "text" > /etc/example.conf` may fail and use `sudo tee` to write to a protected file.

**Challenge:**

Create a pipeline that searches a log file for lines containing `error`, displays the matching lines, and saves the same results into `errors.txt`.

### Exercise 4 — Searching Text with grep

**Objective:** Develop practical text-searching skills using `grep`.

1. Create a file named `application.log` containing several informational messages, warnings, and errors.
2. Search for all lines containing the word `error`.
3. Perform a case-insensitive search for `warning`.
4. Display matching lines with their line numbers.
5. Search for a complete word using `-w`.
6. Search for an exact line using `-x`.
7. Display lines that do not contain `error`.
8. Count the number of lines containing `warning`.
9. Display only the matching portions of lines using `-o`.
10. Search for multiple patterns using `-E`.
11. Use `-A` to display lines after a match.
12. Use `-B` to display lines before a match.
13. Use `-C` to display context around a match.
14. Search recursively for a configuration setting inside `/etc/`.
15. Search only files ending in `.conf`.
16. Exclude files ending in `.log` from a recursive search.
17. Use `-f` to search for multiple patterns stored in a separate file.
18. Use `-F` to search for a literal string containing special characters.
19. Use `-q` inside an `if` statement to check whether a pattern exists.

**Challenge:**

Search an application log for lines containing `error`, regardless of capitalization. Display their line numbers and three surrounding lines of context.

### Exercise 5 — Processing Text with awk

**Objective:** Learn how to extract fields, filter records, and perform calculations.

1. Create an `employees.txt` file using the employee data from Exercise 2.
2. Use `awk` to display the entire contents of the file.
3. Display only the first field of each record.
4. Display the first and third fields.
5. Display the last field using `$NF`.
6. Display the number of fields in each record using `NF`.
7. Display record numbers using `NR`.
8. Display employees whose salaries are greater than `5000`.
9. Display employees whose roles are `Developer`.
10. Use logical operators to find employees whose salaries are between `4000` and `5500`.
11. Use regular expressions to find employee names beginning with an uppercase letter.
12. Use `BEGIN` to display a report title.
13. Use `END` to display a completion message.
14. Calculate the total salary of all employees.
15. Calculate the average salary.
16. Find the employee with the highest salary.
17. Find the employee with the lowest salary.
18. Use `toupper()` to display employee names in uppercase.
19. Use `length()` to display the length of each employee name.
20. Use `printf` to create a formatted employee table.

**Challenge:**

Create an employee salary report that includes:

- Employee name.
- Employee role.
- Individual salary.
- Total number of employees.
- Total salary.
- Average salary.
- Highest salary.
- Lowest salary.

Use `BEGIN`, regular processing blocks, and `END` to organize your program.

### Exercise 6 — Working with System Files

**Objective:** Apply text-processing commands to real Linux system files.

1. Display all usernames from `/etc/passwd` using `awk`.
2. Display usernames and their home directories.
3. Display usernames and their configured login shells.
4. Find accounts configured with `/bin/bash` as their login shell.
5. Count the number of account entries in `/etc/passwd`.
6. Search `/var/log/` for lines containing `error`.
7. Perform a case-insensitive search for `failed` in available authentication logs.
8. Use `grep -n` to display matching line numbers.
9. Use `awk` to extract filesystem names and usage percentages from `df -h`.
10. Use `ps` and `awk` to display process names and memory usage.
11. Count processes belonging to each user.
12. Use `grep -v` to exclude unwanted log entries.

**Note:** System log filenames, permissions, and available commands may differ between Linux distributions. Some exercises may require `sudo` privileges.

### Exercise 7 — Advanced Text Processing Pipeline

**Objective:** Combine multiple commands to solve practical text-processing problems.

1. Create a file named `access.log` containing sample web server log entries.
2. Use `grep` to identify lines containing HTTP status code `404`.
3. Use `awk` to extract the IP address from each matching entry.
4. Use `sort` to organize the extracted IP addresses.
5. Use `uniq -c` to count requests from each IP address.
6. Sort the results numerically in descending order.
7. Display the five IP addresses with the highest number of failed requests.
8. Use `tee` to save the results into `top_ips.txt`.
9. Use `wc -l` to count the number of unique IP addresses in the report.
10. Use `awk` to calculate the total number of matching requests.

**Challenge:**

Build a complete pipeline that identifies the most active IP addresses generating HTTP 404 responses and produces a sorted report.

### Exercise 8 — Final Challenge: Linux Log Analyzer

**Objective:** Build a practical log-analysis workflow using the commands introduced throughout Day 05.

Create a sample file named `system.log` containing entries similar to the following:

```text
2026-11-22 08:15:10 INFO System started
2026-11-22 08:17:22 WARNING High memory usage
2026-11-22 08:20:15 ERROR Database connection failed
2026-11-22 08:22:40 INFO User logged in
2026-11-22 08:25:10 ERROR Disk operation failed
2026-11-22 08:27:55 WARNING CPU usage is high
2026-11-22 08:30:12 INFO Backup completed
2026-11-22 08:35:40 ERROR Application crashed
2026-11-22 08:40:15 INFO Service restarted
2026-11-22 08:45:20 WARNING Network latency detected
```

Complete the following tasks:

1. Display the first five entries using `head`.
2. Display the last three entries using `tail`.
3. Search for all `ERROR` entries using `grep`.
4. Perform a case-insensitive search for `warning`.
5. Display matching entries with their line numbers.
6. Count the total number of error entries.
7. Count the total number of warning entries.
8. Use `awk` to extract the date, time, and severity level.
9. Use `awk` to count how many entries belong to each severity level.
10. Calculate the total number of log entries.
11. Use `awk` to display only `ERROR` entries.
12. Use `sort` and `uniq` to identify repeated severity levels.
13. Create a report containing all error messages.
14. Use `tee` to display the report and save it into `error-report.txt`.
15. Redirect standard output and standard error separately when running a command that may encounter errors.

**Final Challenge:**

Create a complete log-analysis report with the following information:

- Total number of log entries.
- Number of informational messages.
- Number of warning messages.
- Number of error messages.
- All error messages with their timestamps.
- The most frequently occurring severity level.
- A summary file containing the extracted results.

Your solution should combine `grep`, `awk`, `sort`, `uniq`, `wc`, pipes, redirection, and `tee`.

---

**Goal:** Develop practical skills in managing standard input, output, and error streams, processing and filtering text, building command pipelines, searching with regular expressions, and analyzing structured data using grep and awk in Linux.

<p align="right">
  <a href="../Day-04/README.md">← Day 04</a>
  &nbsp; | &nbsp;
  <a href="../Day-06/">Day 06 →</a>
  &nbsp; | &nbsp;
  <a href="#day-05--text-processing">Back to top ↑</a>
</p>
