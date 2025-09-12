# System Monitoring & Logs in Linux

This document provides a detailed guide on monitoring system performance and managing logs in Linux. Covered tools:
- **`journalctl`** – view systemd logs.
- **`/var/log/`** – location of system log files.
- **`uptime`** – show system load and uptime.
- **`free`** – view memory usage.
- **`vmstat`** – process, memory, and CPU statistics.
- **`iostat`** – CPU and I/O usage statistics.

---

## Table of Contents
1. Introduction
2. Logs Management
   - `journalctl`
   - `/var/log/`
3. System Monitoring Commands
   - `uptime`
   - `free`
   - `vmstat`
   - `iostat`
4. Practical Examples
5. Troubleshooting Tips
6. Cheatsheet

---

# 1. Introduction

System monitoring and log management are essential for maintaining the health of Linux systems. Logs record system and application events, while monitoring tools provide real-time information about CPU, memory, disk, and network usage.

---

# 2. Logs Management

## 2.1 `journalctl`

### What it is
- A command for querying and displaying logs collected by **systemd journal**.

### Syntax
```sh
journalctl [options]
```

### Common Options
- `-b` → logs from current boot.
- `-u <service>` → logs for a specific service.
- `-f` → follow logs in real-time (like `tail -f`).
- `--since` / `--until` → filter by time.
- `-p` → filter by priority (0=emerg, 3=err, 6=info, etc.).

### Examples
```sh
journalctl -b                     # logs from this boot
journalctl -u ssh                 # logs for ssh service
journalctl -f                     # follow logs live
journalctl --since "2024-01-01"   # logs since given date
journalctl -p err                 # show only errors
```

---

## 2.2 `/var/log/`

### What it is
- Directory containing plain text log files for system and applications.

### Important Log Files
- `/var/log/syslog` → general system messages (Debian/Ubuntu).
- `/var/log/messages` → system messages (RHEL/CentOS).
- `/var/log/auth.log` → authentication logs.
- `/var/log/kern.log` → kernel logs.
- `/var/log/dmesg` → boot-time kernel messages.
- `/var/log/apache2/` or `/var/log/httpd/` → web server logs.

### Examples
```sh
tail -f /var/log/syslog        # view logs in real-time
less /var/log/auth.log         # scroll through authentication logs
grep "error" /var/log/syslog   # search errors in logs
```

---

# 3. System Monitoring Commands

## 3.1 `uptime`

### What it is
- Shows system uptime, load averages, and logged-in users.

### Syntax
```sh
uptime
```

### Example Output
```
 14:10:23 up 5 days,  2:31,  3 users,  load average: 0.20, 0.15, 0.10
```

### Explanation
- **uptime** → how long system has been running.
- **users** → how many users logged in.
- **load average** → system load over 1, 5, and 15 minutes.

---

## 3.2 `free`

### What it is
- Displays memory usage information.

### Syntax
```sh
free [options]
```

### Common Options
- `-h` → human-readable.
- `-m` → show in MB.
- `-g` → show in GB.

### Example
```sh
free -h
```
Output:
```
              total        used        free      shared  buff/cache   available
Mem:           16Gi       8.5Gi       2.0Gi       512Mi       5.5Gi       6.9Gi
Swap:          2.0Gi        0B        2.0Gi
```

---

## 3.3 `vmstat`

### What it is
- Provides a summary of system processes, memory, paging, block I/O, and CPU usage.

### Syntax
```sh
vmstat [interval] [count]
```

### Example
```sh
vmstat 2 5
```
- Displays stats every 2 seconds, 5 times.

### Key Columns
- **procs** → r (running), b (blocked).
- **memory** → swap, free, cache.
- **io** → bi (blocks in), bo (blocks out).
- **cpu** → us (user), sy (system), id (idle), wa (wait).

---

## 3.4 `iostat`

### What it is
- Displays CPU and input/output statistics for devices.
- Provided by `sysstat` package.

### Syntax
```sh
iostat [options] [interval] [count]
```

### Common Options
- `-c` → CPU statistics.
- `-d` → device statistics.
- `-x` → extended stats.

### Example
```sh
iostat -x 2 3
```
- Shows extended stats every 2 seconds, 3 times.

---

# 4. Practical Examples

1. **Check system uptime and load**
```sh
uptime
```

2. **View memory usage in human-readable format**
```sh
free -h
```

3. **Monitor CPU/memory every 5 seconds**
```sh
vmstat 5
```

4. **Check disk I/O performance**
```sh
iostat -dx 5
```

5. **View logs of SSH service**
```sh
journalctl -u ssh
```

6. **Check login attempts**
```sh
tail -n 50 /var/log/auth.log
```

---

# 5. Troubleshooting Tips

- If `journalctl` shows no logs → check if systemd-journald is running.
- If `/var/log/` is missing files → verify logrotate settings.
- High load average (`uptime`) → investigate with `top`, `htop`, or `vmstat`.
- Low memory in `free` output is normal; check `available` column for true free memory.
- If `iostat` missing → install `sysstat` package.

---

# 6. Cheatsheet

**Logs**
- `journalctl -b` → logs from current boot.
- `tail -f /var/log/syslog` → live logs.

**Monitoring**
- `uptime` → show uptime & load.
- `free -h` → memory usage.
- `vmstat 2` → system stats every 2s.
- `iostat -x 2` → extended I/O stats.

---

This concludes the **System Monitoring & Logs** guide. Practice these commands to monitor system health and analyze logs