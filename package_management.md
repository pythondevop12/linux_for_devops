# Package Management in Linux

Package management is a crucial part of Linux administration. It allows
you to install, update, remove, and manage software packages. Different
Linux distributions use different package managers.

------------------------------------------------------------------------

# 1. **Debian/Ubuntu Based Systems**

Debian-based distributions (like Ubuntu, Linux Mint) use `.deb` packages
and tools like `apt` and `dpkg`.

------------------------------------------------------------------------

## **apt (Advanced Package Tool)**

The `apt` command is the most common package manager in Debian/Ubuntu.

### Common Commands:

-   Update package list:

``` bash
sudo apt update
```

-   Upgrade installed packages:

``` bash
sudo apt upgrade
```

-   Install a package:

``` bash
sudo apt install package_name
```

-   Remove a package:

``` bash
sudo apt remove package_name
```

-   Search for a package:

``` bash
apt search package_name
```

-   Show details of a package:

``` bash
apt show package_name
```

------------------------------------------------------------------------

## **dpkg (Debian Package Manager)**

`dpkg` is a low-level tool for managing `.deb` packages directly.

### Common Commands:

-   Install a `.deb` package:

``` bash
sudo dpkg -i package_name.deb
```

-   Remove a package:

``` bash
sudo dpkg -r package_name
```

-   List installed packages:

``` bash
dpkg -l
```

-   Get details of a package:

``` bash
dpkg -s package_name
```

-   Find the files installed by a package:

``` bash
dpkg -L package_name
```

------------------------------------------------------------------------

# 2. **RHEL/CentOS Based Systems**

Red Hat-based distributions (like CentOS, Fedora) use `.rpm` packages
and tools like `yum`, `dnf`, and `rpm`.

------------------------------------------------------------------------

## **yum (Yellowdog Updater Modified)**

`yum` is the traditional package manager for RHEL and CentOS.

### Common Commands:

-   Update package list and upgrade:

``` bash
sudo yum update
```

-   Install a package:

``` bash
sudo yum install package_name
```

-   Remove a package:

``` bash
sudo yum remove package_name
```

-   Search for a package:

``` bash
yum search package_name
```

-   Get info about a package:

``` bash
yum info package_name
```

------------------------------------------------------------------------

## **dnf (Dandified YUM)**

`dnf` is the next-generation package manager that replaces `yum` in
newer versions of Fedora, CentOS, and RHEL.

### Common Commands:

-   Update all packages:

``` bash
sudo dnf update
```

-   Install a package:

``` bash
sudo dnf install package_name
```

-   Remove a package:

``` bash
sudo dnf remove package_name
```

-   Search for a package:

``` bash
dnf search package_name
```

-   Get package info:

``` bash
dnf info package_name
```

------------------------------------------------------------------------

## **rpm (Red Hat Package Manager)**

`rpm` is a low-level tool for handling `.rpm` packages directly.

### Common Commands:

-   Install an `.rpm` package:

``` bash
sudo rpm -i package_name.rpm
```

-   Upgrade an `.rpm` package:

``` bash
sudo rpm -U package_name.rpm
```

-   Remove a package:

``` bash
sudo rpm -e package_name
```

-   List installed packages:

``` bash
rpm -qa
```

-   Verify a package:

``` bash
rpm -V package_name
```

-   Show details of a package:

``` bash
rpm -qi package_name
```

------------------------------------------------------------------------

# Summary

-   **Debian/Ubuntu**: Use `apt` for higher-level management and `dpkg`
    for direct `.deb` package handling.
-   **RHEL/CentOS**: Use `yum` or `dnf` for higher-level management and
    `rpm` for direct `.rpm` package handling.

These tools allow Linux administrators to efficiently install, update,
and manage software packages across different distributions.
