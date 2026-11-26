<p align="center">
  <img src="../../media/images/Linux/Day-09/image_01.png" alt="image_01">
</p>

# Day 09 — Service Management (systemd)

## Table of Contents

- [Day 09 — Service Management (systemd)](#day-09--service-management-systemd)
  - [Table of Contents](#table-of-contents)
  - [Service Management](#service-management)
    - [Understanding systemd and Services](#understanding-systemd-and-services)
    - [Managing Services with systemctl](#managing-services-with-systemctl)
    - [Checking Service Status](#checking-service-status)
    - [Enabling and Disabling Services](#enabling-and-disabling-services)
    - [Creating and Managing Service Units](#creating-and-managing-service-units)
    - [Monitoring Service Logs](#monitoring-service-logs)
    - [Troubleshooting Services](#troubleshooting-services)
  - [Practical Exercises](#practical-exercises)
    - [Exercise 1 — Understanding systemd and Services](#exercise-1--understanding-systemd-and-services)
    - [Exercise 2 — Managing Services with systemctl](#exercise-2--managing-services-with-systemctl)
    - [Exercise 3 — Checking Service Status](#exercise-3--checking-service-status)
    - [Exercise 4 — Enabling and Disabling Services](#exercise-4--enabling-and-disabling-services)
    - [Exercise 5 — Creating and Managing Service Units](#exercise-5--creating-and-managing-service-units)
    - [Exercise 6 — Monitoring Service Logs](#exercise-6--monitoring-service-logs)
    - [Exercise 7 — Troubleshooting Services](#exercise-7--troubleshooting-services)
    - [Exercise 8 — Service Management with Shell Scripts](#exercise-8--service-management-with-shell-scripts)
    - [Exercise 9 — Final Challenge: Linux Service Administration](#exercise-9--final-challenge-linux-service-administration)
    - [Exercise 10 — Knowledge Check](#exercise-10--knowledge-check)

---

## Service Management

Service management is an important part of Linux system administration. Services are background processes that provide functionality to the operating system and other applications.

Examples include:

- SSH services for remote system access.
- Web servers such as Apache and Nginx.
- Database servers such as MySQL and PostgreSQL.
- Network management services.
- System logging services.
- Scheduling services such as cron.

Linux uses service management systems to start, stop, monitor, and control these processes. On most modern Linux distributions, `systemd` is the primary service manager.

Understanding service management allows administrators to control system behavior, configure services to start automatically, monitor service activity, and troubleshoot operational problems.

### Understanding systemd and Services

**What Is systemd?**

`systemd` is a system and service manager used by many modern Linux distributions. It is responsible for initializing the system during boot and managing services and other system resources throughout the system's runtime.

It generally runs as the first userspace process after the Linux kernel initializes the system.

You can inspect the process using:

```bash
ps -p 1 -o pid,ppid,comm,args
```

On a typical system using systemd, the output identifies PID 1 as `systemd`.

Some of its main responsibilities include:

- Starting system services during boot.
- Managing service dependencies and startup order.
- Monitoring and restarting services according to their configuration.
- Managing system targets and boot states.
- Managing sockets, timers, mounts, and other system resources.
- Collecting service logs through the system journal.
- Providing tools for controlling and inspecting system services.

**Understanding Services**

A service is a background process that performs a specific task without requiring continuous interaction from the user.

For example, an SSH server runs in the background and waits for incoming remote connections.

Services can be started manually or automatically during system startup.

Common examples include:

| Service | Description |
|---|---|
| `ssh` or `sshd` | Provides secure remote access. |
| `nginx` | Provides web server functionality. |
| `apache2` or `httpd` | Runs the Apache web server. |
| `cron` or `crond` | Executes scheduled tasks. |
| `rsyslog` | Collects and manages system logs. |
| `NetworkManager` | Manages network connections. |
| `systemd-journald` | Collects and stores system journal messages. |

Service names may differ between Linux distributions.

**Understanding systemd Units**

systemd manages system resources through objects called units.

A unit describes a resource or task that systemd can manage.

Each unit type has a specific purpose and uses a corresponding file extension.

| Unit Type | Extension | Description |
|---|---|---|
| Service | `.service` | Manages system services and processes. |
| Socket | `.socket` | Manages communication sockets and socket activation. |
| Target | `.target` | Groups units and represents system states. |
| Mount | `.mount` | Manages filesystem mount points. |
| Automount | `.automount` | Manages automatic filesystem mounting. |
| Timer | `.timer` | Schedules tasks and activates other units. |
| Path | `.path` | Monitors filesystem paths and activates units. |
| Device | `.device` | Represents devices managed by the kernel and device manager. |
| Swap | `.swap` | Manages swap devices and files. |
| Slice | `.slice` | Groups processes for resource management. |
| Scope | `.scope` | Represents externally created process groups managed by systemd. |

For example:

```bash
systemctl status ssh.service
```

This command displays the status of the SSH service unit.

The `.service` extension can often be omitted when using `systemctl`:

```bash
systemctl status ssh
```

**Understanding the systemd Boot Process**

During startup, the Linux kernel initializes the system and starts the first userspace process.

On a system using systemd, the general startup sequence is:

1. The firmware initializes the hardware.
2. The bootloader loads the Linux kernel.
3. The kernel initializes hardware and essential kernel components.
4. The kernel starts PID 1, which is normally systemd.
5. systemd activates the default target and its dependencies.
6. Required services and other units are started.
7. The system reaches its configured operational state.

You can inspect the default target using:

```bash
systemctl get-default
```

Common system targets include:

| Target | Description |
|---|---|
| `default.target` | The default target selected during system startup. |
| `multi-user.target` | Provides a multi-user, non-graphical environment. |
| `graphical.target` | Provides a graphical login environment. |
| `rescue.target` | Starts a basic rescue environment. |
| `emergency.target` | Starts a minimal emergency environment. |
| `poweroff.target` | Shuts down the system. |
| `reboot.target` | Reboots the system. |

To change the default target:

```bash
sudo systemctl set-default multi-user.target
```

To restore a graphical default target:

```bash
sudo systemctl set-default graphical.target
```

To display the currently active targets:

```bash
systemctl list-units --type=target
```

Changing the default target affects future startups. It does not necessarily change the current session immediately.

**Understanding Service Dependencies**

Services often depend on other services or system resources.

For example, a web application might depend on a database service and network connectivity.

systemd uses dependency relationships to determine which units should be started or stopped together.

Common dependency directives include:

| Directive | Description |
|---|---|
| `Requires=` | Declares required units. |
| `Wants=` | Declares recommended units that are started alongside the current unit. |
| `After=` | Controls ordering after another unit. |
| `Before=` | Controls ordering before another unit. |
| `Conflicts=` | Declares units that cannot be active together. |
| `PartOf=` | Propagates certain stop and restart operations from another unit. |

**Important:** `After=` and `Before=` control ordering, not whether another unit is automatically started. Dependencies such as `Requires=` and `Wants=` establish activation relationships.

For example:

```ini
[Unit]
Description=Example Web Application
Requires=network.target
After=network.target
```

This configuration declares a dependency on `network.target` and specifies that the application should start after it.

**Understanding Service States**

systemd tracks the operational state of services.

Common service states include:

| State | Description |
|---|---|
| `active` | The unit is active. |
| `inactive` | The unit is currently inactive. |
| `failed` | The unit failed during activation or execution. |
| `activating` | The unit is in the process of starting. |
| `deactivating` | The unit is in the process of stopping. |
| `reloading` | The unit is reloading its configuration. |

A service can be `active` without continuously running a process. For example, a successful `Type=oneshot` service can become active after completing its task if configured with `RemainAfterExit=yes`.

**Understanding Unit File Locations**

systemd reads unit files from several locations.

Common system unit file locations include:

| Directory | Purpose |
|---|---|
| `/usr/lib/systemd/system/` | Unit files installed by packages on many distributions. |
| `/lib/systemd/system/` | Unit files installed by packages on some distributions. |
| `/etc/systemd/system/` | Administrator-managed unit files and configuration overrides. |
| `/run/systemd/system/` | Runtime unit files and temporary configuration. |

The exact directory layout depends on the distribution. On some systems, `/lib/systemd/system` is linked to `/usr/lib/systemd/system`.

To inspect the directories used by systemd:

```bash
systemd-analyze unit-paths
```

To display the unit file associated with a service:

```bash
systemctl cat ssh.service
```

Administrator-managed unit files and drop-in configurations generally take precedence over vendor-provided settings.

### Managing Services with systemctl

`systemctl` is the primary command-line utility for communicating with the systemd service manager.

It allows administrators to start, stop, restart, reload, inspect, and configure services.

Basic syntax:

```bash
systemctl [OPTIONS] COMMAND [UNIT]
```

Administrative operations generally require `sudo`.

**Starting Services**

The `start` command activates a service immediately.

Syntax:

```bash
sudo systemctl start service-name
```

Example:

```bash
sudo systemctl start ssh
```

This starts the SSH service.

Starting a service does not automatically configure it to start during future system boots.

**Stopping Services**

The `stop` command deactivates a running service.

Syntax:

```bash
sudo systemctl stop service-name
```

Example:

```bash
sudo systemctl stop nginx
```

This stops the Nginx service.

Stopping a service may interrupt applications or users that depend on it.

**Restarting Services**

The `restart` command stops and starts a service again.

Syntax:

```bash
sudo systemctl restart service-name
```

Example:

```bash
sudo systemctl restart ssh
```

Restarting is useful when a service needs to be restarted after a configuration change or operational problem.

**Reloading Services**

Some services support reloading their configuration without completely stopping the running process.

Syntax:

```bash
sudo systemctl reload service-name
```

Example:

```bash
sudo systemctl reload nginx
```

A reload is generally less disruptive than a restart because the service can continue running while applying updated configuration.

However, not every service supports reloading.

You can check whether a service supports reload operations by inspecting its unit configuration or documentation.

**Restarting or Reloading When Supported**

The `reload-or-restart` command attempts to reload a service if it supports reloading. Otherwise, it restarts the service.

```bash
sudo systemctl reload-or-restart nginx
```

The `try-restart` command restarts a service only if it is already running:

```bash
sudo systemctl try-restart nginx
```

**Listing Services**

To display currently loaded service units:

```bash
systemctl list-units --type=service
```

To display only active services:

```bash
systemctl list-units --type=service --state=active
```

To display inactive services:

```bash
systemctl list-units --type=service --state=inactive
```

To display failed services:

```bash
systemctl --failed
```

To include inactive units in the listing:

```bash
systemctl list-units --type=service --all
```

**Inspecting Unit Configuration**

The `systemctl cat` command displays the unit file and its drop-in configuration files.

```bash
systemctl cat nginx.service
```

The `systemctl show` command displays properties of a unit.

```bash
systemctl show nginx.service
```

To display a specific property:

```bash
systemctl show nginx.service -p ActiveState
```

To inspect the main process ID:

```bash
systemctl show nginx.service -p MainPID
```

To display the unit's loaded state:

```bash
systemctl show nginx.service -p LoadState
```

**Checking Dependencies**

To display the dependencies of a service:

```bash
systemctl list-dependencies nginx.service
```

To display reverse dependencies:

```bash
systemctl list-dependencies --reverse nginx.service
```

Reverse dependencies show which units depend on the specified unit.

**Masking and Unmasking Services**

Sometimes administrators need to prevent a service from being started, either manually or automatically.

The `mask` command makes a unit impossible to activate through normal systemd activation mechanisms by linking it to `/dev/null`.

```bash
sudo systemctl mask nginx.service
```

To remove the mask:

```bash
sudo systemctl unmask nginx.service
```

Masking is stronger than disabling a service.

- `disable` removes automatic startup links.
- `mask` prevents normal activation of the unit.

Use masking carefully because other services may depend on the masked unit.

### Checking Service Status

Monitoring service status is essential for identifying operational problems and verifying that services are working correctly.

systemd provides several commands for checking service activity, configuration, and process information.

**Using systemctl status**

The `status` command displays detailed information about a service.

Syntax:

```bash
systemctl status service-name
```

Example:

```bash
systemctl status ssh
```

Typical output:

```text
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/usr/lib/systemd/system/ssh.service; enabled)
     Active: active (running) since Wed 2026-11-25 10:00:00
   Main PID: 1234 (sshd)
      Tasks: 1
     Memory: 2.5M
        CPU: 120ms
     CGroup: /system.slice/ssh.service
             └─1234 /usr/sbin/sshd -D
```

The output contains several important fields:

| Field | Description |
|---|---|
| Unit Name | The name and description of the service. |
| Loaded | Indicates whether the unit file was loaded and its configuration status. |
| Active | Shows the current operational state. |
| Main PID | The process ID of the main service process, when applicable. |
| Tasks | The number of tasks associated with the service. |
| Memory | The memory currently attributed to the service. |
| CPU | CPU time consumed by the service. |
| CGroup | The control group associated with the service. |

The exact output depends on the service and systemd version.

**Checking Whether a Service Is Active**

To check whether a service is active:

```bash
systemctl is-active nginx
```

Possible results include:

```text
active
inactive
failed
```

This command is useful in shell scripts because its exit status can be used in conditional statements.

Example:

```bash
if systemctl is-active --quiet nginx; then
    echo "Nginx is running."
else
    echo "Nginx is not running."
fi
```

**Checking Whether a Service Is Enabled**

To check whether a service is configured to start automatically:

```bash
systemctl is-enabled nginx
```

Possible results include:

- `enabled`
- `disabled`
- `static`
- `masked`
- `indirect`
- `generated`
- `alias`

An enabled service is configured for automatic activation through its associated installation links.

A static service does not have its own normal enablement configuration, often because it is activated through another unit.

**Understanding Active and Enabled**

Active and enabled describe different aspects of a service.

| Status | Meaning |
|---|---|
| Active | The service is currently running or otherwise active. |
| Inactive | The service is currently not active. |
| Enabled | The service is configured for automatic activation. |
| Disabled | The service is not configured for automatic activation through its enablement links. |
| Failed | The service has encountered an activation or execution failure. |

A service can be:

- Active and enabled.
- Active and disabled.
- Inactive and enabled.
- Inactive and disabled.

For example, manually starting a disabled service makes it active without enabling automatic startup.

**Inspecting Service Processes**

You can inspect the processes associated with a service using `systemctl status` or other process monitoring tools.

To display the main process ID:

```bash
systemctl show nginx.service -p MainPID
```

To inspect a specific process:

```bash
ps -p 1234 -o pid,ppid,user,stat,cmd
```

Replace `1234` with the actual PID.

To display processes belonging to a service's control group:

```bash
systemd-cgls /system.slice/nginx.service
```

The exact control group path can be obtained from the service status or its properties.

**Inspecting System-Wide Service Failures**

To list failed units:

```bash
systemctl --failed
```

To display failed service units only:

```bash
systemctl --failed --type=service
```

To reset the recorded failed state of a service after investigating and correcting the problem:

```bash
sudo systemctl reset-failed nginx.service
```

This clears the recorded failure state. It does not fix the underlying problem or automatically start the service.

### Enabling and Disabling Services

Linux services can be configured to start automatically during system startup or remain inactive until manually or indirectly activated.

systemd uses the `enable` and `disable` commands to manage automatic service activation.

**Enabling Services**

The `enable` command configures a service for automatic activation according to its unit installation settings.

Syntax:

```bash
sudo systemctl enable service-name
```

Example:

```bash
sudo systemctl enable nginx
```

This configures Nginx to start automatically when its associated target is reached during system startup.

Enabling a service does not necessarily start it immediately.

**Enabling and Starting a Service Together**

To enable a service and start it immediately:

```bash
sudo systemctl enable --now nginx
```

This performs both operations.

**Disabling Services**

The `disable` command removes the enablement links associated with a service.

Syntax:

```bash
sudo systemctl disable service-name
```

Example:

```bash
sudo systemctl disable nginx
```

Disabling a service does not necessarily stop it if it is currently running.

**Disabling and Stopping a Service Together**

To disable a service and stop it immediately:

```bash
sudo systemctl disable --now nginx
```

**Preventing Automatic and Manual Activation**

If a service must not be started through normal systemd activation mechanisms, use `mask`.

```bash
sudo systemctl mask nginx
```

To reverse this operation:

```bash
sudo systemctl unmask nginx
```

**Understanding Enablement Links**

When a service is enabled, systemd commonly creates symbolic links in target-specific directories.

For example:

```text
/etc/systemd/system/multi-user.target.wants/
```

These links indicate that a service should be activated when the corresponding target is reached.

You can inspect a service's enablement configuration using:

```bash
systemctl is-enabled nginx
```

You can also inspect the unit's installation section:

```bash
systemctl cat nginx.service
```

Look for the `[Install]` section, which may contain directives such as:

```ini
[Install]
WantedBy=multi-user.target
```

This indicates that enabling the service associates it with `multi-user.target`.

**Understanding Runtime Enablement**

systemd also supports runtime-only enablement.

To enable a service for the current boot without permanently configuring it:

```bash
sudo systemctl enable --runtime nginx
```

To disable runtime enablement:

```bash
sudo systemctl disable --runtime nginx
```

Runtime configuration is stored under `/run` and is normally lost after reboot.

**Practical Example**

Suppose you have an Nginx server and want it to start automatically after every reboot.

Use:

```bash
sudo systemctl enable --now nginx
```

Verify that the service is active:

```bash
systemctl is-active nginx
```

Verify that it is enabled:

```bash
systemctl is-enabled nginx
```

If you no longer want it to start automatically:

```bash
sudo systemctl disable nginx
```

If you also want to stop it:

```bash
sudo systemctl disable --now nginx
```

### Creating and Managing Service Units

systemd allows administrators to create custom service units for applications, scripts, and other processes.

A service unit defines how a process should start, stop, restart, and interact with the system.

Custom service units are useful for:

- Running background applications.
- Starting custom scripts during boot.
- Managing application processes.
- Automatically restarting failed applications.
- Defining dependencies and execution environments.
- Running applications under dedicated user accounts.

**Understanding Service Unit Files**

A service unit file normally uses the `.service` extension.

A typical service unit contains several sections:

| Section | Purpose |
|---|---|
| `[Unit]` | Defines the service description and dependencies. |
| `[Service]` | Defines how the application runs and behaves. |
| `[Install]` | Defines how the service can be enabled. |

A basic service unit looks like this:

```ini
[Unit]
Description=Example Application Service
After=network.target

[Service]
Type=simple
ExecStart=/usr/local/bin/example-app

[Install]
WantedBy=multi-user.target
```

**Understanding the [Unit] Section**

The `[Unit]` section contains general information and dependency relationships.

Example:

```ini
[Unit]
Description=RootBound Example Service
Documentation=man:systemd.service(5)
After=network.target
Wants=network.target
```

Important directives include:

| Directive | Description |
|---|---|
| `Description=` | Provides a human-readable service description. |
| `Documentation=` | Specifies documentation references. |
| `After=` | Defines startup ordering. |
| `Before=` | Defines startup ordering before another unit. |
| `Requires=` | Declares required units. |
| `Wants=` | Declares recommended units. |
| `Conflicts=` | Declares incompatible units. |

**Understanding the [Service] Section**

The `[Service]` section defines how the application is executed and managed.

Example:

```ini
[Service]
Type=simple
ExecStart=/usr/local/bin/example-app
Restart=on-failure
User=appuser
Group=appgroup
WorkingDirectory=/opt/example-app
```

Important directives include:

| Directive | Description |
|---|---|
| `Type=` | Defines how systemd determines when the service has started. |
| `ExecStart=` | Specifies the command used to start the service. |
| `ExecStop=` | Specifies an optional command used to stop the service. |
| `ExecReload=` | Specifies an optional command used to reload the service. |
| `Restart=` | Defines when systemd should restart the service. |
| `User=` | Specifies the user account used to run the service. |
| `Group=` | Specifies the group used to run the service. |
| `WorkingDirectory=` | Sets the working directory. |
| `Environment=` | Defines environment variables. |
| `EnvironmentFile=` | Loads environment variables from a file. |
| `StandardOutput=` | Configures standard output handling. |
| `StandardError=` | Configures standard error handling. |

**Understanding Service Types**

The `Type=` directive determines how systemd considers a service to have started.

Common service types include:

| Type | Description |
|---|---|
| `simple` | Considers the service started when its main process is launched. |
| `exec` | Considers the service started after the configured executable has successfully been invoked. |
| `forking` | Expects the initial process to fork and exit, leaving a background process. |
| `oneshot` | Runs a short-lived command or task and waits for it to complete. |
| `notify` | Waits for the service to send a readiness notification. |
| `idle` | Delays execution until other jobs are dispatched. |

For modern services that remain in the foreground, `simple` or `exec` is commonly appropriate.

The `forking` type is generally used for applications that daemonize themselves.

The `oneshot` type is useful for tasks that perform an operation and then exit.

**Understanding Restart Policies**

The `Restart=` directive controls whether systemd automatically restarts a service.

Common values include:

| Value | Description |
|---|---|
| `no` | Does not automatically restart the service. |
| `on-success` | Restarts after a successful exit. |
| `on-failure` | Restarts after an unsuccessful exit or certain abnormal termination conditions. |
| `on-abnormal` | Restarts after abnormal termination, such as a signal or timeout. |
| `on-abort` | Restarts when the service terminates due to an uncaught signal. |
| `on-watchdog` | Restarts when a watchdog timeout occurs. |
| `always` | Restarts regardless of how the service exits, subject to systemd's restart rules and limits. |

Example:

```ini
[Service]
Restart=on-failure
RestartSec=5
```

This configures systemd to wait five seconds before attempting to restart a failed service.

Restart attempts are also subject to systemd's start-rate limiting settings.

**Creating a Custom Service**

In this example, you will create a simple background application managed by systemd.

First, create a script:

```bash
sudo nano /usr/local/bin/rootbound-demo.sh
```

Add the following content:

```bash
#!/bin/bash

while true; do
    echo "RootBound service is running..."
    sleep 30
done
```

Save the file and make it executable:

```bash
sudo chmod +x /usr/local/bin/rootbound-demo.sh
```

The script runs continuously and writes messages to standard output.

Next, create a service unit:

```bash
sudo nano /etc/systemd/system/rootbound-demo.service
```

Add the following configuration:

```ini
[Unit]
Description=RootBound Demo Service
After=network.target

[Service]
Type=exec
ExecStart=/usr/local/bin/rootbound-demo.sh
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
```

Save the file.

**Reloading systemd Configuration**

After creating or modifying a unit file, systemd must reload its unit configuration.

Run:

```bash
sudo systemctl daemon-reload
```

This tells systemd to reread unit files and update its configuration.

It does not automatically restart running services.

**Starting the Custom Service**

Start the service:

```bash
sudo systemctl start rootbound-demo.service
```

Check its status:

```bash
systemctl status rootbound-demo.service
```

To view its output:

```bash
journalctl -u rootbound-demo.service
```

**Enabling the Custom Service**

To configure it to start automatically during system startup:

```bash
sudo systemctl enable rootbound-demo.service
```

To enable and start it together:

```bash
sudo systemctl enable --now rootbound-demo.service
```

**Stopping and Removing the Custom Service**

Stop the service:

```bash
sudo systemctl stop rootbound-demo.service
```

Disable automatic startup:

```bash
sudo systemctl disable rootbound-demo.service
```

Remove the unit file:

```bash
sudo rm /etc/systemd/system/rootbound-demo.service
```

Reload systemd configuration:

```bash
sudo systemctl daemon-reload
```

If the service is no longer needed, remove its script as well:

```bash
sudo rm /usr/local/bin/rootbound-demo.sh
```

**Managing Service Overrides**

Sometimes you need to change a service's configuration without editing its original vendor-provided unit file.

systemd supports drop-in configuration files.

To create an override:

```bash
sudo systemctl edit nginx.service
```

This opens an editor for a drop-in configuration.

For example:

```ini
[Service]
Restart=on-failure
RestartSec=5
```

Save the file and reload the unit configuration:

```bash
sudo systemctl daemon-reload
```

Apply the new configuration by restarting the service when appropriate:

```bash
sudo systemctl restart nginx.service
```

To inspect the complete unit configuration, including overrides:

```bash
systemctl cat nginx.service
```

To remove administrator-created overrides:

```bash
sudo systemctl revert nginx.service
```

Use `revert` carefully because it removes administrator-created drop-ins and local overrides for the specified unit.

**Running Services as Dedicated Users**

For security reasons, applications should generally run under dedicated unprivileged user accounts instead of root whenever possible.

For example:

```ini
[Service]
User=appuser
Group=appgroup
WorkingDirectory=/opt/example-app
ExecStart=/opt/example-app/start.sh
```

This runs the application using the `appuser` account and `appgroup` group.

The account must have the necessary permissions to access the application files and resources.

**Understanding User Services**

systemd can also manage services for individual users.

System services run under the system manager, while user services run under an individual user's systemd manager.

To inspect user services:

```bash
systemctl --user list-units --type=service
```

To start a user service:

```bash
systemctl --user start example.service
```

To inspect its status:

```bash
systemctl --user status example.service
```

User service unit files are commonly stored in:

```text
~/.config/systemd/user/
```

To reload the user's systemd configuration:

```bash
systemctl --user daemon-reload
```

User services normally run within the user's login session. Persistent operation after logout may require lingering to be enabled:

```bash
sudo loginctl enable-linger alice
```

This allows the user's systemd manager to remain available without an active login session.

### Monitoring Service Logs

Logs are essential for understanding service behavior, identifying failures, and troubleshooting operational problems.

On systems using systemd, service logs are commonly collected by `systemd-journald`.

The `journalctl` command is used to inspect and filter journal entries.

**Understanding systemd-journald**

`systemd-journald` collects log messages from several sources, including:

- Kernel messages.
- System services.
- Standard output and standard error of services managed by systemd.
- System applications that send messages to the journal.
- Other supported logging sources.

Journal data may be stored persistently or temporarily depending on the system configuration.

Common journal storage locations include:

| Location | Description |
|---|---|
| `/run/log/journal/` | Runtime journal storage, usually temporary. |
| `/var/log/journal/` | Persistent journal storage, when configured. |

**Viewing System Logs**

To display all available journal entries:

```bash
journalctl
```

To display the latest entries:

```bash
journalctl -n 50
```

To display logs from the current boot:

```bash
journalctl -b
```

To display logs from the previous boot:

```bash
journalctl -b -1
```

Previous boot logs are available only if they were retained.

**Viewing Logs for a Specific Service**

Use the `-u` option to filter logs by unit.

```bash
journalctl -u nginx.service
```

To display only the latest entries:

```bash
journalctl -u nginx.service -n 50
```

To display logs from the current boot:

```bash
journalctl -u nginx.service -b
```

To display logs since a specific time:

```bash
journalctl -u nginx.service --since "2026-11-26 10:00:00"
```

To display logs within a time range:

```bash
journalctl -u nginx.service \
    --since "2026-11-26 10:00:00" \
    --until "2026-11-26 12:00:00"
```

**Following Logs in Real Time**

To monitor service logs as new messages are generated:

```bash
journalctl -u nginx.service -f
```

This is useful when:

- Starting a service.
- Restarting an application.
- Testing configuration changes.
- Investigating repeated failures.
- Monitoring application activity.

Press `Ctrl+C` to stop following the logs.

**Filtering Logs by Priority**

The journal supports priority-based filtering.

Common priority levels include:

| Priority | Name | Description |
|---|---|---|
| 0 | Emergency | The system is unusable. |
| 1 | Alert | Immediate action is required. |
| 2 | Critical | Critical conditions. |
| 3 | Error | Error conditions. |
| 4 | Warning | Warning conditions. |
| 5 | Notice | Normal but significant events. |
| 6 | Informational | Informational messages. |
| 7 | Debug | Debugging messages. |

To display error messages and more severe priorities:

```bash
journalctl -p err
```

To display warning messages and more severe priorities:

```bash
journalctl -p warning
```

To filter a specific service by priority:

```bash
journalctl -u nginx.service -p err
```

**Displaying Logs in Reverse Order**

To display the newest journal entries first:

```bash
journalctl -r
```

To display the latest entries of a service in reverse order:

```bash
journalctl -u nginx.service -r
```

**Displaying Detailed Log Information**

To display verbose information:

```bash
journalctl -o verbose
```

Other useful output formats include:

```bash
journalctl -o short
journalctl -o short-iso
journalctl -o json
```

These formats can help when inspecting timestamps, structured metadata, or processing logs.

**Checking Journal Disk Usage**

To inspect the amount of disk space used by journal files:

```bash
journalctl --disk-usage
```

To remove archived journal files older than a specified period:

```bash
sudo journalctl --vacuum-time=7d
```

To limit archived journal storage to a specific size:

```bash
sudo journalctl --vacuum-size=500M
```

These operations remove archived journal files. They do not necessarily remove currently active journal files.

**Combining journalctl with Other Commands**

You can combine journal queries with standard Linux text-processing tools.

For example, display recent error messages from Nginx:

```bash
journalctl -u nginx.service -p err -n 50
```

Search service logs for a specific keyword:

```bash
journalctl -u nginx.service | grep -i error
```

Monitor logs and filter messages containing warnings:

```bash
journalctl -u nginx.service -f | grep -i warning
```

These techniques are useful for investigating service behavior without manually reading large log files.

### Troubleshooting Services

Service failures can occur for many reasons, including incorrect configuration, missing files, permission problems, unavailable dependencies, port conflicts, and resource limitations.

systemd provides several tools for identifying and investigating these problems.

**Checking Failed Services**

Start by listing failed units:

```bash
systemctl --failed
```

To inspect a specific failed service:

```bash
systemctl status nginx.service
```

The status output may show:

- The current service state.
- The main process ID.
- The exit status.
- Recent log messages.
- The unit file location.
- Failure information.

**Inspecting Service Logs**

After identifying a failed service, inspect its journal entries:

```bash
journalctl -u nginx.service -b
```

To display only error messages:

```bash
journalctl -u nginx.service -p err
```

To inspect the latest messages:

```bash
journalctl -u nginx.service -n 100
```

Logs often provide the most useful information about why a service failed.

**Checking Unit File Configuration**

Incorrect unit files can prevent services from starting.

Display the complete unit configuration:

```bash
systemctl cat nginx.service
```

Inspect the service properties:

```bash
systemctl show nginx.service
```

Check whether systemd can parse a unit file:

```bash
sudo systemd-analyze verify /etc/systemd/system/rootbound-demo.service
```

This command can identify various unit file configuration problems.

**Checking Service Dependencies**

A service may fail because a required dependency is unavailable.

Display its dependencies:

```bash
systemctl list-dependencies nginx.service
```

Inspect reverse dependencies:

```bash
systemctl list-dependencies --reverse nginx.service
```

Check the status of a required service:

```bash
systemctl status network.target
```

A dependency being active does not always mean that the underlying application or external resource is fully operational. Additional checks may be necessary.

**Checking Port Conflicts**

Two applications cannot normally listen on the same network address and port combination.

To inspect listening sockets:

```bash
sudo ss -tulnp
```

To inspect a specific port, such as port 80:

```bash
sudo ss -tulnp | grep ':80'
```

If another application is already using the required port, the service may fail to bind to it.

You should identify the process and resolve the conflict rather than terminating an unknown process.

**Checking File Permissions**

A service may fail because its user account cannot access required files or directories.

Inspect file permissions:

```bash
ls -l /opt/example-app/
```

Inspect directory ownership:

```bash
ls -ld /opt/example-app/
```

Check the service's configured user:

```bash
systemctl show example.service -p User -p Group
```

Verify that the configured service account has the required access.

**Checking Service Executables**

A service may fail because its executable does not exist or cannot be executed.

Inspect the configured command:

```bash
systemctl cat example.service
```

Check whether the executable exists:

```bash
ls -l /usr/local/bin/example-app
```

Check whether the file has execute permission:

```bash
test -x /usr/local/bin/example-app && echo "Executable found"
```

Also verify that scripts use a valid interpreter and that their dependencies are available.

**Checking Resource Limitations**

Services may fail because of insufficient memory, CPU resources, file descriptors, or other system limitations.

Useful commands include:

```bash
free -h
```

```bash
df -h
```

```bash
df -i
```

```bash
top
```

You can also inspect service resource information:

```bash
systemctl status nginx.service
```

For more detailed resource and control group information:

```bash
systemd-cgtop
```

**Restarting a Failed Service**

After identifying and correcting the underlying problem, attempt to restart the service:

```bash
sudo systemctl restart nginx.service
```

Verify its status:

```bash
systemctl status nginx.service
```

If systemd has recorded the unit as failed and you want to clear that recorded state:

```bash
sudo systemctl reset-failed nginx.service
```

Remember that resetting the failed state does not correct the original issue.

**Understanding Common Service Problems**

| Problem | Possible Cause | Investigation |
|---|---|---|
| Service fails to start | Invalid configuration or missing executable. | Check `systemctl status` and `journalctl`. |
| Permission denied | Incorrect file ownership or permissions. | Inspect `ls -l` and service user settings. |
| Address already in use | Another process occupies the required port. | Check `ss -tulnp`. |
| Dependency failed | A required unit is unavailable. | Inspect unit dependencies and their status. |
| Service repeatedly restarts | Application crashes or restart policy is triggered. | Inspect logs and restart settings. |
| Service is inactive | It was stopped or has not been activated. | Check `systemctl status` and enablement. |
| Unit not found | Missing unit file or incorrect service name. | Check `systemctl list-unit-files`. |
| Configuration changes ignored | systemd has not reloaded modified unit files. | Run `systemctl daemon-reload`. |
| Service cannot access files | Incorrect service account or directory permissions. | Inspect `User=`, `Group=`, and filesystem permissions. |

**A Practical Troubleshooting Workflow**

When a service fails, follow a structured investigation process:

1. Check the service status.
2. Identify the failure message and exit status.
3. Inspect recent journal entries.
4. Verify the service unit configuration.
5. Check dependencies and required resources.
6. Inspect executable paths and file permissions.
7. Check for port conflicts or resource limitations.
8. Correct the underlying problem.
9. Restart the service if necessary.
10. Verify that the service is working correctly.

Example workflow:

```bash
# Check the service status
systemctl status nginx

# Inspect recent service logs
journalctl -u nginx -n 50

# Check for failed units
systemctl --failed

# Inspect the service configuration
systemctl cat nginx

# Check listening network ports
sudo ss -tulnp

# Restart the service after fixing the issue
sudo systemctl restart nginx

# Verify the service status
systemctl is-active nginx
```

**Important:** Do not repeatedly restart a failing service without investigating the cause. Repeated restarts can hide the original problem and make troubleshooting more difficult.

**Service Management Summary**

| Command | Description |
|---|---|
| `systemctl start` | Start a service. |
| `systemctl stop` | Stop a service. |
| `systemctl restart` | Restart a service. |
| `systemctl reload` | Reload service configuration when supported. |
| `systemctl reload-or-restart` | Reload or restart a service. |
| `systemctl enable` | Enable automatic service activation. |
| `systemctl disable` | Disable automatic service activation. |
| `systemctl enable --now` | Enable and start a service. |
| `systemctl disable --now` | Disable and stop a service. |
| `systemctl status` | Display service status. |
| `systemctl is-active` | Check whether a service is active. |
| `systemctl is-enabled` | Check whether a service is enabled. |
| `systemctl list-units` | List loaded units. |
| `systemctl list-unit-files` | List installed unit files and their enablement states. |
| `systemctl --failed` | Display failed units. |
| `systemctl mask` | Prevent normal activation of a unit. |
| `systemctl unmask` | Remove a unit mask. |
| `systemctl daemon-reload` | Reload systemd unit configuration. |
| `systemctl cat` | Display a unit file and its drop-ins. |
| `systemctl edit` | Create or modify a unit override. |
| `systemctl revert` | Remove local overrides for a unit. |
| `systemctl reset-failed` | Clear recorded failed state and related failure counters. |
| `systemctl list-dependencies` | Display unit dependencies. |
| `journalctl -u` | Display logs for a specific unit. |
| `journalctl -f` | Follow new journal entries. |
| `journalctl -b` | Display logs from the current boot. |
| `journalctl -p` | Filter journal entries by priority. |
| `systemd-analyze verify` | Verify unit file configuration. |
| `systemd-cgtop` | Monitor resource usage by control groups. |

**Best Practices for Service Management**

- Use `systemctl` to manage services instead of manually terminating their processes.
- Understand the difference between starting and enabling a service.
- Verify service status after every important configuration change.
- Inspect logs before restarting a failing service.
- Use `daemon-reload` after modifying or creating unit files.
- Prefer running applications under dedicated unprivileged accounts.
- Avoid running unnecessary services.
- Enable only the services required by the system.
- Use restart policies carefully to prevent endless restart loops.
- Back up important service configurations before modifying them.
- Avoid editing package-provided unit files directly; use drop-in overrides when possible.
- Verify service dependencies and startup ordering.
- Review service permissions and resource usage regularly.
- Test custom service units before enabling them on production systems.

**Additional Resources**

- [systemd Official Documentation](https://systemd.io/)
- [systemd Manual Pages](https://www.freedesktop.org/software/systemd/man/)
- [systemctl Manual](https://www.freedesktop.org/software/systemd/man/latest/systemctl.html)
- [systemd.service Manual](https://www.freedesktop.org/software/systemd/man/latest/systemd.service.html)
- [systemd.unit Manual](https://www.freedesktop.org/software/systemd/man/latest/systemd.unit.html)
- [journalctl Manual](https://www.freedesktop.org/software/systemd/man/latest/journalctl.html)
- [systemd-journald Manual](https://www.freedesktop.org/software/systemd/man/latest/systemd-journald.service.html)

---


## Practical Exercises

The following exercises are designed to reinforce the concepts and commands introduced in the Service Management section. They provide hands-on experience in understanding systemd, managing services, checking service status, configuring automatic startup, creating custom service units, monitoring logs, and troubleshooting service failures.

### Exercise 1 — Understanding systemd and Services

1. Check the process with PID 1 using `ps`.
2. Verify whether your system uses systemd as its init system.
3. Display the default systemd target using `systemctl get-default`.
4. List the currently active system targets.
5. Display all loaded service units.
6. List all installed service unit files.
7. Inspect the available systemd unit types.
8. Display the dependencies of `multi-user.target`.
9. Inspect the unit file of an existing service using `systemctl cat`.
10. Identify the difference between a service unit and a target unit.

### Exercise 2 — Managing Services with systemctl

1. Choose an installed service available on your system.
2. Check its current status using `systemctl status`.
3. Start the service using `systemctl start`.
4. Verify that the service is active.
5. Stop the service using `systemctl stop`.
6. Verify that the service is inactive.
7. Start the service again.
8. Restart the service using `systemctl restart`.
9. Check whether the service supports configuration reloads.
10. If supported, reload its configuration using `systemctl reload`.
11. Inspect the service configuration using `systemctl cat`.
12. Display selected service properties using `systemctl show`.

### Exercise 3 — Checking Service Status

1. Display all active service units.
2. Display all inactive service units.
3. List all failed systemd units.
4. Check whether a specific service is active using `systemctl is-active`.
5. Check whether the service is enabled using `systemctl is-enabled`.
6. Display the main process ID of the service.
7. Inspect the service process using `ps`.
8. Display the service's dependencies.
9. Inspect its control group using `systemd-cgls`.
10. Explain the difference between active, inactive, enabled, disabled, and failed states.

### Exercise 4 — Enabling and Disabling Services

1. Select an installed service that is safe to experiment with.
2. Check whether it is currently enabled.
3. Enable the service using `systemctl enable`.
4. Verify its enablement status.
5. Disable the service using `systemctl disable`.
6. Verify that it is disabled.
7. Enable and start the service using `systemctl enable --now`.
8. Disable and stop the service using `systemctl disable --now`.
9. Inspect the service's installation configuration.
10. Explain the difference between enabling and starting a service.
11. If appropriate for your test environment, mask the service.
12. Verify that normal activation is blocked.
13. Unmask the service and verify its configuration.

### Exercise 5 — Creating and Managing Service Units

1. Create a shell script named `rootbound-service.sh`.
2. Add a loop that displays a message every 30 seconds.
3. Save the script in `/usr/local/bin/`.
4. Make the script executable using `chmod`.
5. Create a systemd unit named `rootbound-service.service`.
6. Add a description in the `[Unit]` section.
7. Configure the service using `Type=exec`.
8. Set `ExecStart` to the script's absolute path.
9. Configure `Restart=on-failure`.
10. Configure a restart delay using `RestartSec`.
11. Save the unit file in `/etc/systemd/system/`.
12. Reload systemd using `daemon-reload`.
13. Start the custom service.
14. Verify its status using `systemctl status`.
15. Inspect its output using `journalctl`.
16. Enable the service for automatic startup.
17. Stop and disable the service.
18. Remove the unit file and script after completing the exercise.
19. Reload systemd configuration again.

### Exercise 6 — Monitoring Service Logs

1. Display the system journal using `journalctl`.
2. Display the latest 50 journal entries.
3. Display logs from the current boot.
4. Display logs from the previous boot, if available.
5. Display logs for a specific service using `journalctl -u`.
6. Display the latest 20 entries for that service.
7. Filter service logs by error priority.
8. Display warning messages and higher-priority messages.
9. Follow service logs in real time using `journalctl -f`.
10. Filter service logs using `grep`.
11. Display logs within a specific time range.
12. Inspect journal disk usage.
13. Identify the difference between persistent and runtime journal storage.

### Exercise 7 — Troubleshooting Services

1. Select an installed service and inspect its status.
2. Identify its main process ID.
3. Inspect recent logs using `journalctl`.
4. Display the service unit configuration.
5. Check whether its required dependencies are available.
6. Inspect the executable path configured in the unit file.
7. Verify the permissions of the service's executable and working directory.
8. Check listening network ports using `ss -tulnp`.
9. Inspect system memory and disk availability.
10. Identify possible reasons for a service startup failure.
11. If you encounter a safe and reversible failure in your test environment, investigate it.
12. Correct the underlying problem.
13. Restart the service if necessary.
14. Verify that the service is working correctly.
15. Explain the troubleshooting steps you performed.

### Exercise 8 — Service Management with Shell Scripts

Create a script named `service-monitor.sh` that monitors the status of a systemd service.

The script should:

1. Accept a service name as a command-line argument.
2. Check whether an argument was provided.
3. Verify that the specified unit exists.
4. Display the service's current status.
5. Check whether the service is active using `systemctl is-active`.
6. Display whether the service is enabled.
7. Display the main process ID when available.
8. Display the latest 10 journal entries for the service.
9. Handle invalid service names and command errors.
10. Display clear and readable output.

Example:

```bash
./service-monitor.sh ssh.service
```

Expected output:

```text
Service Monitor
----------------
Service: ssh.service
Status: active
Enabled: enabled
Main PID: 1234

Latest Service Logs:
--------------------
[Recent journal entries]

Monitoring completed.
```

### Exercise 9 — Final Challenge: Linux Service Administration

Create and manage a custom systemd service for a simple background application.

Your task is to configure, monitor, and troubleshoot the application.

**Application Requirements:**

- Create a script named `rootbound-app.sh`.
- The script should write a timestamped message to a log file.
- The script should repeat its operation at regular intervals.
- The application should run as a dedicated unprivileged user.
- The application should be managed through systemd.

**Service Management Requirements:**

1. Create a dedicated user named `rootbound`.
2. Create an application directory under `/opt/rootbound`.
3. Configure appropriate ownership and permissions.
4. Create the application script.
5. Make the script executable.
6. Create a unit file named `rootbound-app.service`.
7. Configure the service description and startup dependencies.
8. Set the service execution type.
9. Configure the service to run as `rootbound`.
10. Set the working directory.
11. Configure automatic restart on failure.
12. Reload systemd configuration.
13. Start and enable the service.
14. Verify its status.
15. Inspect its logs using `journalctl`.
16. Stop and restart the service.
17. Verify that the application runs under the correct user.
18. Test the service after a controlled failure.
19. Correct any configuration or permission problems.
20. Disable and remove the service when the exercise is complete.

**Additional Requirements:**

- Use `systemctl status` to verify service state.
- Use `systemctl show` to inspect service properties.
- Use `journalctl` to investigate application behavior.
- Use `systemctl list-dependencies` to inspect dependencies.
- Explain the difference between `daemon-reload` and `restart`.
- Document the service configuration and troubleshooting steps.

### Exercise 10 — Knowledge Check

Answer the following questions to test your understanding of Linux service management.

1. What is systemd?
2. What is the role of PID 1?
3. What is a systemd unit?
4. What is the difference between a service unit and a target unit?
5. Which command starts a service?
6. What is the difference between `start` and `enable`?
7. What is the purpose of `systemctl status`?
8. How can you check whether a service is active?
9. How can you check whether a service is enabled?
10. What is the difference between `restart` and `reload`?
11. What does `systemctl daemon-reload` do?
12. What is the difference between `disable` and `mask`?
13. What is the purpose of the `[Unit]` section?
14. What is the purpose of the `[Service]` section?
15. What is the purpose of the `[Install]` section?
16. What does `Restart=on-failure` do?
17. What is the difference between `Type=simple` and `Type=oneshot`?
18. Which command displays logs for a specific service?
19. How can you identify failed systemd units?
20. Why should applications run under dedicated unprivileged accounts?


**Goal:** Develop practical skills in managing Linux services with systemd, controlling service states and startup behavior, creating custom service units, monitoring logs, and troubleshooting service failures.

---

<p align="right">
  <a href="../Day-08/README.md">← Day 08</a>
  &nbsp; | &nbsp;
  <a href="../Day-10/">Day 10 →</a>
  &nbsp; | &nbsp;
  <a href="#day-09--service-management-systemd">Back to top ↑</a>
</p>
