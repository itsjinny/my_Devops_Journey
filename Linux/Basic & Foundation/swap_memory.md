# 📦 Storage & Memory Management – DevOps & DevSecOps Guide

## Table of content

- [📦 Storage \& Memory Management – DevOps \& DevSecOps Guide](#-storage--memory-management--devops--devsecops-guide)
  - [Table of content](#table-of-content)
  - [📂 1. Filesystem \& Directory Hierarchy](#-1-filesystem--directory-hierarchy)
  - [🔗 2. Mounting \& Unmounting Storage](#-2-mounting--unmounting-storage)
    - [📌 Mount Commands](#-mount-commands)
    - [📌 Persistent Mounts (fstab)](#-persistent-mounts-fstab)
    - [📌 Common Tools](#-common-tools)
  - [💡 3. Swap Space in Linux](#-3-swap-space-in-linux)
    - [✅ What is Swap?](#-what-is-swap)
    - [📌 Check Swap Usage](#-check-swap-usage)
    - [📌 Create Swap File](#-create-swap-file)
    - [📌 Tune Swappiness](#-tune-swappiness)

## 📂 1. Filesystem & Directory Hierarchy
- Understand **FHS (Filesystem Hierarchy Standard)**:
  - `/bin`, `/boot`, `/etc`, `/home`, `/var`, `/usr`, `/tmp`, `/mnt`, `/media`, etc.
- Important system config and log paths:
  - `/etc/fstab`, `/etc/mtab`, `/etc/hostname`, `/etc/hosts`
  - `/var/log`, `/var/tmp`

---

## 🔗 2. Mounting & Unmounting Storage

### 📌 Mount Commands
```bash
mount /dev/sdb1 /mnt/data         # Temporarily mount
umount /mnt/data                  # Unmount
```

### 📌 Persistent Mounts (fstab)
```bash
/dev/sdb1 /mnt/data ext4 defaults 0 2
```

### 📌 Common Tools
```bash
lsblk, blkid, df -h, du -sh, mount, umount, findmnt
```
---

## 💡 3. Swap Space in Linux
### ✅ What is Swap?
> Disk-based memory used when RAM is full.

### 📌 Check Swap Usage
```bash
free -h
swapon --show
cat /proc/swaps
```

### 📌 Create Swap File
```bash
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

### 📌 Tune Swappiness
```bash
cat /proc/sys/vm/swappiness
sudo sysctl vm.swappiness=10
```