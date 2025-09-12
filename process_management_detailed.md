# ⚙️ Linux Process Management (Detailed Guide)

Process management in Linux is one of the most important responsibilities of a system administrator.  
A **process** is simply a running instance of a program. Using process management commands, we can monitor, control, and manipulate these processes.

---

## 🖥️ 1. `ps` – Process Status

The `ps` command displays information about **currently running processes**.

### Syntax:
```bash
ps [options]
```

### Common Options:
| Option | Description |
|-------|-------------|
| `ps` | Show processes for the current shell session |
| `ps -e` | Show all processes running on the system |
| `ps -ef` | Full-format listing with extra details |
| `ps aux` | BSD-style format (commonly used) |

### Example:
```bash
ps -ef | grep apache
```
Finds all running Apache processes.

---

## 📊 2. `top` – Real-Time Process Monitoring

The `top` command shows a **real-time dynamic view** of all running processes.

### Features:
- Displays CPU, memory usage, process ID (PID), and more.
- Allows you to kill processes from within the interface.
- Interactive, updates every few seconds.

### Usage:
```bash
top
```
Inside `top`:
- Press `k` → Kill a process (enter PID).
- Press `r` → Renice (change priority) of a process.
- Press `q` → Quit.

---

## 🎨 3. `htop` – Interactive Process Viewer

`htop` is a more **user-friendly and colorful** version of `top`.

### Features:
- Scroll horizontally and vertically to view processes.
- Easily filter or search for processes.
- Allows killing and renicing processes using function keys.

### Installation (if not installed):
```bash
sudo apt install htop   # Debian/Ubuntu
sudo yum install htop   # RHEL/CentOS
```

### Usage:
```bash
htop
```

Use arrow keys to navigate and function keys (F9 to kill, F7/F8 to change priority).

---

## 🛑 4. `kill` – Terminate Processes

The `kill` command is used to **stop or terminate processes** using their PID.

### Syntax:
```bash
kill [signal] PID
```

### Common Signals:
| Signal | Number | Description |
|-------|--------|-------------|
| `SIGTERM` | 15 | Graceful termination (default) |
| `SIGKILL` | 9 | Forcefully kill the process |
| `SIGHUP` | 1 | Reload configuration for certain daemons |

### Example:
```bash
kill -9 1234
```
Forcefully terminates process with PID `1234`.

---

## ⚖️ 5. `nice` and `renice` – Process Priority

Processes in Linux have a **priority value** called **niceness** (-20 to 19).  
Lower values mean higher priority.

### `nice` – Start Process with Priority
```bash
nice -n 10 python script.py
```
Starts `script.py` with a lower priority (10).

### `renice` – Change Priority of Running Process
```bash
renice -5 -p 1234
```
Changes the priority of process `1234` to `-5` (higher priority).

---

## 🏃 6. `jobs` – Manage Background Jobs

The `jobs` command shows **background tasks** running in the current shell.

### Example:
```bash
jobs
```
Output:
```
[1]+  Running  python script.py &
```

---

## 🔄 7. `bg` – Resume Job in Background

The `bg` command resumes a suspended job in the **background**.

### Example:
```bash
bg %1
```
Resumes job number 1 in the background.

---

## 🎯 8. `fg` – Bring Job to Foreground

The `fg` command brings a **background job back to foreground**.

### Example:
```bash
fg %1
```
Brings job number 1 to the foreground.

---

## 📋 Summary Table

| Command | Purpose | Example |
|--------|---------|--------|
| **ps** | Show running processes | `ps -ef` |
| **top** | Real-time process monitoring | `top` |
| **htop** | User-friendly process monitor | `htop` |
| **kill** | Terminate process by PID | `kill -9 1234` |
| **nice** | Start process with priority | `nice -n 10 script.sh` |
| **renice** | Change priority of running process | `renice -5 -p 1234` |
| **jobs** | List background jobs | `jobs` |
| **bg** | Resume a job in background | `bg %1` |
| **fg** | Bring job to foreground | `fg %1` |

---

## ✅ Best Practices
- Always use `kill -15` before `kill -9` to allow graceful shutdown.
- Use `htop` for easier process monitoring.
- Be careful with `renice` on critical processes — setting a high priority can starve other processes.
- Use background jobs (`&`, `bg`, `fg`) to manage long-running commands.

