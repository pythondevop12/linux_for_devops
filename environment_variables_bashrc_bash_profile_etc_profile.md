# Environment Variables in Linux

This document explains **environment variables** and the configuration files commonly used to set them in Linux:
- **`.bashrc`**
- **`.bash_profile`**
- **`/etc/profile`**

The goal is to help anyone clearly understand what environment variables are, how they work, and where to configure them.

---

## Table of Contents
1. What are Environment Variables?
2. Viewing and Setting Environment Variables
3. Persistent Environment Variables
   - `.bashrc`
   - `.bash_profile`
   - `/etc/profile`
4. Differences Between Files
5. Practical Examples
6. Troubleshooting Tips
7. Cheatsheet

---

# 1. What are Environment Variables?

Environment variables are **key-value pairs** stored in a process’s environment. They influence how programs and shells behave.

### Examples of common environment variables:
- `PATH` → list of directories searched for commands.
- `HOME` → user’s home directory.
- `USER` → current logged-in username.
- `SHELL` → default shell.
- `EDITOR` → default text editor.

### Viewing variables
```sh
echo $PATH     # show PATH
echo $HOME     # show home directory
printenv       # list all environment variables
```

### Setting variables temporarily
```sh
MYVAR="hello"
echo $MYVAR
```
- This will only last until the shell session ends.

### Exporting variables
```sh
export MYVAR="hello"
```
- Makes `MYVAR` available to child processes.

---

# 2. Viewing and Setting Environment Variables

### View all variables
```sh
printenv
```

### Set variable (temporary)
```sh
VAR=value
```

### Export variable
```sh
export VAR=value
```

### Remove variable
```sh
unset VAR
```

---

# 3. Persistent Environment Variables

To make variables **persist** across sessions, add them to shell configuration files.

## 3.1 `.bashrc`

### What it is
- A shell script executed **whenever a new interactive non-login shell** starts.
- Common place to set variables, aliases, and functions.

### Location
- Located in user’s home directory: `~/.bashrc`

### Example
```sh
# ~/.bashrc
export PATH=$PATH:$HOME/bin
alias ll='ls -la'
export EDITOR=nano
```

### When it runs
- Runs for interactive shells (e.g., opening a new terminal window).

---

## 3.2 `.bash_profile`

### What it is
- Executed for **login shells** (when you log in via console, SSH, or text mode).
- Typically used to set environment variables that should only be set once per session.

### Location
- User’s home directory: `~/.bash_profile`

### Example
```sh
# ~/.bash_profile
export PATH=$PATH:$HOME/.local/bin
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk

# Source .bashrc to include its settings
if [ -f ~/.bashrc ]; then
    . ~/.bashrc
fi
```

### When it runs
- When logging in via SSH or at a TTY.
- Not executed when opening new terminals unless configured.

---

## 3.3 `/etc/profile`

### What it is
- A **system-wide configuration file** executed for **login shells**.
- Affects **all users**.

### Location
- `/etc/profile`

### Example
```sh
# /etc/profile
export PATH=$PATH:/usr/local/bin
umask 022
```

### When it runs
- For all login shells across all users.
- Good place for global environment variables.

---

# 4. Differences Between Files

| File            | Scope             | Runs for            | Typical Use |
|-----------------|------------------|--------------------|-------------|
| `~/.bashrc`     | Per-user         | Interactive shells | Aliases, user-specific vars |
| `~/.bash_profile` | Per-user       | Login shells       | Session-wide vars, PATH, sourcing `.bashrc` |
| `/etc/profile`  | System-wide      | Login shells       | Global environment vars, defaults |

---

# 5. Practical Examples

1. **Add custom scripts to PATH**
```sh
echo 'export PATH=$PATH:$HOME/scripts' >> ~/.bashrc
```

2. **Set default editor**
```sh
echo 'export EDITOR=vim' >> ~/.bash_profile
```

3. **Set system-wide umask**
```sh
echo 'umask 027' | sudo tee -a /etc/profile
```

4. **Apply changes immediately**
```sh
source ~/.bashrc
```

---

# 6. Troubleshooting Tips

- If variables are not applying:
  - Check which file is being executed (`.bashrc` vs `.bash_profile`).
  - Ensure you are using `export VAR=value`.
- Use `echo $SHELL` to confirm default shell.
- Run `env` to confirm applied variables.
- To debug, add `echo "Loaded .bashrc"` inside the file.

---

# 7. Cheatsheet

**Temporary variable**
```sh
VAR=value
```

**Export variable**
```sh
export VAR=value
```

**Reload `.bashrc`**
```sh
source ~/.bashrc
```

**Files summary:**
- `~/.bashrc` → per-user, interactive shells.
- `~/.bash_profile` → per-user, login shells.
- `/etc/profile` → system-wide, login shells.

---

This concludes the **Environment Variables in Linux** guide. Use `.bashrc`, `.bash_profile`, and `/etc/profile