# Advanced Linux Usage: Networking

This document provides a detailed overview of essential Linux networking commands and utilities, focusing on:
- **ping, curl, wget** (basic connectivity and web requests)
- **netstat, ss, ip, ifconfig** (network interface and socket information)
- **SSH (ssh, scp, sftp)** (secure remote login and file transfer)

The goal is to make it easy for anyone—even beginners—to understand and use these tools effectively.

---

## Table of Contents
1. Introduction to Networking in Linux
2. Testing Connectivity
   - `ping`
   - `curl`
   - `wget`
3. Network Interfaces and Sockets
   - `netstat`
   - `ss`
   - `ip`
   - `ifconfig`
4. Secure Remote Access and File Transfer
   - `ssh`
   - `scp`
   - `sftp`
5. Practical Examples
6. Troubleshooting Tips
7. Cheatsheet

---

# 1. Introduction to Networking in Linux

Linux provides a wide range of tools to:
- Test network connectivity.
- Monitor and configure network interfaces.
- Debug network issues.
- Securely connect to and transfer files between systems.

Networking commands are vital for system administrators, DevOps engineers, developers, and even regular users.

---

# 2. Testing Connectivity

## 2.1 `ping`

### What it is
- `ping` checks if a remote host is reachable by sending **ICMP Echo Request** packets and waiting for **ICMP Echo Reply**.

### Syntax
```sh
ping [options] destination
```

### Common Options
- `-c <count>` → stop after sending `<count>` packets.
- `-i <interval>` → set interval between packets.
- `-t <ttl>` → set time-to-live.
- `-s <size>` → specify packet size.

### Examples
```sh
ping google.com              # test connectivity to google.com
ping -c 4 8.8.8.8            # send 4 packets to Google's DNS server
ping -i 0.5 example.com      # send packets every 0.5 seconds
```

---

## 2.2 `curl`

### What it is
- `curl` transfers data from or to a server using various protocols (HTTP, HTTPS, FTP, etc.).
- Useful for API testing and web debugging.

### Syntax
```sh
curl [options] [URL]
```

### Common Options
- `-I` → fetch headers only.
- `-L` → follow redirects.
- `-o file` → save output to file.
- `-d data` → send POST data.
- `-H` → send custom headers.

### Examples
```sh
curl https://example.com
curl -I https://example.com                # headers only
curl -o file.html https://example.com      # save to file
curl -L http://short.url                   # follow redirects
curl -d "user=admin&pass=123" https://api.example.com/login
```

---

## 2.3 `wget`

### What it is
- `wget` is used to **download files** from the web (HTTP, HTTPS, FTP).
- Unlike `curl`, it is primarily for downloading, not sending data.

### Syntax
```sh
wget [options] [URL]
```

### Common Options
- `-O file` → save with specific name.
- `-c` → resume a partially downloaded file.
- `-r` → recursive download.
- `--limit-rate=200k` → limit download speed.

### Examples
```sh
wget https://example.com/file.zip
wget -O latest.html https://example.com
wget -c bigfile.iso              # resume download
wget -r https://example.com/docs # recursive download
```

---

# 3. Network Interfaces and Sockets

## 3.1 `netstat` (deprecated, replaced by `ss`)

### What it is
- `netstat` shows network connections, routing tables, interface statistics, and listening ports.
- Deprecated in favor of `ss`.

### Syntax
```sh
netstat [options]
```

### Common Options
- `-t` → TCP connections.
- `-u` → UDP connections.
- `-l` → listening sockets.
- `-p` → show process ID and name.
- `-n` → show numeric addresses.

### Example
```sh
netstat -tulnp   # show listening TCP/UDP ports with process info
```

---

## 3.2 `ss` (socket statistics)

### What it is
- `ss` is the modern replacement for `netstat`.
- Faster and more detailed.

### Syntax
```sh
ss [options]
```

### Common Options
- `-t` → TCP sockets.
- `-u` → UDP sockets.
- `-l` → listening.
- `-n` → numeric.
- `-p` → process info.

