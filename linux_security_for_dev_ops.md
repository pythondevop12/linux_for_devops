# Linux Security for DevOps

This document provides a detailed overview of essential **Linux security concepts and tools** for DevOps engineers. Covered topics:
- Managing **sudo** permissions
- Firewalls (`iptables`, `ufw`, `firewalld`)
- **SSH key-based authentication**
- **File permissions & hardening**
- **SELinux/AppArmor basics**

---

## Table of Contents
1. Introduction
2. Managing sudo Permissions
3. Firewalls
   - `iptables`
   - `ufw`
   - `firewalld`
4. SSH Key-Based Authentication
5. File Permissions & Hardening
6. SELinux/AppArmor Basics
7. Best Practices for DevOps Security
8. Cheatsheet

---

# 1. Introduction

Security is critical for DevOps since servers often host production workloads. Properly managing user permissions, firewall rules, authentication, and security frameworks reduces the attack surface and strengthens defense.

---

# 2. Managing sudo Permissions

### What it is
- `sudo` allows users to run commands as another user (usually root) with controlled access.
- Prevents giving full root access.

### Configuration
- File: `/etc/sudoers` (edited with `visudo`).
- Additional configs: `/etc/sudoers.d/`.

### Examples
- Give user full sudo rights:
```sh
usermod -aG sudo alice
```
- Allow user to run only specific command:
```sh
alice ALL=(ALL) NOPASSWD: /usr/bin/systemctl restart nginx
```
- Allow group access:
```sh
%devops ALL=(ALL) ALL
```

### Tips
- Use `visudo` to prevent syntax errors.
- Limit commands for least privilege.
- Prefer groups over individual user entries.

---

# 3. Firewalls

Firewalls control incoming and outgoing network traffic.

## 3.1 `iptables`

### What it is
- Legacy but powerful firewall tool.
- Works by managing rules in the Linux kernel’s Netfilter framework.

### Common Commands
```sh
iptables -L -v -n        # list rules
iptables -A INPUT -p tcp --dport 22 -j ACCEPT   # allow SSH
iptables -A INPUT -p tcp --dport 80 -j ACCEPT   # allow HTTP
iptables -A INPUT -j DROP # drop everything else
```

### Notes
- Rules are not persistent by default (use `iptables-save`).

---

## 3.2 `ufw` (Uncomplicated Firewall)

### What it is
- Simplified interface to `iptables` (Ubuntu/Debian).

### Common Commands
```sh
ufw enable               # enable firewall
ufw allow 22/tcp         # allow SSH
ufw allow 80,443/tcp     # allow HTTP & HTTPS
ufw deny 3306/tcp        # block MySQL
ufw status verbose       # show rules
```

---

## 3.3 `firewalld`

### What it is
- Dynamic firewall manager (RHEL/CentOS/Fedora).
- Uses **zones** and **services**.

### Common Commands
```sh
systemctl start firewalld
firewall-cmd --get-active-zones
firewall-cmd --zone=public --add-service=http --permanent
firewall-cmd --zone=public --add-port=8080/tcp --permanent
firewall-cmd --reload
```

---

# 4. SSH Key-Based Authentication

### What it is
- Secure method of authentication using cryptographic keys instead of passwords.
- Stronger than passwords and avoids brute-force attacks.

### Steps
1. Generate key pair on client:
```sh
ssh-keygen -t rsa -b 4096 -C "user@example.com"
```
2. Copy public key to server:
```sh
ssh-copy-id user@server
```
(or manually add `~/.ssh/id_rsa.pub` contents to `~/.ssh/authorized_keys` on server)
3. Connect without password:
```sh
ssh user@server
```

### Hardening
- Disable password authentication in `/etc/ssh/sshd_config`:
```sh
PasswordAuthentication no
```
- Restart SSH:
```sh
systemctl restart sshd
```

---

# 5. File Permissions & Hardening

### Linux Permissions
- Every file has **owner, group, others** permissions.
- Use `ls -l` to view.

### Changing ownership & permissions
```sh
chown user:group file.txt
chmod 640 file.txt     # rw- r-- ---
```

### Special Permissions
- **Setuid (s)** → run with file owner’s privileges.
- **Setgid (s)** → run with group’s privileges.
- **Sticky bit (t)** → prevent users from deleting others’ files in shared dirs.

### Hardening Tips
- Remove world-writable permissions (`chmod o-w`).
- Restrict `/etc/shadow` to root-only.
- Use `umask` to set secure default permissions (e.g., `027`).

---

# 6. SELinux/AppArmor Basics

## SELinux (Security-Enhanced Linux)
- Provides **mandatory access control (MAC)**.
- Policies strictly define what processes can access.
- Modes:
  - `enforcing` → block unauthorized actions.
  - `permissive` → only log actions.
  - `disabled`.

### Commands
```sh
getenforce             # check mode
setenforce 1           # enforcing
semanage port -l       # list managed ports
```

## AppArmor
- Alternative to SELinux (Ubuntu/Debian).
- Uses profiles to restrict programs.

### Commands
```sh
apparmor_status
sudo aa-enforce /etc/apparmor.d/usr.bin.nginx
```

---

# 7. Best Practices for DevOps Security

- Use **least privilege** with `sudo`.
- Enable and configure **firewall** properly.
- Use **SSH keys**, disable password login.
- Regularly update and patch systems.
- Monitor logs (`journalctl`, `/var/log/`).
- Apply file permission hardening.
- Enable SELinux/AppArmor in enforcing mode.

---

# 8. Cheatsheet

**sudo**
- `visudo` → safely edit sudoers.
- `user ALL=(ALL) NOPASSWD: /path/cmd` → restrict command.

**firewalls**
- `ufw allow 22/tcp` → allow SSH.
- `firewall-cmd --add-service=http --permanent` → allow HTTP.
- `iptables -L -v -n` → list rules.

**ssh**
- `ssh-keygen -t rsa -b 4096` → generate keys.
- `ssh-copy-id user@host` → install public key.

**permissions**
- `chmod 640 file` → secure file.
- `umask 027` → secure defaults.

**SELinux/AppArmor**
- `getenforce` → check SELinux.
- `apparmor_status` → check AppArmor.

---

This concludes the **Linux Security for DevOps** guide. Following these practices ensures a more secure and reliable infrastructure.

