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

- `& (used at the end of a command` - starts the command immediately in the background.
- `Ctrl + Z` - pauses the job so that it can be managed.
- `Ctrl + D` - sends the EOF character to the job to stop waiting for further input.
- `Ctrl + C` - cancels the job.
- `bg` - continues the job that was paused.
- `fg` - brings the last job moved to background to the foreground.
- `jobs` - shows which jobs are running from the current shell.

## The ps command
    The most common command to get an overview of currently running \
    processes is ps.
    Flags:
    - `ps -aux` - short summary of active processes.
    - `ps -ef` - shows command that was used to start the process.
    - `ps -fax` - shows hierarchical relationships between parent and child     processes.
