# SOP 11: Services and Process Management

## Purpose

Manage processes and system services from the command line, including running tasks in the background and switching between them.

## Run a process in the background

Append `&` to a command to run it in the background and get the prompt back.

| Command | Purpose | Example |
|---------|---------|---------|
| `&` | Run a command in the background | `gedit &` |
| `jobs` | List background jobs | `jobs` |
| `fg` | Bring a job to the foreground | `fg 1` |
| `Ctrl+Z` | Suspend the foreground job | |
| `bg` | Resume a suspended job in the background | `bg 1` |

## List running processes

```bash
ps
```

Lists processes in the current terminal with their process IDs (PIDs):

```
$ ps
    PID TTY          TIME CMD
  56615 pts/0    00:00:00 bash
  56637 pts/0    00:00:00 ps
```

To see every process on the system, use `ps aux` or `ps -ef`.

## Kill a process by PID

`kill` sends a signal to a process. Try signals from gentlest to harshest so the process gets a chance to exit cleanly. Signals can be given by name or number.

| Signal | Number | Effect | Example |
|--------|--------|--------|---------|
| `SIGHUP` | 1 | Hangup. Many daemons reload their configuration | `kill -HUP 1000` |
| `SIGINT` | 2 | Interrupt, same as `Ctrl+C` | `kill -INT 1000` |
| `SIGTERM` | 15 | Ask the process to terminate, allowing cleanup (default) | `kill -TERM 1000` |
| `SIGKILL` | 9 | Forcibly terminate; cannot be caught. **Last resort** | `kill -KILL 1000` |

`kill -l` lists all signals.

## Manage system services: `systemctl`

| Command | Purpose |
|---------|---------|
| `systemctl stop <service>` | Stop a service |
| `systemctl start <service>` | Start a service |
| `systemctl disable <service>` | Don't start at boot |
| `systemctl enable <service>` | Start at boot |
| `systemctl restart <service>` | Restart a service |
| `systemctl reload <service>` | Reload configuration without stopping, if supported |
| `systemctl status <service>` | Show service status |

Example:

```bash
sudo systemctl restart ddclient
```
