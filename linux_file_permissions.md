
# Linux File Permissions

This document provides a detailed explanation of **Linux file permissions**, including how to view and modify them using `chmod`, `chown`, `chgrp`, and `umask`.

---

## 1. Understanding File Permissions

Every file and directory in Linux has **permissions** and an **owner**. Permissions determine who can **read (r)**, **write (w)**, or **execute (x)** a file.

- **Permission Format**: `-rwxr-xr--`
  - First character: file type (`-` for file, `d` for directory)
  - Next 9 characters: permissions
    - First 3: owner permissions
    - Next 3: group permissions
    - Last 3: others permissions

**Example:**
```bash
-rwxr-xr-- 1 joseph developers 4096 Sep 9 12:00 script.sh
```
- Owner `joseph` can read, write, execute
- Group `developers` can read and execute
- Others can only read

- **Permission numbers (octal)**:
  - `r = 4`, `w = 2`, `x = 1`
  - Sum values to get permission number:
    - `rwx` = 4+2+1 = 7
    - `rw-` = 4+2+0 = 6
    - `r--` = 4+0+0 = 4

---

## 2. Viewing Permissions

- **ls -l**: List files with permissions
```bash
ls -l
```
- **stat**: Detailed info about a file
```bash
stat file.txt
```

---

## 3. Changing Permissions: `chmod`

`chmod` modifies file or directory permissions.

### a. Symbolic Mode
- Syntax: `chmod [who][operator][permissions] file`
  - `who`: `u` (user/owner), `g` (group), `o` (others), `a` (all)
  - `operator`: `+` (add), `-` (remove), `=` (set exactly)
  - `permissions`: `r`, `w`, `x`

**Examples:**
```bash
chmod u+x script.sh      # add execute permission to owner
chmod g-w file.txt       # remove write from group
chmod o=r file.txt       # set others to read-only
chmod a+r file.txt       # add read permission to all
```

### b. Numeric Mode (Octal)
- Syntax: `chmod [numeric] file`
- Example: `chmod 754 file.txt`
  - 7 (owner) = rwx
  - 5 (group) = r-x
  - 4 (others) = r--

**Examples:**
```bash
chmod 755 script.sh   # owner rwx, group rx, others rx
chmod 644 file.txt    # owner rw, group r, others r
```

---

## 4. Changing Ownership: `chown` and `chgrp`

- **chown**: Change file owner and optionally group
```bash
sudo chown joseph file.txt       # change owner
sudo chown joseph:developers file.txt  # change owner and group
```

- **chgrp**: Change only group ownership
```bash
sudo chgrp developers file.txt
```

---

## 5. Default Permissions: `umask`

- `umask` determines **default permissions** for newly created files/directories
- View current `umask`:
```bash
umask
```
- Example:
```bash
umask 022
```
- How it works:
  - Default permission: 666 (files) / 777 (directories)
  - `umask` subtracts values
    - `022` → files: 666-022=644, directories: 777-022=755

- Set umask temporarily:
```bash
umask 027
```
- Set umask permanently in `~/.bashrc` or `/etc/profile`

---

## 6. Special Permissions

1. **Setuid (s)**: Execute file as owner
```bash
chmod u+s executable
```
2. **Setgid (s)**: Files created in directory inherit group
```bash
chmod g+s directory
```
3. **Sticky bit (t)**: Only owner can delete files in a directory (e.g., `/tmp`)
```bash
chmod +t directory
```

---

## 7. Examples

```bash
# Make script executable by owner only
chmod u+x script.sh

# Change owner to 'joseph' and group to 'devs'
sudo chown joseph:devs project.txt

# Set default permissions for new files to 640
umask 027

# Add sticky bit to shared directory
chmod +t /shared
```

---

# Conclusion

Understanding and managing Linux file permissions is crucial for **security** and **collaboration**. Mastering `chmod`, `chown`, `chgrp`, and `umask` allows you to control access effectively.
