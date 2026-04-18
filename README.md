<div align="center">

# 🐧 Linux Administration Notes

### A comprehensive reference guide for Linux system administration — from fundamentals to advanced configurations.

[![Topics](https://img.shields.io/badge/Topics-15-blue?style=flat-square)](#table-of-contents)
[![Level](https://img.shields.io/badge/Level-Beginner%20to%20Intermediate-green?style=flat-square)](#)
[![OS](https://img.shields.io/badge/OS-RHEL%20%7C%20CentOS%20%7C%20Amazon%20Linux-red?style=flat-square)](#)

</div>

---

## 📋 Table of Contents

| # | Topic | Key Commands |
|---|-------|-------------|
| 01 | [Linux OS & File System Basics](#01-linux-os--file-system-basics) | `ls`, `cd`, `mkdir`, `cat`, `cp`, `mv` |
| 02 | [Vi / Vim Editor](#02-vi--vim-editor) | `i`, `dd`, `yy`, `:wq!`, `:%s` |
| 03 | [User Management](#03-user-management) | `useradd`, `usermod`, `passwd`, `chage` |
| 04 | [File Permissions](#04-file-permissions) | `chmod`, `chown`, `chgrp`, `umask` |
| 05 | [Special Permissions & ACL](#05-special-permissions--acl) | `SUID`, `SGID`, `Sticky Bit`, `setfacl` |
| 06 | [Linux Links](#06-linux-links) | `ln`, `ln -s`, `unlink` |
| 07 | [File Compression](#07-file-compression) | `zip`, `gzip`, `tar` |
| 08 | [Process Management](#08-process-management) | `top`, `ps`, `kill`, `nice`, `renice` |
| 09 | [Services & Daemons](#09-services--daemons) | `systemctl` |
| 10 | [Package Management – RPM](#10-package-management--rpm) | `rpm -ivh`, `rpm -qa`, `rpm -evh` |
| 11 | [Package Management – YUM](#11-package-management--yum) | `yum install`, `yum remove`, `dnf` |
| 12 | [YUM Repository Setup](#12-yum-repository-setup) | `createrepo`, `yum.repos.d` |
| 13 | [SSH – Secure Shell](#13-ssh--secure-shell) | `ssh`, `scp`, `ssh-keygen` |
| 14 | [Swap Memory](#14-swap-memory) | `fallocate`, `mkswap`, `swapon` |
| 15 | [Linux Networking](#15-linux-networking) | `nmcli`, `ip a`, `netstat` |

---

## 01. Linux OS & File System Basics

### What is Linux?
Linux is a **free and open-source operating system** built upon the Linux kernel, developed by **Linus Torvalds in 1991**. It is used everywhere — from personal computers to supercomputers and cloud servers.

| Component | Description |
|-----------|-------------|
| **Kernel** | Core of the OS — interface between hardware and processes |
| **Shell** | Translator between user and the OS |
| **Root (`/`)** | Top-level directory; all files and directories live under it |

### File System Path Types

| Type | Description | Example |
|------|-------------|---------|
| **Absolute Path** | Full path starting from root `/` | `/home/user/documents/file.txt` |
| **Relative Path** | Path relative to current directory | `documents/file.txt` |

### Directory Structure

```
/                       ← Root directory (top level)
├── home/               ← User home directories
├── etc/                ← Configuration files
├── var/                ← Variable data (logs, mail)
├── usr/
│   ├── bin/            ← Normal user commands
│   └── sbin/           ← Root user commands
├── tmp/                ← Temporary files
└── proc/               ← Kernel/process info (virtual)
```

### Essential Commands

```bash
# Directory Operations
mkdir dir_name              # Create a directory
mkdir -p parent/child       # Create nested directories
mkdir dir{1..10}            # Create 10 directories at once
rmdir dir_name              # Remove empty directory
rmdir dir{1..10}            # Remove multiple directories

# Navigation
pwd                         # Print current working directory
cd ..                       # Move up one directory level
cd /home/user               # Navigate to absolute path

# File Operations
touch filename              # Create an empty file
cp -r source/ dest/         # Copy files or directories
mv oldname newname          # Move or rename files
rm filename                 # Remove a file
rm -r directory/            # Remove a directory recursively

# Listing Files
ls                          # List files (no hidden)
ls -l                       # Long format listing
ls -a                       # Show all including hidden files
ls -lh                      # Human-readable file sizes
ls -lt                      # Sort by modification time
ls -ls                      # Sort by size (descending)
ls -R                       # List recursively
ls -i                       # Show inode numbers
ls -ld dir/                 # Show directory details only

# Viewing File Content
cat filename                # Display file content
cat > filename              # Create/overwrite file (Ctrl+D to save)
cat >> filename             # Append to existing file
head filename               # First 10 lines
head -n 20 filename         # First 20 lines
tail filename               # Last 10 lines
tail -n 20 filename         # Last 20 lines

# Switch User
su -                        # Switch to root
su - username               # Switch to another user
```

---

## 02. Vi / Vim Editor

The **Vim editor** is used to create and edit text files on the command line. It operates in **3 modes**:

```
Normal/Command Mode  →  [Esc]  →  Navigate and use shortcuts
Insert Mode         →  [i]    →  Type and edit text
Execution Mode      →  [:]    →  Run save/quit commands
```

### Switching Between Modes

| Key | Action |
|-----|--------|
| `Esc` | Return to Command Mode from any mode |
| `i` | Insert before cursor |
| `I` | Insert at beginning of line |
| `a` | Append after cursor |
| `A` | Append at end of line |
| `o` | New line below cursor |
| `O` | New line above cursor |

### Command Mode Shortcuts

| Command | Action |
|---------|--------|
| `yy` | Copy (yank) current line |
| `nyy` | Copy n lines |
| `p` | Paste after cursor |
| `P` (Shift+P) | Paste before cursor |
| `dd` | Cut / Delete current line |
| `x` | Delete character under cursor |
| `gg` | Go to first line |
| `G` (Shift+G) | Go to last line |
| `ZZ` | Save and quit |
| `h / l / k / j` | Move left / right / up / down |

### Execution Mode Commands

```vim
:q!                   " Quit without saving (force)
:w                    " Save only
:wq!                  " Save and quit
:n                    " Go to line number n
:set number           " Show line numbers
:set nonumber         " Hide line numbers
:nd                   " Delete line n
:$d                   " Delete last line
:1,nd                 " Delete lines 1 through n
:%s/oldtext/newtext   " Replace ALL occurrences in file
:ns/oldtext/newtext   " Replace on line n only
:/searchterm          " Search for term (highlighted)
:nohlsearch           " Clear search highlight
:X                    " Set password for file
```

---

## 03. User Management

### Types of Linux Users

| Type | Description |
|------|-------------|
| **Root / Superuser** | Full system access and all permissions |
| **Static System User** | Auto-created by OS (e.g., daemon, bin) |
| **System User** | Runs background services/daemons |
| **Normal User** | Regular human user for day-to-day tasks |

### Where User Info is Stored

| File | Contents |
|------|----------|
| `/etc/passwd` | Username, UID, GID, home directory, shell |
| `/etc/shadow` | Encrypted passwords and expiry settings |
| `/etc/group` | Group names, GID, group members |
| `/etc/gshadow` | Group passwords and admins |
| `/home/username/` | User's personal home directory |
| `/var/spool/mail/username` | User's mail spool |

### File Format Reference

```
/etc/passwd:  username : password : UID : GID : comment : home_dir : shell
/etc/shadow:  username : password : last_change : min : max : warn : inactive : expire
/etc/group:   groupname : password : GID : members
```

### User Commands

```bash
# Create Users
useradd username                      # Basic user creation
useradd -g groupname username         # Create with primary group
useradd -u 1500 username              # Create with specific UID
useradd -c "Full Name" username       # Create with comment/description
useradd -M username                   # Create without home directory
useradd -s /bin/bash username         # Create with specific shell
useradd -d /home/custom username      # Create with custom home directory
useradd -e "2025-12-31" username      # Create with account expiry date

# Delete Users
userdel -r username                   # Delete user AND home directory

# Modify Users
usermod -g newgroup username          # Change primary group
usermod -u 1600 username              # Change UID
usermod -c "New Name" username        # Update comment
usermod -s /bin/sh username           # Change login shell
usermod -d /new/home username         # Change home directory
usermod -e "2026-06-01" username      # Update expiry date
usermod -l newname oldname            # Rename user account
usermod -L username                   # Lock user account
usermod -U username                   # Unlock user account

# Password Management
passwd username                       # Set/change password
passwd -d username                    # Delete password
passwd -l username                    # Lock password (adds ! in shadow)
passwd -u username                    # Unlock password
passwd -S username                    # Check password status
```

### Password Aging (chage)

```bash
chage -l username           # Display all aging/shadow details
chage -m 5 username         # Minimum days before password can change
chage -M 90 username        # Maximum days before password must change
chage -w 7 username         # Warning days before expiry
chage -I 3 username         # Inactive days after expiry before lock
chage username              # Interactive: modify all aging settings
```

### Group Commands

```bash
groupadd groupname                    # Create a group
groupdel groupname                    # Delete a group
groupmod -g 1500 groupname            # Change group GID
groupmod -n newname oldname           # Rename a group

gpasswd -a username groupname         # Add user to a group
gpasswd -d username groupname         # Remove user from a group
usermod -aG groupname username        # Add user to secondary group
```

---

## 04. File Permissions

### Permission Structure

```
-  rwx  rwx  rwx
│   │    │    └── Others (o) — everyone else
│   │    └─────── Group  (g) — group members
│   └──────────── Owner  (u) — file owner
└──────────────── File type: - (file), d (dir), l (link)
```

### Permission Values

| Symbol | Name | Numeric | On a File | On a Directory |
|--------|------|---------|-----------|----------------|
| `r` | Read | `4` | View file content | List files with `ls` |
| `w` | Write | `2` | Modify content | Create/delete files inside |
| `x` | Execute | `1` | Run as script | Enter with `cd` |

### Default Permissions

| Type | Full | umask | Default |
|------|------|-------|---------|
| File | `666` | `022` | `644` (rw-r--r--) |
| Directory | `777` | `022` | `755` (rwxr-xr-x) |

### chmod — Change Permissions

```bash
# Numeric (Octal) method
chmod 755 filename          # rwxr-xr-x  (owner full, group/others read+exec)
chmod 644 filename          # rw-r--r--  (owner read+write, others read only)
chmod 777 filename          # rwxrwxrwx  (full access for everyone)
chmod 600 filename          # rw-------  (owner only)
chmod 400 filename          # r--------  (read-only, owner only)

# Symbolic method
chmod u+x filename          # Add execute for owner
chmod g-w filename          # Remove write from group
chmod o+r filename          # Add read for others
chmod a+x filename          # Add execute for all (a = ugo)
chmod u=rwx,g=rx,o=r file   # Set exact permissions

# Recursive
chmod -R 755 directory/
```

### chown & chgrp — Change Ownership

```bash
chown owner filename                  # Change file owner
chgrp group filename                  # Change file group
chown owner:group filename            # Change both owner and group
chown -R owner:group directory/       # Recursive ownership change
```

### umask — Default Creation Mask

```bash
umask                       # View current umask value (default: 022)
umask 027                   # Set new umask for current session

# To change permanently for all users:
vi /etc/profile             # or /etc/login.defs
```

---

## 05. Special Permissions & ACL

### Types of Special Permissions

| Permission | Symbol | Numeric | Purpose |
|------------|--------|---------|---------|
| **SUID** | `s` in owner execute field | `4` | Run file as its owner |
| **SGID** | `s` in group execute field | `2` | Inherit group for new files |
| **Sticky Bit** | `t` in others execute field | `1` | Protect files in shared dirs |

### SUID — Set User ID

When set on an executable, users who run it **temporarily gain the owner's permissions** (useful for commands like `passwd`).

```bash
chmod u+s filename          # Symbolic — shows as 's' in ls -l
chmod 4755 filename         # Numeric
ls -l /usr/bin/passwd       # Example: -rwsr-xr-x
```

### SGID — Set Group ID

When set on a **directory**, new files created inside inherit the **directory's group**.

```bash
chmod g+s directory/        # Symbolic
chmod 2755 directory/       # Numeric
chmod g-s directory/        # Remove SGID
```

### Sticky Bit

When set on a shared directory, **only the file owner or root** can delete files — even if others have write access.

```bash
chmod +t directory/         # Symbolic
chmod 1755 directory/       # Numeric
ls -ld /tmp                 # Example: drwxrwxrwt (public dir with sticky)
```

### ACL — Access Control List

ACL provides **granular per-user or per-group access**, beyond the standard `rwx` model.

```bash
getfacl filename                          # View ACL settings

setfacl -m u:username:rwx filename        # Grant specific user access
setfacl -m g:groupname:rx filename        # Grant specific group access

setfacl -x u:username filename            # Remove user ACL entry
setfacl -b filename                       # Remove ALL ACL entries
```

---

## 06. Linux Links

### Soft Link (Symbolic Link)

A soft link is a **shortcut or pointer** to another file or directory — similar to Windows shortcuts.

- Different **inode number** from the source
- Works for both **files and directories**
- If source is deleted → link **breaks**

```bash
# Create soft link
ln -s /path/to/source /path/to/link

# Example
ln -s /var/log/messages /home/user/mylog

# Remove soft link
unlink linkname
```

### Hard Link

A hard link is a **direct reference** to the same data blocks on disk — a true mirror.

- **Same inode number** as the original
- Works only for **files** (not directories)
- Deleting the source does **not** destroy data

```bash
# Create hard link
ln /path/to/source /path/to/hardlink

# Remove hard link
unlink filename
```

### Comparison Table

| Feature | Soft Link | Hard Link |
|---------|-----------|-----------|
| Inode number | Different | Same |
| Works on directories | ✅ Yes | ❌ No |
| Source deleted | Link breaks | Data still accessible |
| Cross-filesystem | ✅ Yes | ❌ No |
| Syntax | `ln -s source dest` | `ln source dest` |

---

## 07. File Compression

### ZIP

Best for **sharing across platforms** (Windows, Linux, Mac).

```bash
# Compress
zip archive.zip filename            # Single file
zip archive.zip file1 file2 file3   # Multiple files
zip -r archive.zip directory/       # Directory (recursive)

# Inspect without extracting
zip -sf archive.zip

# Decompress
unzip archive.zip

# Delete archive
rm -rf archive.zip
```

### GZIP

Compresses a **single file**, replacing it with a `.gz` version.

```bash
gzip filename                       # Compress (removes original)
gzip -k filename                    # Compress (keep original)
gzip -d filename.gz                 # Decompress
gzip -l filename.gz                 # Show compression details
gzip -r directory/                  # Compress all files in dir recursively
```

### TAR (Tape Archive)

TAR **bundles multiple files** into one archive. Combine with gzip/bzip2 for compressed archives.

```bash
# Create archives
tar -cvf  archive.tar file/dir       # Bundle only (no compression)
tar -czvf archive.tar.gz file/dir    # Bundle + GZIP compression  (.tar.gz)
tar -cjvf archive.tar.bz2 file/dir   # Bundle + BZIP2 compression (.tar.bz2)

# Extract archives
tar -xvf  archive.tar               # Extract TAR
tar -xzvf archive.tar.gz            # Extract TAR+GZIP
tar -xjvf archive.tar.bz2           # Extract TAR+BZIP2

# List contents (without extracting)
tar -tvf archive.tar

# Add file to existing archive
tar -rvf archive.tar newfile
```

**TAR Flags Reference:**

| Flag | Meaning |
|------|---------|
| `c` | Create archive |
| `x` | Extract archive |
| `t` | List contents |
| `v` | Verbose output |
| `f` | Specify filename |
| `z` | GZIP compression |
| `j` | BZIP2 compression |

---

## 08. Process Management

### What is a Process?

An **instance of a running program** is called a process. Every command you run creates a new process.

| Type | Description |
|------|-------------|
| **Foreground** | Runs in terminal, takes direct user input |
| **Background** | Runs independently, terminal remains free |

### Viewing Processes

```bash
top                                 # Real-time interactive process viewer
ps efa                              # Snapshot of all processes (full detail)
ps ef                               # Snapshot (start to end)
ps aux                              # All processes with CPU/MEM stats
ps aux | less                       # Scrollable process list
ps aux | more                       # Paginated process list
ps -eo pid,ppid,cmd,%mem,%cpu       # Custom output columns
```

### `top` Interactive Keys

| Key | Action |
|-----|--------|
| `z` | Toggle color mode |
| `k` | Kill a process by PID |
| `c` | Show full command path |
| `u` | Filter by specific user |
| `n` | Set max number of processes shown |
| `d` or `s` | Set refresh delay time |

### Process Priority — Nice Value

```
Range: -20 (highest CPU priority) to 19 (lowest CPU priority)
Default: 0   |   Root can set any value   |   Normal users can only increase (make nicer)
```

```bash
# Set priority for NEW process
nice -n 10 command              # Lower priority
nice -n -20 command             # Highest priority (root only)

# Change priority of RUNNING process
renice -n 5 -p PID              # Change by PID

# View nice values
ps -eo pid,ni,cmd
```

### Foreground / Background Job Control

```bash
command &                       # Start process in background
Ctrl + Z                        # Pause (suspend) current process
bg                              # Resume paused process in background
fg                              # Bring background process to foreground
jobs                            # List all background/stopped jobs
```

### Kill Signals

| Signal | Number | Usage |
|--------|--------|-------|
| `SIGHUP` | 1 | Reload/restart process |
| `SIGKILL` | 9 | Force kill — immediate (no cleanup) |
| `SIGTERM` | 15 | Graceful termination (default) |
| `SIGCONT` | 18 | Resume stopped process |
| `SIGSTOP` | 19 | Pause/suspend process |

```bash
kill -9 PID                     # Force kill by PID
kill -15 PID                    # Graceful terminate
killall processname             # Kill all processes by name
pkill processname               # Kill by name pattern
```

### System Resource Commands

```bash
nproc                           # Number of CPU cores
cat /proc/cpuinfo               # Detailed CPU information
free -h                         # RAM and swap usage (human-readable)
free -m                         # RAM and swap in MB
df -h                           # Disk usage per partition
w                               # Logged-in users + system load
```

---

## 09. Services & Daemons

### Service vs Daemon

| Term | Description |
|------|-------------|
| **Service** | Manually installed via package manager; must be manually enabled; runs in background |
| **Daemon** | Starts automatically at boot; essential for system operation; always running in background |

### systemctl Command Reference

```bash
# Check Status
systemctl status servicename          # Full status with logs
systemctl -t service                  # List all active services

# Start / Stop / Restart
systemctl start servicename           # Start a service now
systemctl stop servicename            # Stop a running service
systemctl restart servicename         # Restart (stop + start)
systemctl reload servicename          # Reload config without restart

# Enable / Disable at Boot
systemctl enable servicename          # Auto-start at boot
systemctl disable servicename         # Do not start at boot
systemctl enable --now servicename    # Enable AND start immediately
systemctl disable --now servicename   # Disable AND stop immediately
```

### Common Service Examples

```bash
systemctl status sshd                 # Check SSH service
systemctl start httpd                 # Start Apache web server
systemctl enable httpd                # Auto-start Apache at boot
systemctl restart NetworkManager      # Restart networking
systemctl disable firewalld           # Disable firewall
```

---

## 10. Package Management – RPM

### What is RPM?

**RPM (Red Hat Package Manager)** handles `.rpm` packages on RHEL, CentOS, and Fedora. Unlike YUM, RPM does **not automatically resolve dependencies**.

### Package Naming Convention

```
acl      -    2.2.53    -    1.el9    .    x86_64    .    rpm
 │              │                │              │              │
Name         Version          Release         Arch         Extension
```

### Package Repositories

| Repository | Purpose |
|------------|---------|
| **AppStream** | Application packages; supports multiple versions |
| **BaseOS** | Core OS packages; foundational system components |

### RPM Commands

```bash
# Install
rpm -ivh package.rpm                  # Install with verbose + progress bar

# Query / Inspect
rpm -qa                               # List ALL installed packages
rpm -qa --last                        # List recently installed packages
rpm -qi packagename                   # Info about installed package
rpm -ql packagename                   # List files installed by package
rpm -qpR packagename.rpm              # Show dependencies (before install)
rpm -Pqi packagename.rpm              # Info about package file (pre-install)
rpm -qdf /usr/bin/vmstat              # Show documentation for a command

# Remove
rpm -evh packagename                  # Remove package and dependencies
rpm -ev --nodeps packagename          # Remove package only (keep deps)
```

```bash
lsblk                                 # List all storage block devices
```

---

## 11. Package Management – YUM

### What is YUM?

**YUM (Yellowdog Updater Modified)** is a high-level package manager for RPM systems. It **automatically handles dependencies** — install one package, YUM fetches everything it needs.

> `yum` and `dnf` are interchangeable — `dnf` is the modern successor with the same syntax.

### YUM Commands

```bash
# Install & Remove
yum install packagename               # Install a package (with deps)
yum remove packagename                # Remove a package

# Update
yum update packagename                # Update specific package
yum update                            # Update all packages
yum check-update                      # Check what updates are available

# Search & Info
yum search packagename                # Search for a package by name
yum info packagename                  # Show package details and description
yum list                              # List all available packages
yum list installed                    # List only installed packages

# Repository Management
yum repolist                          # Show enabled repositories only
yum repolist all                      # Show all repos (enabled + disabled)
yum-config-manager --enable reponame  # Enable a disabled repo
yum-config-manager --disable reponame # Disable an enabled repo

# Maintenance
yum clean all                         # Clear all cached metadata and packages
yum history                           # Show history of all yum transactions
```

---

## 12. YUM Repository Setup

### Setting Up a Local Repository from ISO

```bash
# Step 1: Install required packages
yum install vsftpd          # FTP server to serve packages over network
yum install createrepo*     # Tool to generate repository metadata

# Step 2: Create directory structure
mkdir -p /var/ftp/pub/AppStream
mkdir -p /var/ftp/pub/BaseOS

# Step 3: Copy packages from mounted ISO
cp -r /run/media/root/CentOS-Stream-9/AppStream/Packages/ /var/ftp/pub/AppStream/
cp -r /run/media/root/CentOS-Stream-9/BaseOS/Packages/    /var/ftp/pub/BaseOS/

# Step 4: Create repository metadata
createrepo -v /var/ftp/pub/AppStream/Packages
createrepo -v /var/ftp/pub/BaseOS/Packages

# Step 5: Create repo config file
vi /etc/yum.repos.d/local.repo
```

```ini
# /etc/yum.repos.d/local.repo
[AppStream]
name=Local AppStream
baseurl=ftp://localhost/pub/AppStream/Packages
enabled=1
gpgcheck=0

[BaseOS]
name=Local BaseOS
baseurl=ftp://localhost/pub/BaseOS/Packages
enabled=1
gpgcheck=0
```

```bash
# Step 6: Disable firewall and SELinux (lab environment only)
systemctl disable --now firewalld
vi /etc/selinux/config      # Change: SELINUX=enforcing → SELINUX=disabled

# Step 7: Configure and start FTP
vi /etc/vsftpd/vsftpd.conf
systemctl enable --now vsftpd

# Step 8: Client-side setup
cd /etc/yum.repos.d
vi local.repo               # Add same repo config
yum clean all               # Clear old cache
yum install packagename     # Install from local repo ✓
```

---

## 13. SSH – Secure Shell

### What is SSH?

**SSH (Secure Shell)** lets you **remotely access and manage any server** using its IP address over an encrypted, authenticated connection.

### SSH Service & Configuration

```bash
# Install SSH packages
yum install openssh*              # Installs openssh, openssh-server, openssh-clients

# Service management
systemctl start sshd
systemctl enable sshd

# Configuration files
/etc/ssh/sshd_config              # Server-side configuration
/etc/ssh/ssh_config               # Client-side configuration
# Default Port: 22

# Change hostname
hostnamectl set-hostname newhostname
bash                              # Reload shell to apply new hostname
```

### Connect to Remote Server

```bash
# Basic SSH connection
ssh username@IP_address

# SSH with key file (AWS / cloud servers)
ssh -i keyfile.pem ec2-user@IP_address

# Change SSH port (in /etc/ssh/sshd_config)
Port 2222                         # Example: change from 22 to 2222
```

### File Transfer with SCP

```bash
# Push: Send files TO remote server
scp filename user@IP:/remote/path/            # Single file
scp -r directory/ user@IP:/remote/path/       # Entire directory

# Pull: Fetch files FROM remote server
scp user@IP:/remote/file /local/path/         # Single file
scp -r user@IP:/remote/dir/ /local/path/      # Entire directory
```

### Passwordless Login (Key-Based Authentication)

```bash
# Step 1: Generate SSH key pair (on your local machine)
ssh-keygen                        # Creates id_rsa (private) + id_rsa.pub (public)
cd ~/.ssh
ls                                # Verify keys exist

# Step 2: Copy public key to target server
ssh-copy-id username@server_IP    # Adds key to server's authorized_keys

# Step 3: Login without password
ssh username@server_IP            # No password prompt ✓
```

> **How it works:** Your **public key** goes to the server (`~/.ssh/authorized_keys`). When you connect, SSH verifies your **private key** matches — authentication without a password.

---

## 14. Swap Memory

### What is Swap?

Swap is **disk space used as overflow memory** when physical RAM is full. The OS moves inactive memory pages to swap, keeping active processes running smoothly.

```
Active RAM (fast)  +  Swap on Disk (slower)  =  Total usable memory
```

### Create and Enable Swap

```bash
# Step 1: Create a swap file (1 GB example)
fallocate -l 1G /swapfile

# Step 2: Secure the file (owner-only access)
chmod 600 /swapfile

# Step 3: Format as swap space
mkswap /swapfile

# Step 4: Enable swap
swapon /swapfile

# Step 5: Make permanent (auto-mount on reboot)
vi /etc/fstab
# Add this line at the bottom:
# /swapfile   swap   swap   defaults   0   0

# Verify swap is active
free -h                           # Human-readable RAM + swap usage
free -m                           # Show in MB
```

### Manage Swap

```bash
swapon /swapfile                  # Enable swap file
swapoff /swapfile                 # Disable swap file

# Check swappiness value
cat /proc/sys/vm/swappiness
```

### Swappiness Reference

| Value | Behavior |
|-------|----------|
| `0` | Avoid swap — use RAM as long as possible |
| `30` | Default on CentOS 7 |
| `60` | Default on CentOS 9 — balanced |
| `100` | Use swap aggressively |

---

## 15. Linux Networking

### IPv4 Address Classes

| Class | Range | Subnet Mask | Total Hosts |
|-------|-------|-------------|-------------|
| **A** | 1 – 126.x.x.x | /8 | 16,777,216 |
| **B** | 128 – 191.x.x.x | /16 | 65,536 |
| **C** | 192 – 223.x.x.x | /24 | 256 |

> `127.x.x.x` → **Loopback** (localhost — points to itself)

### Key Networking Terms

| Term | Definition |
|------|-----------|
| **Subnet** | Dividing a network into smaller segments |
| **Subnet Mask** | Defines which bits belong to network vs host |
| **Gateway** | IP address of the router (network exit point) |
| **Static IP** | Manually configured, permanent IP address |
| **Dynamic IP** | Auto-assigned by DHCP server |

### View Network Information

```bash
ip a                              # Show all interfaces and IP addresses
ifconfig                          # Show network info (older method)
nmcli device show                 # Detailed device information
nmcli device status               # Status of all network devices
nmcli connection show             # List all network connections
```

### Set Dynamic IP (DHCP)

```bash
# After adding network adapter in VM settings:
nmcli device connect ens36        # Connect device and get DHCP IP
ip a                              # Verify new IP address assigned

# Remove adapter:
nmcli device disconnect ens36
nmcli connection delete ens36
```

### Set Static IP (Manual)

```bash
# Method 1: Create a new connection
nmcli con add type ethernet con-name ens36 ifname ens36 \
  ipv4.addresses 192.168.159.50/24 \
  ipv4.gateway 192.168.0.1 \
  ipv4.method manual \
  autoconnect yes

# Method 2: Modify an existing connection
nmcli con modify ens160 ipv4.addresses 192.168.2.100/24
nmcli con mod ens160 ipv4.gateway 192.168.2.1
nmcli con mod ens160 ipv4.dns 8.8.8.8
nmcli con mod ens160 ipv4.method manual
nmcli con mod ens160 connection.autoconnect yes

# Apply changes
nmcli con up ens160

# Verify
ip a
```

### Network Diagnostics

```bash
netstat -rn                       # Show routing table
route -n                          # Show routing table (alternative)
netstat -pultan                   # Show all open ports and listening services
ip route show                     # Show routing info
nmtui                             # Graphical (TUI) network configurator
```

---

## 🗂️ Repository Structure

```
📦 linux-notes/
├── linux_os.docx                        # Linux OS & file system basics
├── vi_editor.docx                       # Vim editor commands
├── user_management.docx                 # User & group management
├── file_permission.docx                 # File permissions & ownership
├── special_permissions.docx             # SUID, SGID, Sticky Bit, ACL
├── linux_links.docx                     # Hard links & soft links
├── file_compression.docx                # zip, gzip, tar
├── process_management.docx              # Process control & priorities
├── Service_and_Daemons.docx             # systemctl & services
├── package_management_rpm.docx          # RPM package manager
├── package_management_yum.docx          # YUM / DNF package manager
├── package_management_yum_repo.docx     # Local YUM repository setup
├── ssh__secure_shell_.docx              # SSH & secure file transfer
├── swap_memory.docx                     # Swap memory management
├── Networking.docx                      # Linux networking & nmcli
└── README.md                            # This file
```

---

## ⚡ Quick Command Reference

```bash
# ── Users ────────────────────────────────────────────────────
useradd / userdel -r / usermod / passwd / chage

# ── Permissions ──────────────────────────────────────────────
chmod / chown / chgrp / umask / setfacl / getfacl

# ── Processes ────────────────────────────────────────────────
top / ps aux / kill -9 / nice / renice / jobs / fg / bg

# ── Services ─────────────────────────────────────────────────
systemctl start|stop|restart|enable|disable|status <service>

# ── Packages ─────────────────────────────────────────────────
rpm -ivh / rpm -evh / rpm -qa
yum install / yum remove / yum update / yum list

# ── Files & Directories ──────────────────────────────────────
ls -lh / cat / head / tail / cp -r / mv / rm -r / mkdir -p

# ── Compression ──────────────────────────────────────────────
zip -r / unzip / gzip -k / gzip -d / tar -czvf / tar -xzvf

# ── Networking ───────────────────────────────────────────────
ip a / nmcli / netstat -rn / netstat -pultan / nmtui

# ── SSH ──────────────────────────────────────────────────────
ssh / scp / ssh-keygen / ssh-copy-id
```

---

<div align="center">

> 📝 These notes were compiled from hands-on Linux administration practice.
> Each section covers theory, commands, and real-world usage examples.

</div>
