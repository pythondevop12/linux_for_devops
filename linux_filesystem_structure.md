# Linux File System Structure

The Linux file system follows a hierarchical directory structure, also
called a **tree structure**, with the root directory (`/`) at the top.
Each directory has a specific purpose and stores related files.

## 1. **/** (Root Directory)

-   The top-most directory in Linux.
-   All other directories and files are placed under it.
-   Only the root user has permission to write directly to it.

------------------------------------------------------------------------

## 2. **/bin** (Binaries)

-   Stands for **binary**.
-   Contains **essential user command binaries** (programs) needed for
    system operation.
-   Examples:
    -   `ls` → list directory contents
    -   `cp` → copy files
    -   `mv` → move files
    -   `rm` → remove files
    -   `cat` → view file contents
-   These commands are available to all users.

------------------------------------------------------------------------

## 3. **/etc** (Configuration Files)

-   Stores **system-wide configuration files** and shell scripts used to
    boot/start system services.
-   Examples:
    -   `/etc/passwd` → user account information
    -   `/etc/fstab` → file system mount points
    -   `/etc/hostname` → system hostname
    -   `/etc/hosts` → hostnames and IP mapping
-   Configuration changes for services like Apache, SSH, Nginx, etc.,
    are stored here.

------------------------------------------------------------------------

## 4. **/var** (Variable Files)

-   Stores **variable data** that changes during system operation.
-   Common subdirectories:
    -   `/var/log` → log files (system, application, kernel logs)
    -   `/var/spool` → print queues and mail spools
    -   `/var/lib` → data files for services (databases, package manager
        info)
-   This directory grows in size as the system runs.

------------------------------------------------------------------------

## 5. **/home** (User Home Directories)

-   Stores personal files, configurations, and data for **regular
    users**.
-   Each user has a subdirectory:
    -   `/home/user1`
    -   `/home/user2`
-   Contains hidden files (dotfiles) such as `.bashrc`, `.profile`, etc.

------------------------------------------------------------------------

## 6. **/tmp** (Temporary Files)

-   Stores **temporary files** created by applications and the system.
-   Accessible by all users, but files are deleted on reboot or after a
    set time.
-   Example use cases:
    -   Storing temporary installation files
    -   Application cache
    -   Session files

------------------------------------------------------------------------

# Summary Table

  Directory   Purpose
  ----------- -------------------------------------------------
  `/bin`      Essential user commands (binaries)
  `/etc`      Configuration files
  `/var`      Variable data like logs, mail, spool, databases
  `/home`     User home directories
  `/tmp`      Temporary files

------------------------------------------------------------------------

# Key Points

-   Linux follows a **tree-like hierarchical structure** starting at
    `/`.
-   Each directory has a **defined purpose** to keep the system
    organized.
-   Knowledge of these directories is crucial for **system
    administration and troubleshooting**.
