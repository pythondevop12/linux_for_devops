# Disk Management in Linux

This document provides a detailed guide to essential disk management commands in Linux:
- **`df`** – show disk space usage.
- **`du`** – show directory/file space usage.
- **`mount`** – attach filesystems.
- **`umount`** – detach filesystems.
- **`lsblk`** – list block devices.
- **`fdisk`** – partition a disk.

The goal is to help anyone (beginners or advanced users) understand how to view, manage, and organize disk space on Linux.

---

## Table of Contents
1. Introduction
2. Viewing Disk Usage
   - `df`
   - `du`
3. Mounting and Unmounting
   - `mount`
   - `umount`
4. Viewing Block Devices
   - `lsblk`
5. Partitioning Disks
   - `fdisk`
6. Practical Examples
7. Troubleshooting Tips
8. Cheatsheet

---

# 1. Introduction

Disk management in Linux involves monitoring available space, identifying large files, mounting storage devices, and creating or modifying disk partitions.

The tools covered here are standard on most Linux distributions.

---

# 2. Viewing Disk Usage

## 2.1 `df` – Disk Free

### What it is
- Displays the **disk space usage** of mounted filesystems.

### Syntax
```sh
df [options] [path]
```

### Common Options
- `-h` → human-readable (e.g., KB, MB, GB).
- `-T` → show filesystem type.
- `-i` → show inode usage.

### Examples
```sh
df -h                # human-readable disk usage
df -Th               # include filesystem type
```

---

## 2.2 `du` – Disk Usage

### What it is
- Shows how much space **directories and files** take up.

### Syntax
```sh
du [options] [path]
```

### Common Options
- `-h` → human-readable.
- `-s` → summary for directory.
- `-c` → show total.
- `--max-depth=N` → limit depth of subdirectories.

### Examples
```sh
du -sh /var/log       # total size of /var/log
du -h --max-depth=1   # sizes of subdirectories in current directory
du -ch *              # sizes of all items with total
```

---

# 3. Mounting and Unmounting

## 3.1 `mount`

### What it is
- Attaches a filesystem (e.g., USB drive, ISO image) to a directory.

### Syntax
```sh
mount [options] device dir
```

### Examples
```sh
mount /dev/sdb1 /mnt      # mount /dev/sdb1 to /mnt
mount -o ro /dev/sr0 /mnt # mount CD-ROM as read-only
```

### Notes
- The directory (`/mnt` in examples) must exist.
- Files become accessible under the mount point.

---

## 3.2 `umount`

### What it is
- Detaches a filesystem from its mount point.

### Syntax
```sh
umount [options] device|dir
```

### Examples
```sh
umount /mnt
umount /dev/sdb1
```

### Notes
- You must **close all files** in use from that filesystem before unmounting.
- If busy, use:
```sh
umount -l /mnt   # lazy unmount
umount -f /mnt   # force unmount (dangerous)
```

---

# 4. Viewing Block Devices

## 4.1 `lsblk`

### What it is
- Lists information about **block devices** (disks, partitions).

### Syntax
```sh
lsblk [options]
```

### Common Options
- `-f` → show filesystem type.
- `-o` → specify columns.

### Examples
```sh
lsblk
lsblk -f             # show filesystem and UUID
lsblk -o NAME,SIZE,TYPE,MOUNTPOINT
```

---

# 5. Partitioning Disks

## 5.1 `fdisk`

### What it is
- Interactive tool to create, delete, and manage disk partitions.

### Syntax
```sh
fdisk [device]
```

### Common Commands (inside `fdisk`)
- `m` → help menu.
- `p` → print partition table.
- `n` → new partition.
- `d` → delete partition.
- `w` → write changes.
- `q` → quit without saving.

### Example Workflow
```sh
sudo fdisk /dev/sdb
# Commands inside fdisk:
n   → new partition
w   → write changes
```

### Notes
- Always back up data before using `fdisk`.
- After creating partitions, format them using `mkfs` (e.g., `mkfs.ext4 /dev/sdb1`).

---

# 6. Practical Examples

1. **Check overall disk space**
```sh
df -h
```

2. **Find largest directories**
```sh
du -sh * | sort -h
```

3. **Mount a USB drive**
```sh
mount /dev/sdb1 /media/usb
```

4. **Unmount USB drive**
```sh
umount /media/usb
```

5. **View disk partitions**
```sh
lsblk -f
```

6. **Create new partition**
```sh
fdisk /dev/sdb
```

---

# 7. Troubleshooting Tips

- If `mount` fails → ensure directory exists and device is formatted.
- If `umount` fails → close all files/programs using the mount.
- Use `lsblk` and `dmesg | tail` after plugging in new storage to detect devices.
- After partitioning with `fdisk`, always format and mount partitions.

---

# 8. Cheatsheet

**Disk Usage**
- `df -h` → show filesystem disk space.
- `du -sh dir` → size of directory.

**Mounting**
- `mount device dir` → mount filesystem.
- `umount dir` → unmount filesystem.

**Devices**
- `lsblk` → list block devices.

**Partitioning**
- `fdisk /dev/sdX` → manage partitions.

---

This concludes the **Linux Disk Management** guide. Practice using these commands with non-critical disks or test environments before applying to production systems.

