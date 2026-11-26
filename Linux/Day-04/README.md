<p align="center">
  <a href="https://github.com/mammaddrik/RootBound">
    <img src="https://i.postimg.cc/xd2pd19m/Root-Bound-Day-04.png" alt="RootBound-Day-04">
  </a>
</p>

# Day 04 — Working with Files

## Table of Contents

- [Day 04 — Working with Files](#day-04--working-with-files)
  - [Table of Contents](#table-of-contents)
  - [File Permissions](#file-permissions)
    - [Understanding File Permissions](#understanding-file-permissions)
      - [User, Group, and Others](#user-group-and-others)
      - [Types of Permissions](#types-of-permissions)
      - [Permission Combinations](#permission-combinations)
    - [Viewing File Permissions](#viewing-file-permissions)
      - [Using ls -l](#using-ls--l)
      - [Understanding the Permission String](#understanding-the-permission-string)
      - [Common File Type Indicators](#common-file-type-indicators)
      - [Viewing Hidden Files and Permissions](#viewing-hidden-files-and-permissions)
      - [Using stat](#using-stat)
      - [Checking Directory Permissions](#checking-directory-permissions)
    - [Changing File Permissions](#changing-file-permissions)
      - [Numeric Method](#numeric-method)
      - [Common Permission Values](#common-permission-values)
      - [Symbolic Method](#symbolic-method)
      - [Recursive Permission Changes](#recursive-permission-changes)
      - [Important Notes](#important-notes)
    - [Changing File Ownership](#changing-file-ownership)
      - [Using chown](#using-chown)
      - [Changing Ownership Recursively](#changing-ownership-recursively)
      - [Using chgrp](#using-chgrp)
      - [Viewing Ownership Information](#viewing-ownership-information)
      - [Comparing chown and chgrp](#comparing-chown-and-chgrp)
    - [Special Permissions](#special-permissions)
      - [Set User ID (SUID)](#set-user-id-suid)
      - [Set Group ID (SGID)](#set-group-id-sgid)
      - [Sticky Bit](#sticky-bit)
      - [Comparing Special Permissions](#comparing-special-permissions)
      - [Using Special Permissions Together](#using-special-permissions-together)
    - [Default Permissions and umask](#default-permissions-and-umask)
      - [Understanding Default Permissions](#understanding-default-permissions)
      - [Understanding umask](#understanding-umask)
      - [Common umask Values](#common-umask-values)
      - [Changing umask](#changing-umask)
      - [Symbolic umask](#symbolic-umask)
      - [Making umask Changes Permanent](#making-umask-changes-permanent)
      - [Important Notes](#important-notes-1)
  - [Archiving and Compressing](#archiving-and-compressing)
    - [Understanding Archiving and Compression](#understanding-archiving-and-compression)
      - [Archiving vs. Compression](#archiving-vs-compression)
      - [Common Archive and Compression Formats](#common-archive-and-compression-formats)
    - [Working with tar](#working-with-tar)
      - [Basic Syntax](#basic-syntax)
      - [Common tar Options](#common-tar-options)
      - [Creating an Archive](#creating-an-archive)
      - [Listing Archive Contents](#listing-archive-contents)
      - [Extracting an Archive](#extracting-an-archive)
      - [Excluding Files from an Archive](#excluding-files-from-an-archive)
      - [Extracting Specific Files](#extracting-specific-files)
      - [Working with Different Directories](#working-with-different-directories)
      - [Preserving Permissions and Ownership](#preserving-permissions-and-ownership)
    - [Working with gzip](#working-with-gzip)
      - [Compressing Files](#compressing-files)
      - [Keeping the Original File](#keeping-the-original-file)
      - [Decompressing Files](#decompressing-files)
      - [Compression Levels](#compression-levels)
      - [Displaying Compression Information](#displaying-compression-information)
      - [Creating gzip-compressed TAR Archives](#creating-gzip-compressed-tar-archives)
      - [Summary](#summary)
    - [Working with bzip2](#working-with-bzip2)
      - [Compressing Files](#compressing-files-1)
      - [Keeping the Original File](#keeping-the-original-file-1)
      - [Decompressing Files](#decompressing-files-1)
      - [Compression Levels](#compression-levels-1)
      - [Creating bzip2-compressed TAR Archives](#creating-bzip2-compressed-tar-archives)
      - [Summary](#summary-1)
    - [Working with xz](#working-with-xz)
      - [Compressing Files](#compressing-files-2)
      - [Keeping the Original File](#keeping-the-original-file-2)
      - [Decompressing Files](#decompressing-files-2)
      - [Compression Levels](#compression-levels-2)
      - [Creating xz-compressed TAR Archives](#creating-xz-compressed-tar-archives)
      - [Summary](#summary-2)
    - [Working with zip and unzip](#working-with-zip-and-unzip)
      - [Creating ZIP Archives](#creating-zip-archives)
      - [Listing ZIP Archive Contents](#listing-zip-archive-contents)
      - [Extracting ZIP Archives](#extracting-zip-archives)
      - [Extracting Specific Files](#extracting-specific-files-1)
      - [Updating ZIP Archives](#updating-zip-archives)
      - [Removing Files from ZIP Archives](#removing-files-from-zip-archives)
      - [Testing ZIP Archive Integrity](#testing-zip-archive-integrity)
      - [Common ZIP Options](#common-zip-options)
      - [Summary](#summary-3)
  - [Copying and Renaming](#copying-and-renaming)
    - [Copying Files with cp](#copying-files-with-cp)
      - [Basic Syntax](#basic-syntax-1)
      - [Copying a File](#copying-a-file)
      - [Copying a File into Another Directory](#copying-a-file-into-another-directory)
      - [Copying Multiple Files](#copying-multiple-files)
      - [Copying Directories](#copying-directories)
      - [Preserving File Attributes](#preserving-file-attributes)
      - [Common cp Options](#common-cp-options)
      - [Interactive Copying](#interactive-copying)
      - [Verbose Copying](#verbose-copying)
      - [Copying Only Newer Files](#copying-only-newer-files)
      - [Summary](#summary-4)
    - [Moving and Renaming Files with mv](#moving-and-renaming-files-with-mv)
      - [Basic Syntax](#basic-syntax-2)
      - [Renaming a File](#renaming-a-file)
      - [Moving a File into Another Directory](#moving-a-file-into-another-directory)
      - [Moving and Renaming a File](#moving-and-renaming-a-file)
      - [Moving Multiple Files](#moving-multiple-files)
      - [Moving Directories](#moving-directories)
      - [Common mv Options](#common-mv-options)
      - [Interactive Moving](#interactive-moving)
      - [Verbose Moving](#verbose-moving)
      - [Moving Without Overwriting](#moving-without-overwriting)
      - [Understanding How mv Works](#understanding-how-mv-works)
      - [Summary](#summary-5)
    - [Working with install](#working-with-install)
      - [Basic Syntax](#basic-syntax-3)
      - [Installing a File](#installing-a-file)
      - [Setting File Permissions](#setting-file-permissions)
      - [Setting Ownership](#setting-ownership)
      - [Creating Directories](#creating-directories)
      - [Creating Multiple Directories](#creating-multiple-directories)
      - [Installing Files into a Directory](#installing-files-into-a-directory)
      - [Common install Options](#common-install-options)
      - [Comparing cp, mv, and install](#comparing-cp-mv-and-install)
      - [Practical Example](#practical-example)
      - [Summary](#summary-6)
  - [Symbolic Links and Hard Links](#symbolic-links-and-hard-links)
    - [Understanding Inodes](#understanding-inodes)
    - [Hard Links](#hard-links)
    - [Symbolic Links](#symbolic-links)
    - [Comparing Hard Links and Symbolic Links](#comparing-hard-links-and-symbolic-links)
    - [Managing and Removing Links](#managing-and-removing-links)
  - [Practical Exercises](#practical-exercises)
    - [Exercise 1 — Understanding File Permissions](#exercise-1--understanding-file-permissions)
    - [Exercise 2 — Ownership and Special Permissions](#exercise-2--ownership-and-special-permissions)
    - [Exercise 3 — Understanding umask](#exercise-3--understanding-umask)
    - [Exercise 4 — Creating and Managing Archives](#exercise-4--creating-and-managing-archives)
    - [Exercise 5 — Working with Compression Tools](#exercise-5--working-with-compression-tools)
    - [Exercise 6 — Copying and Moving Files](#exercise-6--copying-and-moving-files)
    - [Exercise 7 — Installing Files with install](#exercise-7--installing-files-with-install)
    - [Exercise 8 — Working with Hard Links](#exercise-8--working-with-hard-links)
    - [Exercise 9 — Working with Symbolic Links](#exercise-9--working-with-symbolic-links)
    - [Exercise 10 — Final Challenge: Linux File Management](#exercise-10--final-challenge-linux-file-management)

---


## File Permissions

### Understanding File Permissions

Linux is a multi-user operating system, which means multiple users can access the same system and work with shared files and directories. To protect data and control access, Linux uses a permission system.

File permissions determine which users can read, modify, or execute files and which users can access directories.

Every file and directory in Linux has an associated owner, a group, and a set of permissions that define how different users can interact with it.

#### User, Group, and Others

Linux divides users into three permission categories:

| Category | Symbol | Description |
|---|---|---|
| User | `u` | The owner of the file or directory |
| Group | `g` | Users who belong to the file's associated group |
| Others | `o` | All other users on the system |
| All | `a` | User, group, and others combined |

Each category can have different permissions. For example, the owner may have full access to a file, while members of the group can only read it.

#### Types of Permissions

Linux provides three basic permissions:

| Permission | Symbol | Numeric Value | Description |
|---|---|---:|---|
| Read | `r` | 4 | Allows reading file contents or listing directory entries |
| Write | `w` | 2 | Allows modifying a file or creating, deleting, and renaming directory entries |
| Execute | `x` | 1 | Allows executing a file or accessing a directory |

The meaning of these permissions depends on whether they are applied to a regular file or a directory.

**Permissions on Regular Files**

- Read (`r`): Allows users to view the contents of a file.
- Write (`w`): Allows users to modify the contents of a file.
- Execute (`x`): Allows users to execute a file as a program or script, provided it is executable and can be run by the system.

**Permissions on Directories**

- Read (`r`): Allows users to list the names of entries inside a directory.
- Write (`w`): Allows users to create, delete, and rename entries inside a directory.
- Execute (`x`): Allows users to enter or traverse a directory and access its entries, subject to permissions on the relevant paths and files.

For example, a user may have permission to read a directory but may not be able to access a particular file inside it.

Similarly, write permission on a directory does not automatically grant permission to modify the contents of every file inside it.

#### Permission Combinations

The three basic permissions can be combined to create different access levels.

| Permissions | Numeric Value | Meaning |
|---|---:|---|
| `---` | 0 | No permissions |
| `--x` | 1 | Execute only |
| `-w-` | 2 | Write only |
| `-wx` | 3 | Write and execute |
| `r--` | 4 | Read only |
| `r-x` | 5 | Read and execute |
| `rw-` | 6 | Read and write |
| `rwx` | 7 | Read, write, and execute |

Numeric values are calculated by adding the values of the individual permissions.

For example:

```text
rwx = 4 + 2 + 1 = 7
r-x = 4 + 0 + 1 = 5
rw- = 4 + 2 + 0 = 6
```

These values are commonly used with the `chmod` command to manage file and directory permissions.

---

### Viewing File Permissions

Linux provides several commands for inspecting file permissions, ownership, and other filesystem information.

#### Using ls -l

The `ls -l` command displays detailed information about files and directories, including their permissions, ownership, size, and modification date.

```bash
ls -l
```

Example output:

```text
-rwxr-xr-- 1 alice developers 1024 Nov 21 10:30 script.sh
-rw-r--r-- 1 alice developers  256 Nov 21 10:35 notes.txt
drwxr-xr-x 2 alice developers 4096 Nov 21 10:40 projects
```

The output contains several columns:

| Column | Description |
|---|---|
| File Type and Permissions | Indicates the file type and its access permissions |
| Link Count | Number of hard links associated with the file |
| Owner | User who owns the file |
| Group | Group associated with the file |
| Size | File size in bytes |
| Modification Date | Date and time of the last modification |
| Name | File or directory name |

The first column is particularly important because it contains the file type and permission information.

#### Understanding the Permission String

Consider the following example:

```text
-rwxr-xr--
```

The permission string contains ten characters:

```text
- rwx r-x r--
│  │   │   │
│  │   │   └── Others
│  │   └────── Group
│  └────────── Owner
└───────────── File Type
```

The first character represents the file type, while the remaining nine characters represent permissions.

In this example:

| Category | Permissions | Numeric Value |
|---|---|---:|
| Owner | `rwx` | 7 |
| Group | `r-x` | 5 |
| Others | `r--` | 4 |

Therefore, the numeric permission of this file is `754`.

#### Common File Type Indicators

The first character in the permission string identifies the type of filesystem object.

| Symbol | File Type | Description |
|---|---|---|
| `-` | Regular File | A normal file containing data |
| `d` | Directory | A directory containing filesystem entries |
| `l` | Symbolic Link | A link pointing to another path |
| `c` | Character Device | A device that transfers data character by character |
| `b` | Block Device | A device that transfers data in blocks |
| `p` | Named Pipe | A special file used for inter-process communication |
| `s` | Socket | A special file used for communication between processes |

For example:

```text
-rw-r--r--  regular file
drwxr-xr-x  directory
lrwxrwxrwx  symbolic link
```

#### Viewing Hidden Files and Permissions

The `-a` option displays hidden files, while `-l` provides detailed information.

```bash
ls -la
```

Hidden files in Linux typically have names beginning with a dot (`.`).

#### Using stat

The `stat` command provides detailed information about a file or directory, including its permissions, ownership, inode number, size, and timestamps.

```bash
stat file.txt
```

Example output:

```text
  File: file.txt
  Size: 256
  Access: (0644/-rw-r--r--)
  Uid: (1000/alice)
  Gid: (1001/developers)
```

To display only the permissions and ownership information:

```bash
stat -c '%A %a %U %G %n' file.txt
```

Output:

```text
-rw-r--r-- 644 alice developers file.txt
```

The format characters represent:

| Format | Description |
|---|---|
| `%A` | Permissions in symbolic format |
| `%a` | Permissions in octal format |
| `%U` | Owner username |
| `%G` | Group name |
| `%n` | File name |

#### Checking Directory Permissions

To display the permissions of a directory itself rather than its contents, use:

```bash
ls -ld /home/alice
```

The `-d` option tells `ls` to display information about the directory itself.

This is particularly useful when inspecting permissions on directories.

---

### Changing File Permissions

The `chmod` command is used to change the permissions of files and directories.

The name `chmod` stands for *change mode*.

General syntax:

```bash
chmod [options] mode file
```

Linux supports two main methods for changing permissions:

- Numeric (Octal) Method
- Symbolic Method

#### Numeric Method

The numeric method uses values from `0` to `7` to define permissions for the owner, group, and others.

Each permission value is calculated by adding the corresponding permission numbers:

| Permission | Value |
|---|---:|
| Read (`r`) | 4 |
| Write (`w`) | 2 |
| Execute (`x`) | 1 |

For example:

```bash
chmod 755 script.sh
```

This assigns the following permissions:

| Category | Permissions | Value |
|---|---|---:|
| Owner | `rwx` | 7 |
| Group | `r-x` | 5 |
| Others | `r-x` | 5 |

The resulting permission string is:

```text
-rwxr-xr-x
```

Another common example:

```bash
chmod 644 notes.txt
```

This assigns:

| Category | Permissions | Value |
|---|---|---:|
| Owner | `rw-` | 6 |
| Group | `r--` | 4 |
| Others | `r--` | 4 |

The resulting permission string is:

```text
-rw-r--r--
```

#### Common Permission Values

The following permission combinations are commonly used in Linux:

| Numeric Value | Symbolic Permissions | Common Usage |
|---|---|---|
| `644` | `rw-r--r--` | Regular files |
| `600` | `rw-------` | Private files |
| `664` | `rw-rw-r--` | Shared group files |
| `700` | `rwx------` | Private directories or scripts |
| `755` | `rwxr-xr-x` | Executable files and public directories |
| `750` | `rwxr-x---` | Directories accessible to owner and group |
| `775` | `rwxrwxr-x` | Shared group directories |
| `777` | `rwxrwxrwx` | Full permissions for everyone |

These are common configurations, not universal requirements. The appropriate permissions depend on the purpose of the file and the security requirements of the system.

#### Symbolic Method

The symbolic method uses letters and operators to modify permissions.

It is useful when you want to add or remove specific permissions without replacing the entire permission set.

The main permission categories are:

| Symbol | Category |
|---|---|
| `u` | Owner |
| `g` | Group |
| `o` | Others |
| `a` | All categories |

The available operators are:

| Operator | Description |
|---|---|
| `+` | Adds permissions |
| `-` | Removes permissions |
| `=` | Sets exact permissions |

Examples:

Add execute permission for the owner:

```bash
chmod u+x script.sh
```

Remove write permission from the group:

```bash
chmod g-w file.txt
```

Add read permission for others:

```bash
chmod o+r file.txt
```

Add read permission for everyone:

```bash
chmod a+r file.txt
```

Set exact permissions for each category:

```bash
chmod u=rw,g=r,o= file.txt
```

This gives the owner read and write permissions, the group read permission, and others no permissions.

You can also modify multiple categories in one command:

```bash
chmod u+x,g+x script.sh
```

#### Recursive Permission Changes

The `-R` option applies permission changes recursively to a directory and its contents.

For example:

```bash
chmod -R 755 project/
```

This applies `755` permissions to the directory and all files and subdirectories inside it.

However, applying the same permissions recursively can be inappropriate because regular files and directories usually require different permissions.

A safer approach is to set directory and file permissions separately:

```bash
find project/ -type d -exec chmod 755 {} +
find project/ -type f -exec chmod 644 {} +
```

The first command assigns `755` to directories, while the second assigns `644` to regular files.

#### Important Notes

- Only the file owner or a privileged user can normally change file permissions.
- Execute permission does not guarantee that a file can run; the file must also contain a valid executable format or script.
- A directory generally needs execute permission to be traversed.
- Write permission on a directory controls the creation, deletion, and renaming of its entries.
- Be careful when using `chmod -R`, especially on system directories.

---

### Changing File Ownership

Linux associates every file and directory with an owner and a group.

Ownership determines which user and group are associated with a filesystem object and therefore which permission class applies to a particular user.

Linux provides two main commands for changing ownership:

- `chown` — Changes the owner and optionally the group.
- `chgrp` — Changes the group.

#### Using chown

The `chown` command changes the owner of a file or directory.

General syntax:

```bash
chown [options] owner[:group] file
```

Change the owner of a file:

```bash
sudo chown alice file.txt
```

Change both the owner and group:

```bash
sudo chown alice:developers file.txt
```

Change only the group:

```bash
sudo chown :developers file.txt
```

In these examples, `alice` is the username and `developers` is the group name.

#### Changing Ownership Recursively

The `-R` option applies ownership changes recursively to a directory and its contents.

```bash
sudo chown -R alice:developers project/
```

This changes the owner to `alice` and the group to `developers` for the directory and its contents.

Recursive ownership changes should be used carefully, especially when working with system directories.

#### Using chgrp

The `chgrp` command changes the group associated with a file or directory.

General syntax:

```bash
chgrp [options] group file
```

Change the group of a file:

```bash
sudo chgrp developers file.txt
```

Change the group recursively:

```bash
sudo chgrp -R developers project/
```

A regular user can generally change a file's group to one of the groups they belong to, while changing it to another group normally requires elevated privileges.

#### Viewing Ownership Information

Use `ls -l` to display the owner and group of a file:

```bash
ls -l file.txt
```

Example output:

```text
-rw-r--r-- 1 alice developers 256 Nov 21 10:30 file.txt
```

You can also use:

```bash
stat file.txt
```

This provides additional ownership and filesystem information.

#### Comparing chown and chgrp

| Command | Purpose |
|---|---|
| `chown` | Changes the file owner and optionally the group |
| `chgrp` | Changes the file's group |

For example:

```bash
sudo chown bob file.txt
sudo chgrp developers file.txt
```

The first command changes the owner to `bob`, while the second changes the group to `developers`.

---

### Special Permissions

In addition to the standard read, write, and execute permissions, Linux provides three special permission bits:

- Set User ID (SUID)
- Set Group ID (SGID)
- Sticky Bit

These permissions provide additional control over how files and directories are accessed and executed.

#### Set User ID (SUID)

SUID is a special permission that can be applied to executable files.

When an executable file has SUID enabled, it runs with the effective user ID of the file's owner rather than the user who started it.

For example, the `passwd` command commonly uses SUID because changing a user's password requires modifying protected system files.

Inspect its permissions:

```bash
ls -l /usr/bin/passwd
```

On many Linux distributions, the output includes:

```text
-rwsr-xr-x
```

The `s` in the owner's execute position indicates that SUID is enabled.

Enable SUID using numeric notation:

```bash
sudo chmod 4755 program
```

Enable it using symbolic notation:

```bash
sudo chmod u+s program
```

Remove SUID:

```bash
sudo chmod u-s program
```

The numeric value of SUID is `4000`.

If SUID is enabled without the corresponding execute permission, it appears as an uppercase `S` instead of lowercase `s`.

**Important:** SUID on interpreted scripts is generally ignored by Linux for security reasons. SUID should be used cautiously because it can introduce serious security risks.

#### Set Group ID (SGID)

SGID behaves differently depending on whether it is applied to an executable file or a directory.

**On Executable Files**

When SGID is enabled on an executable file, the program runs with the effective group ID of the file's group.

**On Directories**

When SGID is enabled on a directory, newly created files and subdirectories generally inherit the directory's group rather than the creating user's primary group.

This is particularly useful for shared directories where multiple users need to work with files belonging to the same group.

Example:

```bash
sudo chmod 2775 shared/
```

Or using symbolic notation:

```bash
sudo chmod g+s shared/
```

The permission string may look like:

```text
drwxrwsr-x
```

The `s` in the group's execute position indicates that SGID is enabled.

Remove SGID:

```bash
sudo chmod g-s shared/
```

The numeric value of SGID is `2000`.

#### Sticky Bit

The sticky bit is primarily used on shared directories.

When applied to a directory, it restricts users from deleting or renaming files belonging to other users, even when they have write permission on that directory.

A common example is `/tmp`, where multiple users can create temporary files.

Inspect its permissions:

```bash
ls -ld /tmp
```

Typical output:

```text
drwxrwxrwt
```

The `t` in the others' execute position indicates that the sticky bit is enabled.

Enable the sticky bit:

```bash
sudo chmod 1777 shared/
```

Or use symbolic notation:

```bash
sudo chmod +t shared/
```

Remove it:

```bash
sudo chmod -t shared/
```

The numeric value of the sticky bit is `1000`.

If the sticky bit is enabled without the corresponding execute permission, it appears as an uppercase `T`.

#### Comparing Special Permissions

| Special Permission | Numeric Value | Applied To | Main Purpose |
|---|---:|---|---|
| SUID | 4000 | Executable files | Runs with the file owner's effective user ID |
| SGID | 2000 | Executable files and directories | Uses the file's group ID or enables group inheritance |
| Sticky Bit | 1000 | Directories | Restricts deletion and renaming of entries by other users |

#### Using Special Permissions Together

Special permission bits can be combined with standard permissions.

For example:

```bash
sudo chmod 4755 program
sudo chmod 2775 shared/
sudo chmod 1777 shared-temp/
```

The first command enables SUID, the second enables SGID, and the third enables the sticky bit.

You can also combine special bits in a single numeric mode:

```bash
chmod 7755 program
```

In this example, the leading digit `7` represents all three special bits:

```text
4 + 2 + 1 = 7
```

---

### Default Permissions and umask

When a new file or directory is created, Linux assigns it an initial permission mode.

The `umask` command controls which permission bits are removed from the permissions requested during file and directory creation.

Understanding `umask` is important because it helps prevent newly created files from receiving unnecessarily permissive access settings.

#### Understanding Default Permissions

By default, applications commonly request the following permission modes:

| Object Type | Base Permissions | Numeric Value |
|---|---|---:|
| Regular Files | `rw-rw-rw-` | 666 |
| Directories | `rwxrwxrwx` | 777 |

Regular files do not normally receive execute permissions by default.

The final permissions are determined by applying the current `umask` to these requested permissions.

#### Understanding umask

The `umask` value specifies which permission bits should be removed.

For example, a common default umask is:

```bash
umask
```

Output:

```text
0022
```

The leading zero indicates that no special permission bits are masked in this example. The remaining digits represent the owner, group, and others.

With a umask of `022`:

**For Regular Files**

```text
Base permissions: 666
Umask:            022
Final permissions: 644
```

Result:

```text
-rw-r--r--
```

**For Directories**

```text
Base permissions: 777
Umask:            022
Final permissions: 755
```

Result:

```text
drwxr-xr-x
```

The calculation is based on permission bits: any bit enabled in the umask is removed from the requested mode.

#### Common umask Values

| umask | Regular Files | Directories | Description |
|---|---|---|---|
| `000` | `666` | `777` | No permissions are masked |
| `002` | `664` | `775` | Group members can write |
| `022` | `644` | `755` | Common default configuration |
| `027` | `640` | `750` | Restricts access for others |
| `077` | `600` | `700` | Restricts access to the owner |

These are typical results when applications request the standard base modes and no default ACL changes the outcome.

#### Changing umask

You can temporarily change the umask for the current shell.

For example:

```bash
umask 027
```

Check the new value:

```bash
umask
```

Create a file and directory to inspect their permissions:

```bash
touch private.txt
mkdir private-dir
```

Display their permissions:

```bash
ls -ld private.txt private-dir
```

Expected permissions under the usual creation modes:

```text
-rw-r----- private.txt
drwxr-x--- private-dir
```

The new umask affects files and directories created by the current shell and its child processes.

#### Symbolic umask

You can also display the umask in symbolic format:

```bash
umask -S
```

Example output:

```text
u=rwx,g=rx,o=rx
```

This represents the permissions that are not masked.

You can also set a symbolic umask:

```bash
umask u=rwx,g=rx,o=
```

This removes all permissions for others from the default creation modes.

#### Making umask Changes Permanent

A umask change made directly in the terminal is temporary and normally lasts only for the current shell session.

To make it persistent, add the command to an appropriate shell configuration file.

For example, for Bash:

```bash
nano ~/.bashrc
```

Add:

```bash
umask 027
```

Save the file and reload the configuration:

```bash
source ~/.bashrc
```

The appropriate configuration file depends on the shell and whether the session is interactive or a login session. System-wide defaults may also be configured through distribution-specific files.

#### Important Notes

- `umask` removes permissions; it does not grant additional permissions.
- Regular files normally start with requested permissions of `666`.
- Directories normally start with requested permissions of `777`.
- A more restrictive umask results in fewer default permissions.
- A process inherits its umask from its parent process unless it changes it.
- Default Access Control Lists (ACLs), when configured, can also influence the final permissions of newly created files and directories.

---


## Archiving and Compressing

### Understanding Archiving and Compression

Archiving and compression are common operations in Linux used to organize files, reduce storage usage, create backups, and transfer data between systems.

Although these operations are often used together, they serve different purposes.

**Archiving** combines multiple files and directories into a single archive file. It makes file management and distribution easier without necessarily reducing the total size.

**Compression** reduces the amount of storage space required by encoding data more efficiently. It can be applied to individual files or to an archive.

For example, an archive can contain hundreds of files and directories in a single file, while compression can reduce the size of that archive.

#### Archiving vs. Compression

| Operation | Description | Example |
|---|---|---|
| Archiving | Combines multiple files and directories into one file | `tar` |
| Compression | Reduces the size of data | `gzip`, `bzip2`, `xz` |
| Extraction | Restores files from an archive | `tar -xf` |
| Decompression | Restores compressed data to its original form | `gunzip`, `bunzip2`, `unxz` |

A common example is the `.tar.gz` format:

- `tar` combines files and directories into an archive.
- `gzip` compresses the resulting archive.

#### Common Archive and Compression Formats

Linux supports several archive and compression formats.

| Format | Extension | Description |
|---|---|---|
| TAR | `.tar` | Combines files and directories without compression |
| GZIP | `.gz` | Compresses individual files |
| TAR.GZ | `.tar.gz` or `.tgz` | A TAR archive compressed with gzip |
| BZIP2 | `.bz2` | Compresses files using the bzip2 algorithm |
| TAR.BZ2 | `.tar.bz2` or `.tbz2` | A TAR archive compressed with bzip2 |
| XZ | `.xz` | Compresses files using the xz format |
| TAR.XZ | `.tar.xz` or `.txz` | A TAR archive compressed with xz |
| ZIP | `.zip` | Combines and compresses files into a single archive |

The choice of format depends on compression ratio, processing speed, compatibility, and the requirements of the environment.

---

### Working with tar

The `tar` command is one of the most widely used tools in Linux for creating, listing, and extracting archives.

The name `tar` comes from **Tape Archive**, as it was originally developed for storing files on magnetic tape.

Today, it is commonly used for backups, software distribution, and transferring collections of files.

Unlike compression utilities, `tar` does not compress data by default. It combines files and directories into a single archive while preserving important filesystem information.

#### Basic Syntax

The general syntax of the `tar` command is:

```bash
tar [options] archive-file files
```

The command accepts different options to determine whether an archive should be created, extracted, or inspected.

#### Common tar Options

| Option | Description |
|---|---|
| `-c` | Create a new archive |
| `-x` | Extract files from an archive |
| `-t` | List the contents of an archive |
| `-f` | Specify the archive filename |
| `-v` | Display processed files |
| `-z` | Use gzip compression |
| `-j` | Use bzip2 compression |
| `-J` | Use xz compression |
| `-C` | Change directory before performing the operation |
| `-p` | Preserve permission information when extracting, subject to privileges and system behavior |
| `--exclude` | Exclude matching files or directories |

Some options can be combined to perform multiple operations in one command.

#### Creating an Archive

Use the `-c` option to create a new archive and `-f` to specify its filename.

For example:

```bash
tar -cvf backup.tar project/
```

This command creates an archive named `backup.tar` containing the `project` directory and its contents.

The options mean:

- `-c`: Create a new archive.
- `-v`: Display the files being processed.
- `-f`: Specify the archive filename.

You can also archive multiple files and directories:

```bash
tar -cvf backup.tar file1.txt file2.txt documents/
```

This combines all specified items into one archive.

#### Listing Archive Contents

To inspect an archive without extracting it, use the `-t` option.

```bash
tar -tvf backup.tar
```

This displays the archive contents, including file permissions, ownership information, sizes, and modification dates.

For a simple file listing:

```bash
tar -tf backup.tar
```

#### Extracting an Archive

Use the `-x` option to extract an archive.

```bash
tar -xvf backup.tar
```

By default, the contents are extracted into the current working directory.

To extract an archive into a specific directory, use `-C`:

```bash
mkdir extracted
tar -xvf backup.tar -C extracted/
```

The destination directory must already exist.

#### Excluding Files from an Archive

The `--exclude` option allows you to skip specific files or directories when creating an archive.

For example:

```bash
tar -cvf backup.tar --exclude='*.log' project/
```

This creates an archive of the `project` directory while excluding files matching the `*.log` pattern.

You can exclude multiple patterns:

```bash
tar -cvf backup.tar \
  --exclude='*.log' \
  --exclude='*.tmp' \
  project/
```

#### Extracting Specific Files

You do not always need to extract an entire archive.

You can specify the exact file or directory you want to extract:

```bash
tar -xvf backup.tar project/notes.txt
```

To extract a specific directory:

```bash
tar -xvf backup.tar project/documents/
```

The specified path must match the path stored inside the archive.

#### Working with Different Directories

The `-C` option allows `tar` to change directories before processing files.

For example:

```bash
tar -cvf backup.tar -C /home/alice project/
```

This creates an archive containing `project/` without storing the full `/home/alice/` path.

This is useful when you want to control the directory structure inside an archive.

#### Preserving Permissions and Ownership

By default, `tar` stores filesystem metadata such as permissions and modification times in the archive.

During extraction, ownership restoration and permission handling depend on the extraction options, user privileges, and system configuration.

For example:

```bash
tar -xvpf backup.tar
```

The `-p` option requests preservation of permission information during extraction.

Restoring original ownership generally requires sufficient privileges. When extracting as a regular user, ownership is normally assigned to the extracting user.

**Important:** Be careful when extracting archives from untrusted sources. Archive entries can contain unexpected paths or file types, so inspect their contents before extraction.

---

### Working with gzip

`gzip` is a widely used compression utility in Linux that uses the DEFLATE compression algorithm.

It is commonly used to compress individual files and TAR archives.

Gzip generally provides a good balance between compression speed and compression ratio.

#### Compressing Files

The basic syntax is:

```bash
gzip [options] file
```

To compress a file:

```bash
gzip file.txt
```

This creates a compressed file named:

```text
file.txt.gz
```

By default, `gzip` removes the original uncompressed file after successful compression.

#### Keeping the Original File

If you want to preserve the original file, use the `-k` option.

```bash
gzip -k file.txt
```

This creates `file.txt.gz` while keeping `file.txt` unchanged.

#### Decompressing Files

There are several ways to decompress a gzip file.

Using `gunzip`:

```bash
gunzip file.txt.gz
```

Using `gzip -d`:

```bash
gzip -d file.txt.gz
```

Both commands restore the original file and normally remove the compressed file.

To keep the compressed file:

```bash
gzip -dk file.txt.gz
```

#### Compression Levels

Gzip supports different compression levels, ranging from `1` to `9`.

| Option | Description |
|---|---|
| `-1` | Fast compression with a lower compression ratio |
| `-6` | Default compression level |
| `-9` | Maximum compression effort |

Examples:

```bash
gzip -1 file.txt
gzip -6 file.txt
gzip -9 file.txt
```

Higher compression levels generally require more processing time and may produce smaller files.

#### Displaying Compression Information

Use the `-l` option to display information about a compressed file.

```bash
gzip -l file.txt.gz
```

The output includes:

- Compressed size
- Original size
- Compression ratio
- Original filename

#### Creating gzip-compressed TAR Archives

Gzip can be combined with `tar` to create compressed archives.

```bash
tar -czvf backup.tar.gz project/
```

The options mean:

- `-c`: Create an archive.
- `-z`: Compress using gzip.
- `-v`: Display processed files.
- `-f`: Specify the archive filename.

The `.tgz` extension is also commonly used:

```bash
tar -czvf backup.tgz project/
```

Extract the archive:

```bash
tar -xzvf backup.tar.gz
```

List its contents without extracting:

```bash
tar -tzvf backup.tar.gz
```

#### Summary

- `gzip` compresses individual files.
- `gunzip` and `gzip -d` decompress gzip files.
- `-k` preserves the original file.
- Compression levels range from `1` to `9`.
- `tar -z` combines archiving and gzip compression.

---

### Working with bzip2

`bzip2` is a compression utility that uses the Burrows-Wheeler block-sorting compression algorithm together with Huffman coding.

It is commonly used when a higher compression ratio is preferred over compression speed.

Compared with gzip, bzip2 often produces smaller files for suitable data, although it generally requires more processing time.

#### Compressing Files

The basic syntax is:

```bash
bzip2 [options] file
```

To compress a file:

```bash
bzip2 file.txt
```

This creates:

```text
file.txt.bz2
```

By default, the original file is removed after successful compression.

#### Keeping the Original File

Use the `-k` option to keep the original file:

```bash
bzip2 -k file.txt
```

This creates `file.txt.bz2` without removing `file.txt`.

#### Decompressing Files

Use `bunzip2` to decompress a file:

```bash
bunzip2 file.txt.bz2
```

You can also use:

```bash
bzip2 -d file.txt.bz2
```

To keep the compressed file:

```bash
bzip2 -dk file.txt.bz2
```

#### Compression Levels

Bzip2 provides compression levels from `1` to `9`.

| Option | Description |
|---|---|
| `-1` | Smaller memory usage and faster processing |
| `-9` | Larger compression blocks and potentially better compression |

Examples:

```bash
bzip2 -1 file.txt
bzip2 -9 file.txt
```

The higher compression level may improve the compression ratio but can require more memory and processing time.

#### Creating bzip2-compressed TAR Archives

The `-j` option tells `tar` to use bzip2 compression.

```bash
tar -cjvf backup.tar.bz2 project/
```

Extract the archive:

```bash
tar -xjvf backup.tar.bz2
```

List its contents:

```bash
tar -tjvf backup.tar.bz2
```

The `.tbz2` and `.tbz` extensions are also used for bzip2-compressed TAR archives.

#### Summary

- `bzip2` compresses files using the bzip2 algorithm.
- `bunzip2` and `bzip2 -d` decompress files.
- `-k` preserves the original file.
- Compression levels range from `1` to `9`.
- `tar -j` combines archiving and bzip2 compression.

---

### Working with xz

`xz` is a compression utility that uses the LZMA2 compression algorithm.

It is commonly used for software packages, system archives, and backups where a high compression ratio is important.

XZ often provides better compression than gzip and bzip2 for suitable data, but it can require more processing time and memory, especially at higher compression settings.

#### Compressing Files

The basic syntax is:

```bash
xz [options] file
```

To compress a file:

```bash
xz file.txt
```

This creates:

```text
file.txt.xz
```

By default, the original file is removed after successful compression.

#### Keeping the Original File

Use the `-k` option:

```bash
xz -k file.txt
```

This creates the compressed file while keeping the original.

#### Decompressing Files

Use `unxz` to decompress a file:

```bash
unxz file.txt.xz
```

Alternatively:

```bash
xz -d file.txt.xz
```

To keep the compressed file:

```bash
xz -dk file.txt.xz
```

#### Compression Levels

XZ supports compression levels from `0` to `9`.

| Option | Description |
|---|---|
| `-0` | Fastest compression with a lower compression ratio |
| `-6` | Default compression level |
| `-9` | Higher compression effort and memory usage |

Examples:

```bash
xz -0 file.txt
xz -6 file.txt
xz -9 file.txt
```

Higher compression levels can produce smaller archives but may require significantly more memory and processing time.

#### Creating xz-compressed TAR Archives

The `-J` option tells `tar` to use xz compression.

```bash
tar -cJvf backup.tar.xz project/
```

Extract the archive:

```bash
tar -xJvf backup.tar.xz
```

List its contents:

```bash
tar -tJvf backup.tar.xz
```

The `.txz` extension is also commonly used.

#### Summary

- `xz` compresses files using the LZMA2 algorithm.
- `unxz` and `xz -d` decompress files.
- `-k` preserves the original file.
- Compression levels range from `0` to `9`.
- `tar -J` combines archiving and xz compression.

---

### Working with zip and unzip

The `zip` and `unzip` utilities are commonly used to create, inspect, and extract ZIP archives.

Unlike the separate TAR and compression utilities, ZIP combines archiving and compression into a single format.

ZIP is particularly useful when exchanging files between Linux, Windows, and macOS systems because it is widely supported across operating systems.

#### Creating ZIP Archives

The basic syntax is:

```bash
zip [options] archive.zip files
```

To create an archive containing a single file:

```bash
zip backup.zip file.txt
```

To include multiple files:

```bash
zip backup.zip file1.txt file2.txt
```

To include an entire directory and its contents, use the `-r` option:

```bash
zip -r backup.zip project/
```

The `-r` option enables recursive processing of directories.

#### Listing ZIP Archive Contents

Use `unzip -l` to display the contents of a ZIP archive without extracting it.

```bash
unzip -l backup.zip
```

This displays the names, sizes, and other information about the archived files.

#### Extracting ZIP Archives

To extract an archive into the current directory:

```bash
unzip backup.zip
```

To extract it into a specific directory:

```bash
mkdir extracted
unzip backup.zip -d extracted/
```

The destination directory is created automatically if it does not already exist.

#### Extracting Specific Files

You can extract only selected files from a ZIP archive.

```bash
unzip backup.zip file.txt
```

To extract a specific directory:

```bash
unzip backup.zip project/documents/*
```

The specified paths must match the paths stored inside the archive.

#### Updating ZIP Archives

The `-u` option updates an existing ZIP archive by adding new files or replacing archived files with newer versions.

```bash
zip -u backup.zip file.txt
```

You can also add another file to an existing archive:

```bash
zip backup.zip new-file.txt
```

#### Removing Files from ZIP Archives

The `-d` option removes specified entries from an existing ZIP archive.

```bash
zip -d backup.zip file.txt
```

This removes the archived entry without deleting the original file from the filesystem.

#### Testing ZIP Archive Integrity

Use the `-T` option to test whether a ZIP archive can be read successfully.

```bash
unzip -t backup.zip
```

This checks the archive's contents and reports any detected errors.

#### Common ZIP Options

| Option | Description |
|---|---|
| `-r` | Include directories recursively |
| `-q` | Suppress normal output |
| `-v` | Display detailed information |
| `-u` | Update existing archive entries |
| `-d` | Delete entries from an archive |
| `-l` | List archive contents when used with `unzip` |
| `-d` | Specify an extraction directory when used with `unzip` |

Note that the meaning of `-d` depends on whether it is used with `zip` or `unzip`.

#### Summary

- `zip` creates ZIP archives containing files and directories.
- `unzip` extracts ZIP archives.
- `zip -r` includes directories recursively.
- `unzip -l` lists archive contents.
- `unzip -t` checks archive integrity.
- ZIP is widely supported across different operating systems.

---


## Copying and Renaming

### Copying Files with cp

The `cp` command is used to copy files and directories from one location to another in Linux.

Copying creates a separate copy of the original file, allowing both files to exist independently. Changes made to one copy do not normally affect the other.

The `cp` command is commonly used for creating backups, duplicating files, organizing directories, and transferring data between locations.

#### Basic Syntax

The general syntax of the `cp` command is:

```bash
cp [options] source destination
```

- `source`: The file or directory to be copied.
- `destination`: The location or name of the new copy.
- `options`: Additional arguments that control the copying process.

#### Copying a File

To copy a file in the current directory:

```bash
cp file.txt backup.txt
```

This creates a new file named `backup.txt` containing the same data as `file.txt`.

If the destination file already exists, its contents are normally overwritten.

#### Copying a File into Another Directory

To copy a file into an existing directory:

```bash
cp file.txt /home/alice/documents/
```

The file is copied into the specified directory while keeping its original filename.

You can also specify a new filename:

```bash
cp file.txt /home/alice/documents/notes.txt
```

#### Copying Multiple Files

The `cp` command supports copying multiple files into a single destination directory.

```bash
cp file1.txt file2.txt file3.txt /home/alice/documents/
```

All three files are copied into the specified directory.

When copying multiple files, the destination must be a directory.

#### Copying Directories

By default, `cp` does not copy directories recursively.

Use the `-r` or `-R` option to copy a directory and its contents.

```bash
cp -r project/ project-backup/
```

This creates a copy of the `project` directory, including its files and subdirectories.

The recursive option is especially useful when backing up directory structures.

#### Preserving File Attributes

The `-p` option preserves important file attributes, including permissions, ownership where permitted, and timestamps.

```bash
cp -p file.txt backup.txt
```

To preserve these attributes while copying a directory recursively:

```bash
cp -rp project/ project-backup/
```

The `-a` option enables archive mode, which is useful for creating more complete copies of directory structures.

```bash
cp -a project/ project-backup/
```

Archive mode preserves attributes and copies directories recursively. It also attempts to preserve symbolic links, hard links, and other relevant filesystem information.

Some attributes may not be preserved if the user lacks the necessary privileges or the destination filesystem does not support them.

#### Common cp Options

| Option | Description |
|---|---|
| `-r`, `-R` | Copy directories recursively |
| `-i` | Ask before overwriting an existing destination |
| `-n` | Do not overwrite existing files |
| `-v` | Display the files being copied |
| `-p` | Preserve file mode, ownership where permitted, and timestamps |
| `-a` | Archive mode; recursively copy and preserve relevant attributes |
| `-u` | Copy only when the source is newer or the destination is missing |
| `-f` | Remove an existing destination file if it cannot be opened for writing, then retry |

#### Interactive Copying

The `-i` option asks for confirmation before overwriting an existing file.

```bash
cp -i file.txt backup.txt
```

If `backup.txt` already exists, the command prompts before replacing it.

This is useful when copying important files and avoiding accidental data loss.

#### Verbose Copying

The `-v` option displays information about the copying process.

```bash
cp -v file.txt backup.txt
```

For directories:

```bash
cp -rv project/ project-backup/
```

This displays the files and directories being copied.

#### Copying Only Newer Files

The `-u` option copies a source file only if the destination does not exist or the source is newer than the destination.

```bash
cp -u file.txt backup.txt
```

This can be useful when updating existing copies without unnecessarily replacing newer files.

#### Summary

- `cp` creates copies of files and directories.
- `cp -r` copies directories recursively.
- `cp -p` preserves important file attributes.
- `cp -a` is useful for creating attribute-preserving directory copies.
- `cp -i` helps prevent accidental overwriting.
- `cp -v` displays copying operations.

---

### Moving and Renaming Files with mv

The `mv` command is used to move files and directories from one location to another or change their names.

Unlike `cp`, which creates a separate copy, `mv` changes the location or name of the original filesystem entry.

The command is commonly used for organizing files, restructuring directories, and renaming existing data.

#### Basic Syntax

The general syntax of the `mv` command is:

```bash
mv [options] source destination
```

- `source`: The file or directory to move or rename.
- `destination`: The new name or destination location.
- `options`: Additional arguments that control the operation.

#### Renaming a File

To rename a file in the current directory:

```bash
mv old-name.txt new-name.txt
```

This changes the filename from `old-name.txt` to `new-name.txt`.

The file remains in the same directory.

#### Moving a File into Another Directory

To move a file into an existing directory:

```bash
mv file.txt /home/alice/documents/
```

The file is moved to the specified directory while keeping its original name.

#### Moving and Renaming a File

You can move a file and change its name in a single command:

```bash
mv file.txt /home/alice/documents/notes.txt
```

This moves `file.txt` into the `documents` directory and renames it to `notes.txt`.

#### Moving Multiple Files

The `mv` command can move multiple files into one destination directory.

```bash
mv file1.txt file2.txt file3.txt /home/alice/documents/
```

The destination must be an existing directory when multiple source files are specified.

#### Moving Directories

Directories can also be moved using `mv`.

```bash
mv project/ /home/alice/
```

This moves the entire `project` directory and its contents into `/home/alice/`.

Unlike `cp`, the `mv` command does not require a recursive option to move directories.

#### Common mv Options

| Option | Description |
|---|---|
| `-i` | Ask before overwriting an existing destination |
| `-n` | Do not overwrite existing files |
| `-v` | Display the files being moved |
| `-f` | Do not prompt before overwriting |
| `-u` | Move only when the source is newer or the destination is missing |
| `-T` | Treat the destination as a normal file rather than a directory |

#### Interactive Moving

The `-i` option asks for confirmation before overwriting an existing destination.

```bash
mv -i file.txt backup.txt
```

If `backup.txt` already exists, the command asks before replacing it.

#### Verbose Moving

The `-v` option displays information about the operation.

```bash
mv -v file.txt /home/alice/documents/
```

For directories:

```bash
mv -v project/ /home/alice/
```

#### Moving Without Overwriting

The `-n` option prevents the command from replacing an existing destination.

```bash
mv -n file.txt backup.txt
```

If `backup.txt` already exists, the source file is not moved over it.

#### Understanding How mv Works

When moving a file within the same filesystem, `mv` normally performs a rename operation.

This operation changes the directory entry without copying the file's contents.

When moving a file across different filesystems, a direct rename is generally not possible. In that case, `mv` typically copies the file to the destination and then removes the original after a successful copy.

This distinction explains why moving files within the same filesystem is usually very fast, while moving files between different filesystems can take longer.

#### Summary

- `mv` moves files and directories.
- `mv` can also rename files and directories.
- Moving a directory does not require a recursive option.
- `mv -i` asks before overwriting.
- `mv -n` prevents overwriting existing destinations.
- Moving files across filesystems may involve copying and removing the original.

---

### Working with install

The `install` command is a utility used to copy files and set their permissions and ownership during the process.

It is commonly used by system administrators and software installation scripts to place executable files, configuration files, and other resources in their appropriate system directories.

Unlike `cp`, `install` provides convenient options for setting file permissions and ownership as part of the same operation.

#### Basic Syntax

The general syntax of the `install` command is:

```bash
install [options] source destination
```

It can also be used to create directories without copying a source file.

#### Installing a File

To copy a file into another location:

```bash
install script.sh /usr/local/bin/script.sh
```

This copies `script.sh` to `/usr/local/bin/`.

By default, `install` sets the destination file permissions to `755`, unless a different mode is specified.

Writing to system directories normally requires elevated privileges:

```bash
sudo install script.sh /usr/local/bin/script.sh
```

#### Setting File Permissions

The `-m` option specifies the permissions of the destination file.

```bash
sudo install -m 755 script.sh /usr/local/bin/script.sh
```

This copies the file and assigns the following permissions:

```text
-rwxr-xr-x
```

Another example:

```bash
install -m 644 config.conf config-backup.conf
```

This creates a copy with read and write permissions for the owner and read-only permissions for the group and others.

#### Setting Ownership

The `-o` option specifies the owner, while the `-g` option specifies the group.

For example:

```bash
sudo install -o alice -g developers script.sh /opt/myapp/script.sh
```

This copies the file and assigns `alice` as its owner and `developers` as its group.

These options generally require sufficient privileges.

#### Creating Directories

The `-d` option creates directories instead of copying files.

```bash
sudo install -d /opt/myapp
```

You can also specify permissions and ownership:

```bash
sudo install -d -m 755 -o alice -g developers /opt/myapp
```

This creates the directory with the specified permissions and ownership.

Parent directories are created when necessary.

#### Creating Multiple Directories

You can create multiple directories in one command:

```bash
install -d project/bin project/config project/logs
```

This creates the specified directory structure.

#### Installing Files into a Directory

The `-t` option specifies the target directory.

```bash
install -t /usr/local/bin script1.sh script2.sh
```

This copies both scripts into `/usr/local/bin/`.

You can also set permissions:

```bash
sudo install -m 755 -t /usr/local/bin script1.sh script2.sh
```

#### Common install Options

| Option | Description |
|---|---|
| `-m` | Set destination file permissions |
| `-o` | Set destination owner |
| `-g` | Set destination group |
| `-d` | Create directories instead of copying files |
| `-v` | Display created directories and installed files |
| `-t` | Specify the target directory |
| `-D` | Create necessary parent directories before copying |
| `-p` | Preserve timestamps of the source file |

#### Comparing cp, mv, and install

These three commands can all be used to manage files, but they serve different purposes.

| Command | Main Purpose | Creates a Copy | Moves or Renames | Sets Permissions During Operation |
|---|---|---|---|---|
| `cp` | Copying files and directories | Yes | No | No, except when preserving existing attributes |
| `mv` | Moving and renaming files | No, normally | Yes | No |
| `install` | Installing files and creating directories | Yes | No | Yes |

#### Practical Example

Imagine that you have developed a shell script named `backup.sh` and want to install it as a system command.

First, create the destination directory if necessary:

```bash
sudo install -d /usr/local/bin
```

Then install the script with executable permissions:

```bash
sudo install -m 755 backup.sh /usr/local/bin/backup
```

You can now execute the script by using its command name:

```bash
backup
```

This works because `/usr/local/bin` is commonly included in the system's `PATH` environment variable.

#### Summary

- `install` copies files to a destination.
- `install -m` sets file permissions.
- `install -o` and `install -g` set ownership.
- `install -d` creates directories.
- `install -t` specifies a target directory.
- It is commonly used when deploying executables and configuration files.

---


## Symbolic Links and Hard Links

Linux provides two main types of links that allow multiple directory entries to reference files: hard links and symbolic links.

Links are useful for sharing files between directories, creating shortcuts to frequently accessed files, organizing directory structures, and providing alternative paths to existing resources.

To understand how links work, it is important to first understand inodes.

### Understanding Inodes

An **inode** (index node) is a data structure used by Linux filesystems to store information about a file or directory.

Every filesystem object has an inode that stores metadata about that object. The inode does not store the file's name or its directory path. Instead, directory entries associate filenames with inode numbers.

An inode typically stores information such as:

- File type (regular file, directory, symbolic link, and others).
- File permissions.
- Owner user ID (UID).
- Owner group ID (GID).
- File size.
- Timestamps, such as access time, modification time, and metadata change time.
- Number of hard links pointing to the inode.
- Pointers or references to the file's data blocks.

**Viewing Inode Information**

The `ls -i` command displays the inode number associated with each directory entry.

```bash
ls -i
```

Example output:

```text
131073 notes.txt
131074 script.sh
131075 documents
```

The `-i` option displays the inode number before each filename.

To display inode numbers in a detailed listing, use:

```bash
ls -li
```

The `stat` command provides more detailed information about a file, including its inode number and link count.

```bash
stat notes.txt
```

Example output:

```text
  File: notes.txt
  Size: 120        Blocks: 8
  IO Block: 4096   regular file
Device: 8,1        Inode: 131073
Links: 1
Access: (0644/-rw-r--r--)  Uid: (1000/user)   Gid: (1000/user)
```

The `Inode` field shows the inode number, while `Links` indicates how many hard links refer to that inode.

**Important Concepts**

- A filename is a directory entry that points to an inode.
- An inode stores file metadata and references to the file's data.
- Multiple filenames can reference the same inode through hard links.
- Symbolic links have their own inode and store a path to another file or directory.
- Inode numbers are unique within a filesystem, but the same inode number may exist on different filesystems.

Understanding inodes helps explain why hard links work differently from symbolic links.

### Hard Links

A **hard link** is an additional directory entry that points directly to the same inode as an existing file.

Unlike a symbolic link, a hard link does not point to a filename or path. It references the same underlying inode as the original directory entry.

This means that both filenames refer to the same file data and share the same metadata, including permissions, ownership, and timestamps.

**Creating a Hard Link**

The `ln` command creates hard links.

Syntax:

```bash
ln SOURCE LINK_NAME
```

Example:

```bash
echo "Learning Linux" > original.txt
ln original.txt backup.txt
```

This creates `backup.txt` as a hard link to `original.txt`.

To verify that both names reference the same inode, use:

```bash
ls -li original.txt backup.txt
```

Example output:

```text
131073 -rw-r--r-- 2 user user 14 Nov 21 10:00 original.txt
131073 -rw-r--r-- 2 user user 14 Nov 21 10:00 backup.txt
```

Both files have the same inode number (`131073`).

The link count is `2`, indicating that two directory entries reference the same inode.

**Understanding Hard Link Behavior**

Because hard links reference the same inode, changes made through one name are visible through the other.

For example:

```bash
echo "New content" >> backup.txt
cat original.txt
```

Output:

```text
Learning Linux
New content
```

The content appears in both filenames because they refer to the same underlying file.

Changing the file's permissions through one name also changes the permissions observed through the other:

```bash
chmod 600 backup.txt
ls -l original.txt backup.txt
```

Both names display the updated permissions.

**Removing a Hard Link**

Removing one hard link does not remove the underlying file data as long as another hard link still exists.

```bash
rm original.txt
```

The remaining link can still access the file:

```bash
cat backup.txt
```

The file's data is released by the filesystem only after its link count reaches zero and no process is still holding it open.

**Important Limitations of Hard Links**

Hard links have some restrictions:

- Hard links generally cannot be created for directories because doing so could introduce filesystem cycles and complicate directory traversal.
- Hard links cannot cross filesystem boundaries. Both names must belong to the same filesystem.
- A hard link does not depend on the original filename. Removing or renaming one directory entry does not invalidate the others.
- Hard links share the same inode, so they are not independent copies of a file.

To create a hard link, the source and destination must be on the same filesystem.

### Symbolic Links

A **symbolic link**, also known as a symlink or soft link, is a special type of file that stores a path to another file or directory.

Unlike a hard link, a symbolic link has its own inode. It does not directly reference the target's inode. Instead, the filesystem follows the stored path when the link is accessed.

Symbolic links are commonly used to create shortcuts, provide alternative paths, and organize files and directories.

**Creating a Symbolic Link**

The `ln -s` command creates a symbolic link.

Syntax:

```bash
ln -s TARGET LINK_NAME
```

Example:

```bash
echo "Learning Linux" > original.txt
ln -s original.txt shortcut.txt
```

This creates `shortcut.txt` as a symbolic link to `original.txt`.

To verify the link, use:

```bash
ls -l
```

Example output:

```text
-rw-r--r-- 1 user user 14 Nov 21 10:00 original.txt
lrwxrwxrwx 1 user user 11 Nov 21 10:01 shortcut.txt -> original.txt
```

The `l` at the beginning of the symbolic link's permissions indicates that it is a symbolic link.

The `->` symbol shows the path stored in the link.

You can also use `readlink` to display the target path:

```bash
readlink shortcut.txt
```

Output:

```text
original.txt
```

**Using Symbolic Links with Directories**

Symbolic links can point to directories, making them useful for accessing directories through alternative paths.

Example:

```bash
mkdir projects
ln -s projects workspace
```

Now `workspace` provides another path to the `projects` directory.

You can access its contents through either path:

```bash
ls projects
ls workspace
```

You can also change into the directory through the symbolic link:

```bash
cd workspace
pwd
```

Depending on the shell and its configuration, `pwd` may display the logical path containing the symbolic link rather than the physical target path.

To display the physical directory path, use:

```bash
pwd -P
```

**Absolute and Relative Symbolic Links**

Symbolic links can store either absolute or relative paths.

An **absolute symbolic link** stores a complete path starting from the root directory.

Example:

```bash
ln -s /home/user/projects project-link
```

An absolute link points to the same location regardless of the current working directory.

A **relative symbolic link** stores a path relative to the directory containing the symbolic link.

Example:

```bash
ln -s ../projects project-link
```

Relative links can be useful when moving an entire directory structure because the relationship between the link and its target can remain unchanged.

However, their validity depends on the relative directory structure remaining intact.

**Broken Symbolic Links**

A symbolic link becomes broken, or dangling, when its target no longer exists or cannot be resolved.

For example:

```bash
ln -s missing.txt broken-link
```

Because `missing.txt` does not exist, accessing the link fails:

```bash
cat broken-link
```

Output:

```text
cat: broken-link: No such file or directory
```

The symbolic link itself still exists, even though its target is unavailable.

You can identify symbolic links with:

```bash
ls -l
```

A broken link is typically displayed with its target path, just like a normal symbolic link.

The `readlink` command can still display the stored target:

```bash
readlink broken-link
```

Output:

```text
missing.txt
```

To test whether a symbolic link points to an existing target, use:

```bash
test -e broken-link && echo "Target exists" || echo "Target is missing"
```

**Important Characteristics of Symbolic Links**

- Symbolic links have their own inode.
- They can point to files or directories.
- They can reference targets on different filesystems.
- They can reference targets using absolute or relative paths.
- Removing the target does not automatically remove the symbolic link.
- Accessing a broken symbolic link usually results in a file-not-found error.
- Symbolic links can point to other symbolic links, creating chains of links.

### Comparing Hard Links and Symbolic Links

Hard links and symbolic links both provide alternative names or paths for accessing files, but they work differently internally.

The following table summarizes their main differences.

| Feature | Hard Link | Symbolic Link |
|---|---|---|
| Inode | Shares the target's inode | Has its own inode |
| Target reference | Direct inode reference | Stores a pathname |
| Filesystem boundaries | Cannot cross filesystems | Can cross filesystems |
| Directory links | Generally not allowed | Allowed |
| Target removal | Other hard links remain valid | Link usually becomes broken |
| Target renaming | Other hard links remain valid | May become broken if the stored path no longer resolves |
| File permissions | Shared with other hard links | Link permissions are generally not used for access control; target permissions apply |
| Link count | Increases the target inode's link count | Does not increase the target's hard link count |
| Relative paths | Not applicable | Supported |
| Typical use | Multiple names for the same file | Shortcuts and alternative paths |

**Practical Comparison**

Consider the following commands:

```bash
echo "Linux Filesystem" > original.txt

ln original.txt hardlink.txt
ln -s original.txt symlink.txt
```

Now inspect their inode numbers:

```bash
ls -li original.txt hardlink.txt symlink.txt
```

Example output:

```text
131073 -rw-r--r-- 2 user user 17 Nov 21 10:00 original.txt
131073 -rw-r--r-- 2 user user 17 Nov 21 10:00 hardlink.txt
131074 lrwxrwxrwx 1 user user 11 Nov 21 10:01 symlink.txt -> original.txt
```

Notice that:

- `original.txt` and `hardlink.txt` share the same inode number.
- `symlink.txt` has a different inode number.
- The hard link count is `2`.
- The symbolic link has its own inode and does not increase the target's hard link count.

Now remove the original filename:

```bash
rm original.txt
```

Check the remaining files:

```bash
cat hardlink.txt
cat symlink.txt
```

The hard link still works because it directly references the inode.

The symbolic link fails because it still points to `original.txt`, which no longer exists.

This example demonstrates the most important difference between hard links and symbolic links.

### Managing and Removing Links

Linux provides several commands for inspecting, creating, updating, and removing links.

**Identifying Links**

Use `ls -l` to identify symbolic links:

```bash
ls -l
```

Symbolic links are displayed with an `l` file-type indicator and an arrow pointing to their target.

Use `ls -i` to compare inode numbers:

```bash
ls -i
```

Files with the same inode number on the same filesystem are hard links to the same inode.

Use `stat` to inspect link information:

```bash
stat filename
```

For symbolic links, use:

```bash
stat -L symlink
```

The `-L` option follows the symbolic link and displays information about its target.

To inspect the symbolic link itself rather than its target, use:

```bash
stat symlink
```

**Creating Links**

Create a hard link:

```bash
ln original.txt hardlink.txt
```

Create a symbolic link:

```bash
ln -s original.txt symlink.txt
```

Create a symbolic link to a directory:

```bash
ln -s /var/log log-link
```

**Updating Symbolic Links**

If a symbolic link already exists, creating another link with the same name normally fails.

The `-f` option can force replacement of an existing destination:

```bash
ln -sf new-target.txt shortcut.txt
```

This replaces the existing `shortcut.txt` entry with a symbolic link to `new-target.txt`.

Use this option carefully because it can replace an existing file or link at the destination.

**Removing Links**

Use `rm` to remove a symbolic link:

```bash
rm symlink.txt
```

This removes the link itself, not the target.

Use `unlink` as another way to remove a single link:

```bash
unlink hardlink.txt
```

This removes one hard link to the file.

If other hard links still exist, the underlying file remains accessible through them.

**Removing Symbolic Links to Directories**

A symbolic link to a directory can also be removed with `rm`:

```bash
rm directory-link
```

This removes the symbolic link, not the directory it points to.

Do not add a trailing slash to a symbolic link when removing it. A trailing slash can cause the path to be interpreted as a directory path rather than the link itself.

**Finding Symbolic Links**

The `find` command can locate symbolic links.

Find symbolic links in the current directory and its subdirectories:

```bash
find . -type l
```

Display their target paths:

```bash
find . -type l -exec readlink {} \;
```

Find symbolic links that point to missing targets:

```bash
find . -xtype l
```

On GNU `find`, `-xtype l` identifies symbolic links whose targets cannot be resolved.

**Important Safety Notes**

- Removing a symbolic link does not remove its target.
- Removing one hard link does not necessarily remove the file's data.
- Avoid using recursive deletion commands on paths you have not inspected.
- Be careful when using symbolic links in scripts, especially when the target may change.
- Always verify a link's target before replacing or deleting it.

**Summary**

- An inode stores file metadata and references to file data.
- A hard link is another directory entry referencing the same inode.
- A symbolic link is a separate filesystem object that stores a path to another file or directory.
- Hard links generally cannot cross filesystem boundaries or reference directories.
- Symbolic links can cross filesystem boundaries and reference directories.
- Removing a hard link removes only that directory entry.
- Removing a symbolic link removes the link itself, not its target.
- A symbolic link can become broken when its target is moved or deleted.
- Commands such as `ln`, `ln -s`, `ls -i`, `readlink`, `stat`, `find`, `rm`, and `unlink` are useful for working with links.

**Useful Resources**

- [Linux `ln` Manual](https://man7.org/linux/man-pages/man1/ln.1.html)
- [Linux `link` Manual](https://man7.org/linux/man-pages/man2/link.2.html)
- [Linux `symlink` Manual](https://man7.org/linux/man-pages/man7/symlink.7.html)
- [Linux `inode` Manual](https://man7.org/linux/man-pages/man7/inode.7.html)
- [Linux `stat` Manual](https://man7.org/linux/man-pages/man1/stat.1.html)

---


## Practical Exercises

The following exercises are designed to reinforce the concepts and commands introduced throughout Day 04.

Each exercise focuses on a specific topic and gradually increases in difficulty. Complete the tasks using a Linux terminal and verify your results with the appropriate commands.

### Exercise 1 — Understanding File Permissions

**Objective:** Practice viewing and understanding file permissions.

1. Create a directory named `permissions-lab`.
2. Navigate into the directory.
3. Create three files:
   - `public.txt`
   - `private.txt`
   - `script.sh`
4. Display their permissions using `ls -l`.
5. Display detailed information about `public.txt` using `stat`.
6. Change the permissions of `public.txt` to `644`.
7. Change the permissions of `private.txt` to `600`.
8. Change the permissions of `script.sh` to `755`.
9. Verify the updated permissions using `ls -l`.
10. Use symbolic notation to remove write permission from others on `script.sh`.
11. Explain the difference between permissions `644`, `600`, and `755`.

**Expected Knowledge:**
- Reading permission strings.
- Using `ls -l` and `stat`.
- Changing permissions with `chmod`.
- Understanding numeric and symbolic permission notation.

### Exercise 2 — Ownership and Special Permissions

**Objective:** Practice managing file ownership and understanding special permissions.

1. Create a directory named `ownership-lab`.
2. Create a file named `shared.txt` inside it.
3. Display the file's owner and group using `ls -l`.
4. Change the file's group using `chgrp` (if you have access to another group).
5. Use `chown` to change the file's owner (requires appropriate privileges).
6. Create a directory named `shared-directory`.
7. Enable the SGID permission on that directory.
8. Verify the permission using `ls -ld`.
9. Enable the sticky bit on another directory named `temporary`.
10. Verify the sticky bit using `ls -ld`.
11. Explain the purpose of SUID, SGID, and the sticky bit.

**Expected Knowledge:**
- Managing ownership with `chown` and `chgrp`.
- Understanding user and group ownership.
- Recognizing special permission bits.
- Using `chmod` with special permissions.

### Exercise 3 — Understanding umask

**Objective:** Understand how default permissions are calculated.

1. Display your current umask using:

   ```bash
   umask
   ```

2. Display the symbolic representation of your umask:

   ```bash
   umask -S
   ```

3. Set your umask to `022`.
4. Create a file named `file-022.txt`.
5. Create a directory named `directory-022`.
6. Check their permissions using `ls -l`.
7. Change your umask to `077`.
8. Create another file named `file-077.txt`.
9. Create another directory named `directory-077`.
10. Compare the permissions of all four objects.
11. Explain why files and directories have different default permission bases.

**Expected Knowledge:**
- Viewing and changing umask.
- Understanding default permission calculation.
- Comparing file and directory permissions.

### Exercise 4 — Creating and Managing Archives

**Objective:** Practice creating and inspecting compressed archives using `tar`.

1. Create a directory named `archive-lab`.
2. Inside it, create three files:
   - `file1.txt`
   - `file2.txt`
   - `file3.txt`
3. Add some sample content to each file.
4. Create an uncompressed TAR archive named `files.tar`.
5. List the contents of the archive without extracting it.
6. Create a gzip-compressed archive named `files.tar.gz`.
7. Create a bzip2-compressed archive named `files.tar.bz2`.
8. Create an xz-compressed archive named `files.tar.xz`.
9. Compare the sizes of the four archives using `ls -lh`.
10. Extract `files.tar.gz` into a directory named `extracted`.
11. Verify that all files were extracted successfully.

**Expected Knowledge:**
- Creating archives with `tar`.
- Using gzip, bzip2, and xz compression.
- Listing archive contents.
- Extracting archives into specific directories.
- Comparing archive sizes.

### Exercise 5 — Working with Compression Tools

**Objective:** Practice using individual compression utilities.

1. Create a file named `data.txt` containing several lines of text.
2. Compress it using `gzip`.
3. List the resulting files.
4. Decompress it using `gunzip`.
5. Repeat the process using `bzip2`.
6. Repeat the process using `xz`.
7. Use the `-k` option to keep the original file while compressing another copy.
8. Compare the compressed file sizes.
9. Create a ZIP archive containing multiple files.
10. List its contents using `unzip -l`.
11. Extract the ZIP archive into a directory named `zip-extracted`.
12. Test the integrity of the ZIP archive using `unzip -t`.

**Expected Knowledge:**
- Using gzip, bzip2, and xz.
- Understanding compression and decompression.
- Preserving original files.
- Creating and extracting ZIP archives.

### Exercise 6 — Copying and Moving Files

**Objective:** Practice copying, moving, and renaming files and directories.

1. Create a directory named `file-management-lab`.
2. Inside it, create two directories:
   - `source`
   - `destination`
3. Create three files inside `source`:
   - `notes.txt`
   - `script.sh`
   - `config.conf`
4. Copy `notes.txt` into `destination` using `cp`.
5. Copy all files from `source` into `destination`.
6. Create a directory named `backup`.
7. Copy the entire `source` directory into `backup` using `cp -r`.
8. Rename `notes.txt` to `linux-notes.txt` using `mv`.
9. Move `config.conf` into `destination`.
10. Use `mv -i` to observe how interactive overwrite confirmation works.
11. Display the directory structure using `ls -R`.
12. Explain the difference between copying, moving, and renaming files.

**Expected Knowledge:**
- Copying files and directories with `cp`.
- Moving and renaming with `mv`.
- Understanding recursive operations.
- Using interactive options.

### Exercise 7 — Installing Files with install

**Objective:** Practice using `install` to copy files and set permissions.

1. Create a shell script named `hello.sh`.
2. Add a simple command that displays `Hello from RootBound!`.
3. Create a directory named `install-lab`.
4. Use `install -d` to create a directory named `bin` inside `install-lab`.
5. Copy `hello.sh` into `install-lab/bin` with permissions `755`.
6. Verify the file permissions using `ls -l`.
7. Create another copy with permissions `644`.
8. Compare the permissions of both copies.
9. Use `install -D` to copy a file into a nested directory structure that does not yet exist.
10. Explain how `install` differs from `cp`.

**Expected Knowledge:**
- Copying files with `install`.
- Setting permissions during installation.
- Creating directories with `install -d`.
- Using `install -D`.

### Exercise 8 — Working with Hard Links

**Objective:** Understand how hard links share the same inode.

1. Create a directory named `hardlink-lab`.
2. Navigate into the directory.
3. Create a file named `original.txt`.
4. Add some text to the file.
5. Create a hard link named `hardlink.txt`.
6. Display the inode numbers using `ls -li`.
7. Verify that both filenames have the same inode number.
8. Display the link count using `stat`.
9. Append new content to `hardlink.txt`.
10. Read the content through `original.txt`.
11. Remove `original.txt`.
12. Verify that `hardlink.txt` still exists and can be accessed.
13. Explain why removing one filename does not necessarily delete the underlying file.

**Expected Knowledge:**
- Understanding inodes.
- Creating hard links with `ln`.
- Checking inode numbers and link counts.
- Understanding hard link behavior.

### Exercise 9 — Working with Symbolic Links

**Objective:** Practice creating, inspecting, and managing symbolic links.

1. Create a directory named `symlink-lab`.
2. Navigate into the directory.
3. Create a file named `target.txt`.
4. Add some content to the file.
5. Create a symbolic link named `shortcut.txt`.
6. Display the directory contents using `ls -l`.
7. Inspect the link target using `readlink`.
8. Compare the inode numbers of the target and symbolic link using `ls -li`.
9. Create a directory named `documents`.
10. Create a symbolic link named `documents-link` pointing to it.
11. Access the directory through the symbolic link.
12. Remove `target.txt`.
13. Verify that `shortcut.txt` still exists but no longer points to an available target.
14. Use `test -e` to check whether the target exists.
15. Remove the broken symbolic link without deleting any other files.

**Expected Knowledge:**
- Creating symbolic links with `ln -s`.
- Using `readlink`.
- Understanding absolute and relative paths.
- Identifying broken symbolic links.
- Removing symbolic links safely.

### Exercise 10 — Final Challenge: Linux File Management

**Objective:** Combine permissions, ownership, archiving, copying, installation, and links in a practical scenario.

Imagine you are preparing a small project directory that contains scripts, configuration files, and documentation.

Complete the following tasks:

1. Create a directory named `rootbound-project`.
2. Inside it, create the following directory structure:

   ```text
   rootbound-project/
   ├── bin/
   ├── config/
   ├── docs/
   └── backup/
   ```

3. Create the following files:
   - `bin/backup.sh`
   - `config/app.conf`
   - `docs/README.txt`
4. Add sample content to each file.
5. Set the permissions of `backup.sh` to `755`.
6. Set the permissions of `app.conf` to `600`.
7. Set the permissions of `README.txt` to `644`.
8. Display the permissions of all files and directories.
9. Create a symbolic link named `backup-script` pointing to `bin/backup.sh`.
10. Create a hard link named `README-backup.txt` pointing to `docs/README.txt`.
11. Verify the inode numbers of the hard link and its original file.
12. Create a compressed archive of the entire project using `tar` and gzip.
13. List the archive contents without extracting them.
14. Copy the archive into the `backup` directory using `cp`.
15. Use `install` to create a copy of `backup.sh` with executable permissions inside a new directory named `installed/bin`.
16. Verify the permissions of the installed script.
17. Remove the symbolic link and verify that the original script still exists.
18. Remove one of the hard links and verify that the other filename still works.
19. Extract the backup archive into a new directory named `restored`.
20. Verify that the restored project contains the expected files and directories.

**Final Review Questions**

1. What is an inode, and what information does it store?
2. What is the difference between a hard link and a symbolic link?
3. Why can hard links not normally reference directories?
4. What happens when the target of a symbolic link is deleted?
5. What is the difference between `chmod` and `chown`?
6. What is the purpose of SUID, SGID, and the sticky bit?
7. How does `umask` affect the default permissions of newly created files and directories?
8. What is the difference between archiving and compression?
9. What is the difference between `tar -czf` and `tar -cjf`?
10. What is the difference between `cp`, `mv`, and `install`?


**Goal:** Develop practical skills in managing file permissions and ownership, archiving and compressing files, copying and moving files, installing files, and creating and managing hard links and symbolic links in Linux.

<p align="right">
  <a href="../Day-03/README.md">← Day 03</a>
  &nbsp; | &nbsp;
  <a href="../Day-05/">Day 05 →</a>
  &nbsp; | &nbsp;
  <a href="#day-04--working-with-files">Back to top ↑</a>
</p>
