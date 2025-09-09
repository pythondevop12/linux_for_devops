# 👤 User & Group Management in Linux

Managing users and groups is a fundamental task for Linux system administration.  
This includes creating, modifying, and securing user accounts as well as managing groups.

---

## 1. `useradd`
The `useradd` command is used to **create a new user account**.

### Syntax:
```bash
useradd [options] username
```

### Common Options:
- `-m` → Create the user’s home directory.
- `-d /path/to/dir` → Specify a custom home directory.
- `-s /bin/bash` → Define the default shell.
- `-u UID` → Assign a custom user ID.

### Example:
```bash
useradd -m -s /bin/bash amit
```
Creates a user **amit** with a home directory and bash shell.

---

## 2. `usermod`
The `usermod` command is used to **modify an existing user account**.

### Syntax:
```bash
usermod [options] username
```

### Common Options:
- `-l newname` → Change username.
- `-d /new/home -m` → Change home directory and move files.
- `-s /bin/zsh` → Change login shell.
- `-aG groupname` → Add user to a group (append).

### Example:
```bash
usermod -aG sudo amit
```
Adds user **amit** to the `sudo` group.

---

## 3. `passwd`
The `passwd` command is used to **set or change a user’s password**.

### Syntax:
```bash
passwd [username]
```

### Example:
```bash
passwd amit
```
Prompts to set a new password for user **amit**.

---

## 4. `groupadd`
The `groupadd` command is used to **create a new group**.

### Syntax:
```bash
groupadd [options] groupname
```

### Common Options:
- `-g GID` → Assign a custom group ID.

### Example:
```bash
groupadd developers
```
Creates a new group called **developers**.

---

## 🔑 Summary Table

| Command   | Purpose | Example |
|-----------|---------|---------|
| **useradd** | Create a new user | `useradd -m -s /bin/bash amit` |
| **usermod** | Modify existing user | `usermod -aG sudo amit` |
| **passwd**  | Set/change password | `passwd amit` |
| **groupadd** | Create new group | `groupadd developers` |

---

## ✅ Best Practices
- Always set strong passwords for users.
- Use `usermod -aG` instead of `usermod -G` (to avoid removing other groups).
- Regularly audit `/etc/passwd` and `/etc/group` for unused accounts.
