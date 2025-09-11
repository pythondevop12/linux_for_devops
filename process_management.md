# Process Management in Linux

Process management in Linux is essential for monitoring, controlling,
and managing system resources. Below are some important commands with
detailed explanations:

------------------------------------------------------------------------

## 1. **ps (Process Status)**

The `ps` command is used to display information about active processes.

### Common Usage:

-   `ps` → Shows processes running in the current shell.
-   `ps -e` → Displays all processes.
-   `ps -ef` → Shows full-format listing of all processes.
-   `ps aux` → Displays detailed information including memory & CPU
    usage.

### Example:

``` bash
ps aux | grep ssh
```

This will show all processes related to `ssh`.

------------------------------------------------------------------------

## 2. **top**

The `top` command provides a dynamic, real-time view of system
processes.

### Features:

-   Displays CPU usage, memory usage, process IDs, and more.
-   Processes are sorted by CPU usage by default.

### Useful Shortcuts in `top`:

-   `q` → Quit `top`
-   `k` → Kill a process
-   `P` → Sort by CPU usage
-   `M` → Sort by memory usage

### Example:

``` bash
top
```

------------------------------------------------------------------------

## 3. **htop**

`htop` is an improved, interactive version of `top`. It provides a
user-friendly interface.

### Features:

-   Colorful output for better readability.
-   Easy navigation using arrow keys.
-   Allows killing processes without typing PID.

### Example:

``` bash
htop
```

------------------------------------------------------------------------

## 4. **kill**

The `kill` command is used to terminate processes manually.

### Syntax:

``` bash
kill [signal] PID
```

### Common Signals:

-   `kill -9 PID` → Forcefully kill a process.
-   `kill -15 PID` → Gracefully terminate a process.

### Example:

``` bash
kill -9 1234
```

This will kill the process with PID `1234`.

------------------------------------------------------------------------

## 5. **nice**

The `nice` command is used to start a process with a specific priority.

### Priority Range:

-   From **-20 (highest priority)** to **19 (lowest priority)**.

### Example:

``` bash
nice -n 10 python script.py
```

This runs `script.py` with a lower priority.

------------------------------------------------------------------------

## 6. **jobs**

The `jobs` command displays background jobs started in the current shell
session.

### Example:

``` bash
jobs
```

Output might look like:

    [1]   Running    sleep 100 &

------------------------------------------------------------------------

## 7. **bg (Background)**

The `bg` command resumes a suspended job in the background.

### Example:

``` bash
bg %1
```

This resumes job number `1` in the background.

------------------------------------------------------------------------

## 8. **fg (Foreground)**

The `fg` command brings a background job to the foreground.

### Example:

``` bash
fg %1
```

This brings job number `1` to the foreground.

------------------------------------------------------------------------

# Summary

-   **ps** → View processes
-   **top** → Real-time monitoring
-   **htop** → Interactive process viewer
-   **kill** → Terminate processes
-   **nice** → Start process with priority
-   **jobs** → View background jobs
-   **bg** → Resume job in background
-   **fg** → Bring job to foreground

These commands help you effectively monitor and manage processes in
Linux.
