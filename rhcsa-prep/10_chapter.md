# Managing processes
## Proccess types

1. Shell jobs.
2. Daemons.
3. Kernel threads.
## Job managment overview

- `& (used at the end of a command` - starts the command immediately in the background.
- `Ctrl + Z` - pauses the job so that it can be managed.
- `Ctrl + D` - sends the EOF character to the job to stop waiting for further input.
- `Ctrl + C` - cancels the job.
- `bg` - continues the job that was paused.
- `fg` - brings the last job moved to background to the foreground.
- `jobs` - shows which jobs are running from the current shell.
