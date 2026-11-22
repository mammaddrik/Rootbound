
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