### Examples
```sh
ss -tulnp            # all listening ports
ss -tn state ESTAB   # established TCP connections
ss -s                # summary statistics
```

---

## 3.3 `ip`

### What it is
- `ip` is the modern tool for managing network interfaces, replacing `ifconfig`.

### Syntax
```sh
ip [object] [command]
```

### Common Objects
- `addr` → IP addresses.
- `link` → interfaces.
- `route` → routing.

### Examples
```sh
ip addr show               # list IP addresses
ip addr add 192.168.1.10/24 dev eth0  # assign IP
ip link set eth0 up        # bring interface up
ip route show              # show routing table
```

---

## 3.4 `ifconfig` (deprecated)

### What it is
- Legacy tool for configuring network interfaces.
- Still found on many systems, but `ip` is recommended.

### Syntax
```sh
ifconfig [interface] [options]
```

### Examples
```sh
ifconfig eth0                # show details for eth0
ifconfig eth0 192.168.1.20   # assign IP
ifconfig eth0 up             # enable interface
```

---

# 4. Secure Remote Access and File Transfer

## 4.1 `ssh` (Secure Shell)

### What it is
- `ssh` provides encrypted remote login between machines.

### Syntax
```sh
ssh [options] user@host
```

### Common Options
- `-p <port>` → specify SSH port (default 22).
- `-i keyfile` → specify private key.
- `-X` → enable X11 forwarding.

### Examples
```sh
ssh user@192.168.1.5
ssh -p 2222 user@remote.server
ssh -i ~/.ssh/id_rsa user@host
```

---

## 4.2 `scp` (Secure Copy)

### What it is
- `scp` copies files securely between hosts using SSH.

### Syntax
```sh
scp [options] source destination
```

### Examples
```sh
scp file.txt user@remote:/home/user/
scp user@remote:/home/user/file.txt ./
scp -r project/ user@remote:/var/www/
```

---

## 4.3 `sftp` (Secure File Transfer Protocol)

### What it is
- `sftp` provides an interactive file transfer session over SSH.

### Syntax
```sh
sftp [options] user@host
```

### Examples
```sh
sftp user@remote
sftp> ls
sftp> get file.txt
sftp> put localfile.txt
sftp> exit
```

---

# 5. Practical Examples

1. **Check if server is alive**
   ```sh
   ping -c 4 example.com
   ```
2. **Download a file**
   ```sh
   wget https://example.com/file.tar.gz
   ```
3. **Test API endpoint**
   ```sh
   curl -X GET https://api.example.com/users
   ```
4. **Check listening ports**
   ```sh
   ss -tulnp
   ```
5. **Bring interface up**
   ```sh
   ip link set eth0 up
   ```
6. **SSH into server with key**
   ```sh
   ssh -i ~/.ssh/id_rsa user@host
   ```
7. **Copy logs from remote server**
   ```sh
   scp user@server:/var/log/syslog ./
   ```
8. **Transfer files interactively**
   ```sh
   sftp user@server
   ```

---

# 6. Troubleshooting Tips

- If `ping` fails, check firewall or ICMP filtering.
- If `ssh` fails, verify:
  - Host is reachable (`ping`/`ss`/`nmap`).
  - Correct port is open.
  - Keys/credentials are valid.
- Use `ss -tulnp` to check if services are listening.
- Use `ip addr` and `ip route` to verify network configuration.
- Always prefer `ip` and `ss` over `ifconfig` and `netstat`.

---

# 7. Cheatsheet

**Connectivity**
- `ping -c 4 host` → test reachability.
- `curl -I URL` → check HTTP headers.
- `wget URL` → download file.

**Network Info**
- `ss -tulnp` → list listening ports.
- `ip addr` → show IP addresses.
- `ip route` → show routes.

**Remote Access**
- `ssh user@host` → login.
- `scp file user@host:/path` → copy file.
- `sftp user@host` → interactive file transfer.

---


