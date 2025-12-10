# Managing processes
## Proccess types

1. Shell jobs.
    - Commands started from the current shell.
    - Also reffered to as interactive proccesses.
2. Daemons.
    - Processes that provide services.
    - Normally start the a computer is booted.
    - Often run with root privileges.
3. Kernel threads.
    - Part of the linux kernel.
    - Cannot be managed using common tools.

    A **thread** is a task started by a process and that a dedicated CPU can service.
## Job managment overview

- `& (used at the end of a command)` - starts the command immediately in the background.
- `Ctrl + Z` - pauses the job so that it can be managed.
- `Ctrl + D` - sends the EOF character to the job to stop waiting for further input.
- `Ctrl + C` - cancels the job.
- `bg` - continues the job that was paused.
- `fg` - brings the last job moved to background to the foreground.
- `jobs` - shows which jobs are running from the current shell.
## The ps command

The most common command to get an overview of currently running \
processes is `ps`. Most notable flags:
- `ps -aux` - short summary of active processes.
- `ps -ef` - shows command that was used to start the process.
- `ps -fax` - shows hierarchical relationships between parent and child processes.
## Cgroups

Cgroups are used to allocate system resources. Three system ares, or slices, \
are defined:
1. System - where all systemd-managed processes are running.
2. User - where all user (including root) processes are running.
3. Machine - optional slice used for virtual machines and containers.

By default, CPU capacity is uqually diveded between the slices when there is \
high demand. \
To temporarily shut down a CPU core, use the command:
```echo 0 > /sys/bus/cpu/devices/cpu1/online```
Replace the number for the CPU core you want to shut down.
## Process priority

By default, processes are started with priority number 20.
- `nice` is used to start a process with an adjusted priority.
- `renice` is used to change the priority of a currently active process.
    - Both accept values between -20 and 19, where higher value = decreased priority.
## Killing processes

`kill <PID>` command is used to send a signal to a process.
`killall` is used to send signals to processes using the same name. \
Most common `kill` values:
1. SIGTERM (15) to ask a process to stop.
2. SIGKILL (9) to force a process to stop.
3. SIGHUP (1) to hang up a process, used to reread its configuration files.

Zombie processes have completed execution but are still listed in the process table
    - Harmless.
    - Can be removed by sending SIGCHLD or SIGKILL to their parents.
## Top and other information commands

`top` is a tool to overview and manage processes. It shows process states, \
which are as follows:
1. Running (R)
2. Sleeping (S)
3. Uninterruptible sleep (D)
4. Stopped (T)
5. Zombie (Z)
It Has useful keybinds, such as `k` for `kill` and `r` for `renice`.

Other commands include:
- `lscpu` prints cpu information.
- `uptime` prints current load average statistics.
## Tuned

`tuned` offers a daemon that monitors system activity and provides profiles for \
best possible latency, throughput or power consumption.

Profile overview:
- `balanced` - best compromise between power consumption and performance.
- `desktop` - better response to interactive applications.
- `latency-performace` - maximum throughput.
- `network-latency` - additional options to reduce network latency.
- `network-throughput` - optimizes older CPUs for streaming content.
- `powersave` - maximum power saving.
- `throughput-performance` - maximum throughput.
- `virtual-guest` - for running as a virtual machine.
- `virtual-host` - for use as a KVM host. 

`tuned-adm` is a command used to manage performace profiles.
- `tuned-adm active` - prints selected performace profile.
- `tuned-adm list` - prints overview of available profiles.
- `tuned-adm profile [PROFILE]` - select profile.
- `tuned-adm recommend` - prints recommended profile.
