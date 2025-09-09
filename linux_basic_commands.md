
# Linux Basic Commands

This document provides a detailed overview of commonly used **Linux basic commands** with explanations and examples.

---

## 1. File and Directory Management

- **pwd**  
  Prints the current working directory.  
  ```bash
  pwd
  ```

- **ls**  
  Lists files and directories.  
  ```bash
  ls
  ls -l      # long listing format
  ls -a      # show hidden files
  ls -lh     # human-readable sizes
  ```

- **cd**  
  Changes the current directory.  
  ```bash
  cd /home/user
  cd ..       # go one level up
  cd ~        # go to home directory
  ```

- **mkdir**  
  Creates a new directory.  
  ```bash
  mkdir new_folder
  mkdir -p parent/child  # create parent and child directories
  ```

- **rmdir**  
  Removes an empty directory.  
  ```bash
  rmdir old_folder
  ```

- **rm**  
  Removes files or directories.  
  ```bash
  rm file.txt
  rm -r folder/  # remove directory recursively
  rm -f file.txt # force remove without prompt
  ```

- **touch**  
  Creates an empty file or updates file timestamp.  
  ```bash
  touch newfile.txt
  ```

- **cp**  
  Copies files or directories.  
  ```bash
  cp file1.txt file2.txt
  cp -r dir1/ dir2/
  ```

- **mv**  
  Moves or renames files and directories.  
  ```bash
  mv file.txt /tmp/
  mv oldname.txt newname.txt
  ```

---

## 2. File Viewing

- **cat**  
  Displays file contents.  
  ```bash
  cat file.txt
  ```

- **less**  
  View file content one page at a time.  
  ```bash
  less file.txt
  ```

- **more**  
  Similar to `less` but with limited navigation.  
  ```bash
  more file.txt
  ```

- **head**  
  Shows the first 10 lines of a file.  
  ```bash
  head file.txt
  head -n 20 file.txt
  ```

- **tail**  
  Shows the last 10 lines of a file.  
  ```bash
  tail file.txt
  tail -f logfile.log  # follow log updates
  ```

---

## 3. User Management

- **whoami**  
  Shows current logged-in user.  
  ```bash
  whoami
  ```

- **who**  
  Shows who is logged in.  
  ```bash
  who
  ```

- **id**  
  Displays user ID (UID), group ID (GID), and groups.  
  ```bash
  id
  ```

- **adduser / useradd**  
  Creates a new user.  
  ```bash
  sudo adduser newuser
  ```

- **passwd**  
  Changes user password.  
  ```bash
  passwd
  ```

- **su**  
  Switch user account.  
  ```bash
  su - username
  ```

- **sudo**  
  Run a command as a superuser.  
  ```bash
  sudo apt update
  ```

---

## 4. File Permissions

- **chmod**  
  Change file permissions.  
  ```bash
  chmod 755 file.sh
  chmod u+x file.sh
  ```

- **chown**  
  Change file ownership.  
  ```bash
  sudo chown user:user file.txt
  ```

- **chgrp**  
  Change group ownership.  
  ```bash
  sudo chgrp developers file.txt
  ```

- **umask**  
  View or set default permissions for new files.  
  ```bash
  umask
  umask 022
  ```

---

## 5. Process Management

- **ps**  
  Shows running processes.  
  ```bash
  ps
  ps aux | grep python
  ```

- **top**  
  Interactive process viewer.  
  ```bash
  top
  ```

- **htop** (if installed)  
  Improved process viewer.  
  ```bash
  htop
  ```

- **kill**  
  Terminate a process.  
  ```bash
  kill 1234     # by PID
  kill -9 1234  # force kill
  ```

- **jobs**  
  Shows background jobs.  
  ```bash
  jobs
  ```

- **bg / fg**  
  Run jobs in background/foreground.  
  ```bash
  bg %1
  fg %1
  ```

---

## 6. Disk Usage & System Info

- **df**  
  Show disk space usage.  
  ```bash
  df -h
  ```

- **du**  
  Show directory/file size.  
  ```bash
  du -sh *
  ```

- **free**  
  Show memory usage.  
  ```bash
  free -h
  ```

- **uptime**  
  Show system uptime and load average.  
  ```bash
  uptime
  ```

- **uname**  
  Show system information.  
  ```bash
  uname -a
  ```

- **hostname**  
  Show or set hostname.  
  ```bash
  hostname
  ```

---

## 7. Searching & Finding Files

- **find**  
  Search for files.  
  ```bash
  find / -name file.txt
  find . -type f -name "*.log"
  ```

- **locate**  
  Find files using database (faster than `find`).  
  ```bash
  locate file.txt
  ```

- **which**  
  Locate command binary.  
  ```bash
  which python
  ```

- **whereis**  
  Locate command with binary, source, and man page.  
  ```bash
  whereis ls
  ```

- **grep**  
  Search inside files.  
  ```bash
  grep "error" logfile.log
  grep -r "main" /project/
  ```

---

## 8. Archiving & Compression

- **tar**  
  Archive files.  
  ```bash
  tar -cvf archive.tar file1 file2
  tar -xvf archive.tar
  tar -czvf archive.tar.gz folder/
  tar -xzvf archive.tar.gz
  ```

- **zip / unzip**  
  Compress and extract files.  
  ```bash
  zip archive.zip file1 file2
  unzip archive.zip
  ```

- **gzip / gunzip**  
  Compress or decompress files.  
  ```bash
  gzip file.txt
  gunzip file.txt.gz
  ```

---

## 9. Networking

- **ping**  
  Test connectivity.  
  ```bash
  ping google.com
  ```

- **curl**  
  Transfer data from/to a server.  
  ```bash
  curl https://example.com
  ```

- **wget**  
  Download files.  
  ```bash
  wget https://example.com/file.zip
  ```

- **ifconfig** *(deprecated, use `ip`)*  
  Show network interfaces.  
  ```bash
  ifconfig
  ```

- **ip**  
  Modern replacement for ifconfig.  
  ```bash
  ip a
  ip route
  ```

- **netstat** *(if installed)*  
  Show network connections.  
  ```bash
  netstat -tuln
  ```

- **ss**  
  Show socket statistics.  
  ```bash
  ss -tuln
  ```

- **scp**  
  Copy files over SSH.  
  ```bash
  scp file.txt user@host:/path/
  ```

- **ssh**  
  Connect to remote server.  
  ```bash
  ssh user@host
  ```

---

## 10. Package Management

- **Debian/Ubuntu**  
  ```bash
  sudo apt update
  sudo apt install package_name
  sudo apt remove package_name
  sudo apt upgrade
  ```

- **RHEL/CentOS/Fedora**  
  ```bash
  sudo yum install package_name
  sudo yum remove package_name
  sudo dnf install package_name
  ```

---

## 11. Others

- **echo**  
  Print text.  
  ```bash
  echo "Hello World"
  ```

- **history**  
  Show command history.  
  ```bash
  history
  ```

- **clear**  
  Clear terminal screen.  
  ```bash
  clear
  ```

- **man**  
  Show manual for a command.  
  ```bash
  man ls
  ```

---

# Conclusion

These are the **most commonly used Linux basic commands**. Mastering them will help you efficiently navigate and manage a Linux system.
