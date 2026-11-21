
<div id="top"></div>

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
    - [Moving and Renaming Files with mv](#moving-and-renaming-files-with-mv)
    - [Working with install](#working-with-install)
  - [Symbolic Links and Hard Links](#symbolic-links-and-hard-links)
    - [Understanding Inodes](#understanding-inodes)
    - [Hard Links](#hard-links)
    - [Symbolic Links](#symbolic-links)
    - [Comparing Hard Links and Symbolic Links](#comparing-hard-links-and-symbolic-links)
    - [Managing and Removing Links](#managing-and-removing-links)
  - [Practical Exercises](#practical-exercises)

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

### Moving and Renaming Files with mv

### Working with install

---

## Symbolic Links and Hard Links

### Understanding Inodes

### Hard Links

### Symbolic Links

### Comparing Hard Links and Symbolic Links

### Managing and Removing Links

---

## Practical Exercises

---

<p align="right">
  <a href="../Day-03/README.md">← Day 03</a>
  &nbsp; | &nbsp;
  <a href="../Day-05/">Day 05 →</a>
  &nbsp; | &nbsp;
  <a href="#top">Back to top ↑</a>
</p>
