<p align="center">
  <img src="../../media/images/Linux/Day-08/image_01.png" alt="image_01">
</p>

# Day 08 — User Management

## Table of Contents

- [Day 08 — User Management](#day-08--user-management)
  - [Table of Contents](#table-of-contents)
  - [User Management](#user-management)
    - [Understanding Linux Users and Groups](#understanding-linux-users-and-groups)
    - [Managing User Accounts](#managing-user-accounts)
    - [Managing Groups](#managing-groups)
    - [Understanding User and Group Information](#understanding-user-and-group-information)
    - [Managing User Passwords](#managing-user-passwords)
    - [Managing User Permissions and Access](#managing-user-permissions-and-access)
    - [Switching Between Users](#switching-between-users)
  - [Practical Exercises](#practical-exercises)
    - [Exercise 1 — Understanding Users and Groups](#exercise-1--understanding-users-and-groups)
    - [Exercise 2 — Creating and Managing User Accounts](#exercise-2--creating-and-managing-user-accounts)
    - [Exercise 3 — Managing Groups](#exercise-3--managing-groups)
    - [Exercise 4 — Understanding User and Group Information](#exercise-4--understanding-user-and-group-information)
    - [Exercise 5 — Managing User Passwords](#exercise-5--managing-user-passwords)
    - [Exercise 6 — Managing User Permissions and Access](#exercise-6--managing-user-permissions-and-access)
    - [Exercise 7 — Switching Between Users](#exercise-7--switching-between-users)
    - [Exercise 8 — User Management with Shell Commands](#exercise-8--user-management-with-shell-commands)
    - [Exercise 9 — Final Challenge: Linux User and Group Administration](#exercise-9--final-challenge-linux-user-and-group-administration)
    - [Exercise 10 — Knowledge Check](#exercise-10--knowledge-check)

---

## User Management

User management is an essential part of Linux system administration. Linux is a multi-user operating system, which means multiple users can access and use the same system while maintaining separate accounts, permissions, and resources.

Each user has an identity that determines which files, directories, commands, and system resources they can access. Linux uses user accounts, groups, ownership, and permissions to control access and protect system resources.

User management involves creating and modifying accounts, organizing users into groups, managing passwords, assigning permissions, and controlling access to the system.

### Understanding Linux Users and Groups

Linux uses users and groups to identify accounts and control access to system resources.

**Understanding Users**

A user is an account that allows an individual or service to interact with a Linux system. Every user account has a unique numerical identifier called a User ID (UID).

Linux generally supports three categories of user accounts:

| User Type | Description |
|---|---|
| Root User | The superuser with UID 0 and unrestricted administrative privileges. |
| Regular Users | Accounts created for people who use the system. |
| System Users | Accounts used by system services and applications to run processes with limited privileges. |

Every user account has several important attributes:

- **Username:** The name used to identify and log in to an account.
- **UID:** A unique numerical identifier assigned to the user.
- **GID:** The numerical identifier of the user's primary group.
- **Home Directory:** The directory used to store the user's personal files and configuration.
- **Login Shell:** The shell started when the user logs in interactively.
- **Password Information:** Authentication information associated with the account.

For example, a user account might have the following information:

```bash
username: alice
UID: 1001
GID: 1001
Home Directory: /home/alice
Shell: /bin/bash
```

**Understanding Groups**

A group is a collection of users that share access permissions to files, directories, and other system resources.

Every group has a unique numerical identifier called a Group ID (GID).

Linux uses two main types of group membership:

- **Primary Group:** The default group associated with a user's account. Files created by the user normally inherit this group as their group ownership, subject to directory settings and filesystem behavior.
- **Supplementary Groups:** Additional groups that provide users with access to shared resources and permissions.

For example, a user might belong to the following groups:

```text
alice : alice, developers, docker
```

In this example:

- `alice` is the user's primary group.
- `developers` and `docker` are supplementary groups.

**Understanding User and Group IDs**

Linux uses numerical identifiers internally rather than relying exclusively on usernames and group names.

- UID 0 is reserved for the root user.
- Regular user UID ranges depend on the Linux distribution and system configuration.
- System users commonly receive lower UIDs than regular users.
- Every group has a numerical GID.

You can inspect the UID and GID of the current user using:

```bash
id
```

Example output:

```text
uid=1001(alice) gid=1001(alice) groups=1001(alice),1002(developers),999(docker)
```

**Understanding the Difference Between Users and Groups**

| Feature | Users | Groups |
|---|---|---|
| Purpose | Identify individual accounts. | Organize users for shared access. |
| Identifier | UID | GID |
| Main Configuration | `/etc/passwd` | `/etc/group` |
| Password Information | `/etc/shadow` | Usually no separate group password needed. |
| Example | `alice` | `developers` |

Understanding users and groups is important because Linux uses them as the foundation of its permission and access control system.

### Managing User Accounts

Linux provides several commands for creating, modifying, and deleting user accounts.

The most common user management commands are:

- `useradd` — Create a new user.
- `usermod` — Modify an existing user.
- `userdel` — Delete a user account.
- `adduser` — A higher-level user creation utility available on some distributions.

**Creating Users with useradd**

The `useradd` command creates a new user account.

Basic syntax:

```bash
sudo useradd username
```

Example:

```bash
sudo useradd alice
```

This creates a user account named `alice`.

However, depending on the distribution and configuration, this command may not automatically create a home directory or configure an interactive login shell.

To create a user with a home directory, use:

```bash
sudo useradd -m alice
```

To specify a login shell:

```bash
sudo useradd -m -s /bin/bash alice
```

To specify a custom home directory:

```bash
sudo useradd -m -d /home/developers/alice alice
```

To specify a primary group:

```bash
sudo useradd -m -g developers alice
```

The primary group must already exist unless a distribution-specific tool handles its creation.

To specify supplementary groups:

```bash
sudo useradd -m -G developers,docker alice
```

To specify a UID:

```bash
sudo useradd -m -u 1500 alice
```

To create a user with multiple attributes:

```bash
sudo useradd -m -u 1500 -s /bin/bash -G developers,docker alice
```

Important `useradd` options:

| Option | Description |
|---|---|
| `-m` | Create the user's home directory. |
| `-M` | Do not create a home directory. |
| `-d` | Specify the home directory. |
| `-s` | Specify the login shell. |
| `-u` | Specify the UID. |
| `-g` | Specify the primary group. |
| `-G` | Specify supplementary groups. |
| `-c` | Add a comment or descriptive information. |
| `-e` | Set the account expiration date. |
| `-r` | Create a system account. |

For example:

```bash
sudo useradd -m -c "Development Account" -s /bin/bash alice
```

This creates a user with a home directory, a descriptive comment, and Bash as the login shell.

**Creating Users with adduser**

Some Linux distributions, particularly Debian and Ubuntu, provide the `adduser` command as a more interactive alternative to `useradd`.

Example:

```bash
sudo adduser alice
```

The command typically guides you through setting a password and entering additional account information. It also creates the home directory and configures the account according to distribution defaults.

Unlike `useradd`, which is a lower-level utility, `adduser` is designed to simplify account creation.

**Modifying Users with usermod**

The `usermod` command modifies existing user accounts.

Basic syntax:

```bash
sudo usermod [options] username
```

To change a username:

```bash
sudo usermod -l newname alice
```

This changes the login name from `alice` to `newname`.

To change the user's home directory:

```bash
sudo usermod -d /home/newname alice
```

This updates the configured home directory but does not automatically move the existing files.

To move the existing home directory and update the account:

```bash
sudo usermod -d /home/newname -m alice
```

To change the user's login shell:

```bash
sudo usermod -s /bin/zsh alice
```

To change the user's primary group:

```bash
sudo usermod -g developers alice
```

To add supplementary group membership without removing existing supplementary groups:

```bash
sudo usermod -aG docker alice
```

The `-a` option means append. It should normally be used together with `-G` when adding a user to supplementary groups.

**Important:** Using `-G` without `-a` replaces the user's existing supplementary group list.

For example:

```bash
sudo usermod -G developers alice
```

This replaces the existing supplementary groups with `developers`.

To lock a user account:

```bash
sudo usermod -L alice
```

To unlock a user account:

```bash
sudo usermod -U alice
```

These commands lock or unlock password-based authentication. They do not necessarily prevent every possible form of access, such as SSH key authentication or existing sessions.

To set an account expiration date:

```bash
sudo usermod -e 2026-12-31 alice
```

To remove the account expiration date:

```bash
sudo usermod -e "" alice
```

**Deleting Users with userdel**

The `userdel` command removes a user account.

Basic syntax:

```bash
sudo userdel alice
```

This deletes the account but normally leaves the user's home directory and files behind.

To delete the account and its home directory:

```bash
sudo userdel -r alice
```

The `-r` option also removes the user's home directory and mail spool when applicable.

Before deleting an account, consider:

- Whether the user owns important files.
- Whether the user has running processes.
- Whether other services depend on the account.
- Whether the home directory contains data that must be preserved.
- Whether files owned by the user's UID exist outside the home directory.

You can locate files owned by a user with:

```bash
sudo find / -xdev -user alice 2>/dev/null
```

The `-xdev` option prevents `find` from crossing filesystem boundaries.

**Understanding User Account Defaults**

The `useradd` command uses system configuration files to determine default account settings.

Important files include:

| File | Purpose |
|---|---|
| `/etc/default/useradd` | Contains default settings used by `useradd` on many Linux distributions. |
| `/etc/login.defs` | Defines account-related defaults, UID/GID ranges, password policies, and other settings. |
| `/etc/skel/` | Contains default files copied into newly created home directories. |

You can inspect the default `useradd` configuration using:

```bash
useradd -D
```

To inspect the skeleton directory:

```bash
ls -la /etc/skel/
```

Files in `/etc/skel/` are commonly copied into a newly created user's home directory when the account is created with home directory creation enabled.

### Managing Groups

Groups make it easier to manage permissions for multiple users.

Instead of assigning permissions individually to every user, administrators can create groups and grant access to shared resources through group ownership and permissions.

Linux provides several commands for managing groups:

- `groupadd` — Create a new group.
- `groupmod` — Modify an existing group.
- `groupdel` — Delete a group.
- `gpasswd` — Manage group membership and group administration.

**Creating Groups with groupadd**

The `groupadd` command creates a new group.

Basic syntax:

```bash
sudo groupadd groupname
```

Example:

```bash
sudo groupadd developers
```

This creates a group named `developers`.

To specify a GID:

```bash
sudo groupadd -g 2000 developers
```

To create a system group:

```bash
sudo groupadd -r servicegroup
```

**Adding Users to Groups**

Users can belong to multiple supplementary groups.

To add a user to a supplementary group:

```bash
sudo usermod -aG developers alice
```

To add a user to multiple supplementary groups:

```bash
sudo usermod -aG developers,docker alice
```

To verify group membership:

```bash
groups alice
```

Or:

```bash
id alice
```

Group membership changes generally take effect in new login sessions. Existing sessions may retain their previous supplementary group list.

You can start a new shell with a refreshed group membership using:

```bash
newgrp developers
```

**Removing Users from Groups**

To remove a user from a supplementary group, you can use:

```bash
sudo gpasswd -d alice developers
```

This removes `alice` from the `developers` group without deleting the user account.

You can also modify the supplementary group list using `usermod -G`, but remember that this replaces the existing list unless you explicitly include all required groups.

**Modifying Groups with groupmod**

The `groupmod` command modifies existing groups.

To rename a group:

```bash
sudo groupmod -n newdevelopers developers
```

To change a group's GID:

```bash
sudo groupmod -g 2500 developers
```

Changing a GID does not automatically update the group ownership of existing files throughout the filesystem.

You may need to locate files using the old GID and update their ownership.

**Deleting Groups with groupdel**

The `groupdel` command deletes a group.

Example:

```bash
sudo groupdel developers
```

A group generally cannot be deleted if it is the primary group of an existing user.

Before deleting a group, check whether users or files depend on it.

You can find files belonging to a group with:

```bash
sudo find / -xdev -group developers 2>/dev/null
```

**Managing Groups with gpasswd**

The `gpasswd` command provides additional group administration functionality.

To add a user to a group:

```bash
sudo gpasswd -a alice developers
```

To remove a user from a group:

```bash
sudo gpasswd -d alice developers
```

To assign a group administrator:

```bash
sudo gpasswd -A alice developers
```

To set or manage a group password:

```bash
sudo gpasswd developers
```

Group passwords are rarely used in modern Linux administration. Supplementary group membership is generally a more practical way to grant access.

### Understanding User and Group Information

Linux stores user and group information in several system files.

Understanding these files is important for system administration, troubleshooting, and security auditing.

**The /etc/passwd File**

The `/etc/passwd` file contains basic information about user accounts.

You can inspect it using:

```bash
cat /etc/passwd
```

Each line represents a user account and contains seven colon-separated fields.

Example:

```text
alice:x:1001:1001:Alice:/home/alice:/bin/bash
```

The fields are:

| Field | Description |
|---|---|
| Username | The account's login name. |
| Password Placeholder | Usually `x`, indicating that password information is stored in `/etc/shadow`. |
| UID | The user's numerical identifier. |
| GID | The user's primary group identifier. |
| GECOS | Additional descriptive information about the account. |
| Home Directory | The user's home directory. |
| Login Shell | The shell used for interactive login. |

The password field normally contains `x` rather than the actual password hash because authentication information is stored separately.

You can search for a specific user:

```bash
grep '^alice:' /etc/passwd
```

**The /etc/shadow File**

The `/etc/shadow` file stores password hashes and password-aging information.

You can inspect it with administrative privileges:

```bash
sudo cat /etc/shadow
```

A typical entry looks like:

```text
alice:$y$j9T$example-hash:20000:0:99999:7:::
```

The fields include:

1. Username.
2. Encrypted password hash or account password status.
3. Last password change date.
4. Minimum password age.
5. Maximum password age.
6. Warning period before password expiration.
7. Inactive period after password expiration.
8. Account expiration date.
9. Reserved field.

The password field may contain special values:

- `!` or `!!` — Common indicators that password authentication is locked or no usable password hash is set.
- `*` — A value that cannot be used as a normal Unix password hash.
- A password hash — The stored password verifier.

The exact meaning of account status should be interpreted alongside the authentication configuration.

**Important:** Never share `/etc/shadow` contents publicly. It contains sensitive authentication data.

**The /etc/group File**

The `/etc/group` file stores local group information.

Inspect it using:

```bash
cat /etc/group
```

Example:

```text
developers:x:2000:alice,bob
```

Each entry contains four fields:

| Field | Description |
|---|---|
| Group Name | The name of the group. |
| Password Placeholder | Usually `x`. |
| GID | The group's numerical identifier. |
| Members | A comma-separated list of supplementary group members. |

The primary group of a user is recorded by GID in `/etc/passwd`. Therefore, a user does not necessarily appear in the member list of their primary group in `/etc/group`.

**The /etc/gshadow File**

The `/etc/gshadow` file contains secure group information, including group administrators and membership details.

Inspect it with:

```bash
sudo cat /etc/gshadow
```

Its entries contain:

- Group name.
- Group password information.
- Group administrators.
- Group members.

Like `/etc/shadow`, this file should be protected from unauthorized access.

**Using id**

The `id` command displays user and group identity information.

To display the current user's identity:

```bash
id
```

To inspect another user:

```bash
id alice
```

To display only the UID:

```bash
id -u alice
```

To display only the primary GID:

```bash
id -g alice
```

To display group names:

```bash
id -Gn alice
```

To display numerical group IDs:

```bash
id -G alice
```

**Using who and w**

The `who` command displays users currently logged in to the system.

```bash
who
```

The `w` command provides additional information about logged-in users and their activity.

```bash
w
```

Typical information includes:

- Username.
- Login terminal.
- Login time.
- Remote host, when available.
- Idle time.
- Current process or command.

**Using getent**

The `getent` command retrieves account and group information through the system's Name Service Switch (NSS) configuration.

This is important because user and group information may come from sources other than local files, such as LDAP or other directory services.

To retrieve a user account:

```bash
getent passwd alice
```

To retrieve a group:

```bash
getent group developers
```

To retrieve a group by GID:

```bash
getent group 2000
```

Unlike directly searching `/etc/passwd` or `/etc/group`, `getent` can query configured account databases.

### Managing User Passwords

Passwords are an important part of Linux authentication.

Linux provides several commands for setting, changing, locking, and managing user passwords.

The most commonly used commands are:

- `passwd` — Set or change passwords.
- `chage` — Manage password expiration and aging.
- `usermod` — Lock or unlock password-based authentication.

**Setting a User Password**

When a new user is created with `useradd`, a usable password is not necessarily configured.

To set a password for a user:

```bash
sudo passwd alice
```

The command prompts you to enter and confirm the new password.

To change your own password:

```bash
passwd
```

A regular user normally cannot change another user's password without administrative privileges.

**Locking Password Authentication**

To lock a user's password:

```bash
sudo passwd -l alice
```

This prevents the account from authenticating using its existing Unix password.

To unlock the password:

```bash
sudo passwd -u alice
```

You can also use:

```bash
sudo passwd -S alice
```

This displays the password status of the account.

Common status indicators include:

- `P` — A usable password is set.
- `L` — The password is locked.
- `NP` — No password is set.

Locking a password does not necessarily disable every authentication method. SSH keys, existing sessions, or other authentication mechanisms may still allow access.

**Managing Password Expiration with chage**

The `chage` command manages password aging and account expiration settings.

To display the current password aging information:

```bash
sudo chage -l alice
```

To set the minimum number of days before a password can be changed:

```bash
sudo chage -m 1 alice
```

To set the maximum password age:

```bash
sudo chage -M 90 alice
```

To set the number of warning days before password expiration:

```bash
sudo chage -W 7 alice
```

To set the inactive period after password expiration:

```bash
sudo chage -I 30 alice
```

To force a password change at the next login:

```bash
sudo chage -d 0 alice
```

To set an account expiration date:

```bash
sudo chage -E 2026-12-31 alice
```

To remove the account expiration date:

```bash
sudo chage -E -1 alice
```

Important `chage` options:

| Option | Description |
|---|---|
| `-l` | Display password aging information. |
| `-m` | Set the minimum password age. |
| `-M` | Set the maximum password age. |
| `-W` | Set the warning period. |
| `-I` | Set the inactive period after password expiration. |
| `-d` | Set the last password change date. |
| `-E` | Set the account expiration date. |

**Understanding Password Aging**

Password aging controls how long a password remains valid and when users are required to change it.

For example:

```bash
sudo chage -m 1 -M 90 -W 7 alice
```

This configures the account so that:

- The password cannot normally be changed again for one day.
- The password expires after 90 days.
- The user receives a warning seven days before expiration.

Password expiration and account expiration are different:

- **Password Expiration:** Requires the user to change their password.
- **Account Expiration:** Prevents the account from being used after a specified date, subject to the system's authentication configuration.

**Password Security Best Practices**

When managing passwords:

- Use strong and unique passwords.
- Avoid sharing passwords between accounts.
- Do not store passwords in plain-text files.
- Avoid disabling password policies without a valid reason.
- Use SSH keys where appropriate for remote authentication.
- Review inactive and unused accounts.
- Protect `/etc/shadow` and other authentication files.
- Follow the security policy of the system or organization.

### Managing User Permissions and Access

Linux uses ownership, permissions, and access control mechanisms to determine which users can access files, directories, and system resources.

Every file and directory has an owner and an associated group.

You can inspect ownership and permissions using:

```bash
ls -l
```

Example output:

```text
-rw-r--r-- 1 alice developers 1024 Nov 25 10:00 project.txt
```

This output indicates:

- `alice` is the file owner.
- `developers` is the group owner.
- The owner has read and write permissions.
- The group has read permission.
- Other users have read permission.

**Understanding Ownership**

Linux associates each file with:

- A user owner.
- A group owner.

The owner and group are normally represented by their names when the system can resolve their numeric IDs.

To change file ownership:

```bash
sudo chown bob project.txt
```

To change both the owner and group:

```bash
sudo chown bob:developers project.txt
```

To change only the group:

```bash
sudo chgrp developers project.txt
```

To change ownership recursively:

```bash
sudo chown -R bob:developers /shared/project
```

Use recursive ownership changes carefully because they affect every file and directory below the specified path.

**Understanding File Permissions**

Linux uses three basic permission types:

| Permission | Symbol | Meaning for Files | Meaning for Directories |
|---|---|---|---|
| Read | `r` | Read file contents. | List directory entries. |
| Write | `w` | Modify file contents. | Create, delete, or rename entries, subject to other restrictions. |
| Execute | `x` | Execute a file if it is executable. | Traverse or access entries by name. |

Permissions are assigned to three categories:

- **User (u):** The file owner.
- **Group (g):** Members of the file's group.
- **Others (o):** All other users.

For example:

```text
-rwxr-xr--
```

This means:

- Owner: Read, write, and execute.
- Group: Read and execute.
- Others: Read only.

**Changing Permissions with chmod**

The `chmod` command modifies file and directory permissions.

Using symbolic notation:

```bash
chmod u+x script.sh
```

This adds execute permission for the owner.

```bash
chmod g-w project.txt
```

This removes write permission from the group.

```bash
chmod o-r secret.txt
```

This removes read permission from others.

```bash
chmod u=rw,g=r,o= file.txt
```

This sets exact permissions for each category.

Using numerical notation:

| Permission | Value |
|---|---|
| Read (`r`) | 4 |
| Write (`w`) | 2 |
| Execute (`x`) | 1 |

Each permission category receives a value based on the sum of its permissions.

For example:

```bash
chmod 755 script.sh
```

This sets:

- Owner: `7` = Read + Write + Execute.
- Group: `5` = Read + Execute.
- Others: `5` = Read + Execute.

Another example:

```bash
chmod 640 private.txt
```

This gives:

- Owner: Read and write.
- Group: Read only.
- Others: No permissions.

**Understanding sudo and Administrative Access**

The `sudo` command allows authorized users to execute commands with elevated privileges.

Example:

```bash
sudo systemctl restart ssh
```

The command runs with the privileges granted by the system's sudo configuration.

To execute a command as another user:

```bash
sudo -u alice whoami
```

To open a shell as another user:

```bash
sudo -u alice -s
```

To inspect your sudo permissions:

```bash
sudo -l
```

To edit the sudo configuration safely:

```bash
sudo visudo
```

The `visudo` command checks the sudoers configuration for syntax errors before saving it.

On many Linux distributions, members of the `sudo` or `wheel` group receive administrative privileges through the sudoers configuration.

The exact group name and policy depend on the distribution.

**Understanding Access Control Lists (ACLs)**

Traditional Linux permissions provide access control for the owner, group, and others.

Access Control Lists (ACLs) allow more specific permissions to be assigned to individual users and groups.

To inspect ACLs:

```bash
getfacl project.txt
```

To grant a specific user read and write access:

```bash
sudo setfacl -m u:bob:rw project.txt
```

To grant a group read and write access:

```bash
sudo setfacl -m g:developers:rw project.txt
```

To remove a user's ACL entry:

```bash
sudo setfacl -x u:bob project.txt
```

To remove all extended ACL entries:

```bash
sudo setfacl -b project.txt
```

To set a default ACL on a directory:

```bash
sudo setfacl -m d:g:developers:rwx /shared/project
```

Default ACLs can influence the permissions of newly created entries inside a directory. The final permissions are also affected by the permissions requested by the creating application and the system's permission rules.

ACL support depends on the filesystem and mount configuration.

**Understanding Shared Directories**

Groups are frequently used to manage access to shared directories.

For example, suppose several developers need access to the same project directory.

First, create a group:

```bash
sudo groupadd developers
```

Add users to the group:

```bash
sudo usermod -aG developers alice
sudo usermod -aG developers bob
```

Create the shared directory:

```bash
sudo mkdir -p /shared/project
```

Set its group ownership:

```bash
sudo chown root:developers /shared/project
```

Set appropriate permissions:

```bash
sudo chmod 2775 /shared/project
```

The leading `2` enables the setgid bit on the directory.

Newly created files and directories inside it normally inherit the directory's group ownership, making collaboration easier.

**Important:** The setgid bit helps maintain group ownership, but it does not automatically guarantee that every new file is group-writable. The creating application's permissions, umask, and default ACLs also affect the final permissions.

### Switching Between Users

Linux allows users to switch between accounts without logging out of the current session.

This is useful for administrative tasks, testing permissions, and performing operations under different user identities.

The most common commands are `su`, `sudo`, and `runuser`.

**Switching Users with su**

The `su` command allows you to switch to another user account.

Basic syntax:

```bash
su username
```

Example:

```bash
su alice
```

This starts a shell as `alice` but generally does not provide the same login environment as a full login session.

To switch users with a login shell:

```bash
su - alice
```

The `-` option initializes the target user's login environment, including relevant environment variables and working directory.

To switch to the root account:

```bash
su -
```

To return to the previous user:

```bash
exit
```

Or press:

```text
Ctrl + D
```

**Using sudo to Run Commands as Another User**

The `sudo` command can execute individual commands as another user.

To execute a command as `alice`:

```bash
sudo -u alice whoami
```

To start a shell as `alice`:

```bash
sudo -u alice -s
```

To start a login shell as `alice`:

```bash
sudo -iu alice
```

To start a root login shell:

```bash
sudo -i
```

To execute a single command as root:

```bash
sudo command
```

For example:

```bash
sudo -u alice ls -la /home/alice
```

This runs `ls` with `alice`'s identity and permissions.

**Understanding runuser**

The `runuser` command is available on many Linux distributions and is primarily intended for running commands as another user from privileged scripts or administrative contexts.

Example:

```bash
sudo runuser -u alice -- whoami
```

To run a login shell:

```bash
sudo runuser -l alice
```

Unlike `su`, `runuser` does not prompt for the target user's password. It generally requires appropriate administrative privileges.

**Comparing su, sudo, and runuser**

| Command | Purpose | Typical Usage |
|---|---|---|
| `su` | Switch to another user account. | `su - alice` |
| `sudo` | Execute commands with authorized elevated or alternate-user privileges. | `sudo -u alice whoami` |
| `runuser` | Run commands as another user, mainly in administrative contexts. | `sudo runuser -u alice -- command` |

**Important Security Considerations**

When switching between users:

- Avoid using the root account for everyday activities.
- Prefer `sudo` for individual administrative commands.
- Use `sudo -i` only when an administrative login shell is necessary.
- Verify the current identity using `whoami` or `id`.
- Exit elevated shells when administrative work is complete.
- Never assume that switching users automatically terminates processes or sessions started under another account.

**Verifying the Current User**

You can check the current username using:

```bash
whoami
```

To display detailed identity information:

```bash
id
```

To display the current user's home directory:

```bash
echo "$HOME"
```

To display the current shell:

```bash
echo "$SHELL"
```

Remember that `$SHELL` usually indicates the user's configured login shell, not necessarily the shell currently executing the command.

**User Management Summary**

| Command | Description |
|---|---|
| `useradd` | Create a new user account. |
| `adduser` | Create a user through a higher-level interactive utility. |
| `usermod` | Modify an existing user account. |
| `userdel` | Delete a user account. |
| `groupadd` | Create a new group. |
| `groupmod` | Modify an existing group. |
| `groupdel` | Delete a group. |
| `gpasswd` | Manage group membership and administration. |
| `passwd` | Set or change passwords. |
| `chage` | Manage password aging and expiration. |
| `id` | Display user and group IDs. |
| `groups` | Display group memberships. |
| `who` | Display logged-in users. |
| `w` | Display logged-in users and their activity. |
| `getent` | Retrieve account and group database entries. |
| `su` | Switch to another user. |
| `sudo` | Execute commands with elevated or alternate-user privileges. |
| `runuser` | Run commands as another user in administrative contexts. |
| `chown` | Change file ownership. |
| `chgrp` | Change group ownership. |
| `chmod` | Change file permissions. |
| `getfacl` | Display access control lists. |
| `setfacl` | Modify access control lists. |

**Best Practices for User Management**

- Create separate user accounts instead of sharing credentials.
- Use regular accounts for everyday activities.
- Grant administrative privileges only when necessary.
- Organize users into groups based on their responsibilities.
- Use supplementary groups to manage shared access.
- Set appropriate file ownership and permissions.
- Review inactive and unnecessary accounts regularly.
- Apply appropriate password and account expiration policies.
- Protect sensitive authentication files such as `/etc/shadow`.
- Verify account and group changes before applying them to production systems.
- Avoid deleting accounts or groups before checking their files and dependencies.
- Use `sudo` and its configuration carefully to limit administrative access.

**Additional Resources**

- [Linux man-pages — useradd](https://man7.org/linux/man-pages/man8/useradd.8.html)
- [Linux man-pages — usermod](https://man7.org/linux/man-pages/man8/usermod.8.html)
- [Linux man-pages — userdel](https://man7.org/linux/man-pages/man8/userdel.8.html)
- [Linux man-pages — groupadd](https://man7.org/linux/man-pages/man8/groupadd.8.html)
- [Linux man-pages — passwd](https://man7.org/linux/man-pages/man1/passwd.1.html)
- [Linux man-pages — chage](https://man7.org/linux/man-pages/man1/chage.1.html)
- [Linux man-pages — sudo](https://man7.org/linux/man-pages/man8/sudo.8.html)
- [Linux man-pages — su](https://man7.org/linux/man-pages/man1/su.1.html)
- [Linux man-pages — credentials](https://man7.org/linux/man-pages/man7/credentials.7.html)
- [Linux man-pages — user_namespaces](https://man7.org/linux/man-pages/man7/user_namespaces.7.html)

---

## Practical Exercises

The following exercises are designed to reinforce the concepts and commands introduced in the User Management section. They provide hands-on experience in creating and managing user accounts, organizing groups, managing passwords, controlling permissions, and switching between users.

### Exercise 1 — Understanding Users and Groups

1. Display the username of the currently logged-in user using `whoami`.
2. Display detailed user and group information using `id`.
3. Display the UID of the current user using `id -u`.
4. Display the primary GID using `id -g`.
5. List all groups associated with the current user using `groups`.
6. Display all users currently logged in using `who`.
7. Display additional information about logged-in users using `w`.
8. Inspect the `/etc/passwd` file.
9. Inspect the `/etc/group` file.
10. Use `getent passwd` to retrieve information about a specific user.

### Exercise 2 — Creating and Managing User Accounts

1. Create a new user named `alice` using `useradd`.
2. Create another user named `bob` with a home directory.
3. Set a login shell of `/bin/bash` for both users.
4. Assign passwords to both accounts using `passwd`.
5. Verify that both users exist using `getent passwd`.
6. Display their UIDs and GIDs using `id`.
7. Inspect their home directories using `ls -la`.
8. Change `bob`'s login shell to `/bin/sh` using `usermod`.
9. Change `alice`'s descriptive information using `usermod -c`.
10. Delete the `bob` account using `userdel -r`.

### Exercise 3 — Managing Groups

1. Create a group named `developers`.
2. Create another group named `testers`.
3. Add `alice` to the `developers` group.
4. Add `alice` to the `testers` group without removing existing supplementary groups.
5. Verify the group memberships using `groups alice`.
6. Display detailed group information using `id alice`.
7. Create a user named `charlie` with `developers` as the primary group.
8. Add `charlie` to the `testers` group.
9. Remove `alice` from the `testers` group using `gpasswd`.
10. Rename the `testers` group to `qa-team`.
11. Inspect the `/etc/group` file to verify your changes.

### Exercise 4 — Understanding User and Group Information

1. Display the contents of `/etc/passwd`.
2. Find the entry belonging to `alice` using `grep`.
3. Identify the username, UID, GID, home directory, and login shell.
4. Use `getent passwd alice` to retrieve the account information.
5. Display the group information using `getent group developers`.
6. Display the numerical UID and GID of `alice`.
7. Inspect `/etc/shadow` using administrative privileges.
8. Use `getent group` to retrieve information about all configured groups.
9. Compare the output of `groups alice` and `id alice`.
10. Explain the difference between `/etc/passwd`, `/etc/shadow`, and `/etc/group`.

### Exercise 5 — Managing User Passwords

1. Set a password for `alice` using `passwd`.
2. Change the password of `alice` again.
3. Display the password status using `passwd -S alice`.
4. Display password aging information using `chage -l alice`.
5. Set the minimum password age to one day.
6. Set the maximum password age to 90 days.
7. Configure a warning period of seven days.
8. Force `alice` to change her password at the next login using `chage -d 0`.
9. Lock password authentication for `alice`.
10. Verify the password status.
11. Unlock password authentication and verify the result.

### Exercise 6 — Managing User Permissions and Access

1. Create a directory named `/shared/project`.
2. Create a file named `project.txt` inside the directory.
3. Change the file owner to `alice`.
4. Change the file's group ownership to `developers`.
5. Set the file permissions to `640`.
6. Verify the permissions using `ls -l`.
7. Create a script named `deploy.sh`.
8. Add execute permission for the owner using `chmod`.
9. Create a shared directory owned by `root:developers`.
10. Set its permissions to `2775`.
11. Explain how the setgid bit affects group ownership of newly created files and directories.
12. Use `getfacl` to inspect the permissions of the shared directory.
13. Grant a specific user read and write access using `setfacl`.
14. Verify the ACL changes using `getfacl`.
15. Remove the additional ACL entry using `setfacl -x`.

### Exercise 7 — Switching Between Users

1. Switch to `alice` using `su`.
2. Verify the current identity using `whoami`.
3. Display the current user's UID and group memberships using `id`.
4. Return to the original account using `exit`.
5. Switch to `alice` using `su - alice`.
6. Compare the working directory and environment variables with the previous session.
7. Execute a command as `alice` using `sudo -u alice`.
8. Start a login shell as `alice` using `sudo -iu alice`.
9. Start a root login shell using `sudo -i`.
10. Return to your original account and verify your identity.

### Exercise 8 — User Management with Shell Commands

Create a script named `user-manager.sh` that performs basic user and group information checks.

The script should:

1. Accept a username as a command-line argument.
2. Check whether the username was provided.
3. Verify that the user exists using `id`.
4. Display the username, UID, primary GID, and supplementary groups.
5. Display the user's home directory and login shell.
6. Check whether the home directory exists.
7. Display the groups associated with the user.
8. Handle invalid usernames and missing arguments.
9. Display clear messages for each operation.

Example:

```bash
./user-manager.sh alice
```

Expected output:

```text
User Information
----------------
Username: alice
UID: 1001
Primary GID: 1001
Groups: alice developers
Home Directory: /home/alice
Login Shell: /bin/bash

Account Status: User exists
Home Directory Status: Available
```

### Exercise 9 — Final Challenge: Linux User and Group Administration

Create a small user and group management environment for a development team.

Your task is to configure the following accounts and groups:

**Users:**

- `alice`
- `bob`
- `charlie`

**Groups:**

- `developers`
- `testers`
- `administrators`

Complete the following tasks:

1. Create all three groups.
2. Create all three user accounts with home directories.
3. Set `/bin/bash` as the login shell for each user.
4. Assign a password to every account.
5. Add `alice` and `bob` to the `developers` group.
6. Add `bob` and `charlie` to the `testers` group.
7. Configure `charlie` as a member of the `administrators` group.
8. Create `/shared/development` and assign ownership to `root:developers`.
9. Configure the directory permissions to `2775`.
10. Create a file named `README.txt` inside the shared directory.
11. Verify the ownership and permissions of the file.
12. Configure password aging for `alice` with a maximum password age of 90 days.
13. Verify all user and group memberships.
14. Test access to the shared directory using the different user accounts.
15. Document the commands used and their results.

**Additional Requirements:**

- Use `id` and `getent` to verify account information.
- Use `getfacl` and `setfacl` to demonstrate additional access control.
- Verify that new files inherit the expected group ownership.
- Explain the difference between primary and supplementary groups.
- Identify which users can access the shared directory and why.

### Exercise 10 — Knowledge Check

Answer the following questions to test your understanding of Linux user management.

1. What is the difference between a regular user and the root user?
2. What is a UID?
3. What is a GID?
4. What is the difference between primary and supplementary groups?
5. Which command creates a new user account?
6. What is the purpose of the `-m` option in `useradd`?
7. What is the difference between `useradd` and `adduser`?
8. Which command modifies an existing user account?
9. What is the difference between `userdel` and `userdel -r`?
10. Which command creates a new group?
11. What does the `-aG` option do in `usermod`?
12. What is the purpose of `/etc/passwd`?
13. What information is stored in `/etc/shadow`?
14. Which command displays a user's UID and group memberships?
15. What is the purpose of the `chage` command?
16. How can you lock and unlock password authentication for a user?
17. What is the difference between `su` and `sudo`?
18. What does the setgid bit do on a directory?
19. What is the purpose of `getfacl` and `setfacl`?
20. Why should administrative privileges be granted carefully?

---

**Goal:** Develop practical skills in creating and managing Linux user accounts, organizing groups, controlling authentication and password policies, managing file access, and switching between users securely.

<p align="right">
  <a href="../Day-07/README.md">← Day 07</a>
  &nbsp; | &nbsp;
  <a href="../Day-09/">Day 09 →</a>
  &nbsp; | &nbsp;
  <a href="#day-08--user-management">Back to top ↑</a>
</p>
