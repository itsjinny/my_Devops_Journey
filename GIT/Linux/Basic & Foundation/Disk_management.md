# 🧠 Linux Disk Management Cheatsheet (DevOps/DevSecOps Ready)

> Covers everything from device files to partitioning, formatting, mounting, and filesystem checks.

---

## 📁 Table of Contents

- [🧠 Linux Disk Management Cheatsheet (DevOps/DevSecOps Ready)](#-linux-disk-management-cheatsheet-devopsdevsecops-ready)
  - [📁 Table of Contents](#-table-of-contents)
  - [/proc, /sys \& Virtual Filesystems](#proc-sys--virtual-filesystems)
  - [File Types](#file-types)
  - [Symbolic vs Hard Links](#symbolic-vs-hard-links)
  - [Understanding Block Devices: /dev/sdX, /dev/nvmeX, /dev/mapper/](#understanding-block-devices-devsdx-devnvmex-devmapper)
  - [Mounting Manually Using `mount`](#mounting-manually-using-mount)
  - [Disk Tools: `mount`, `umount`, `df`, `du`, `lsblk`, `blkid`, `findmnt`](#disk-tools-mount-umount-df-du-lsblk-blkid-findmnt)
  - [Mount Options: `noexec`, `nosuid`, `ro`, `rw`, `bind`, `loop`, `defaults`](#mount-options-noexec-nosuid-ro-rw-bind-loop-defaults)
  - [Mounting ISO, Loop Devices, Network Shares (NFS, CIFS)](#mounting-iso-loop-devices-network-shares-nfs-cifs)
  - [Tools: `fdisk`, `gdisk`, `parted`](#tools-fdisk-gdisk-parted)
  - [Create/Resize/Delete Partitions](#createresizedelete-partitions)
  - [Filesystem Creation: `mkfs.ext4`, `mkfs.xfs`, `mkfs.btrfs`, `mkfs.vfat`](#filesystem-creation-mkfsext4-mkfsxfs-mkfsbtrfs-mkfsvfat)
  - [Filesystem Check \& Repair: `fsck`, `tune2fs`, `e2fsck`, `xfs_repair`](#filesystem-check--repair-fsck-tune2fs-e2fsck-xfs_repair)
- [🧱 Linux LVM (Logical Volume Manager) - DevOps Notes](#-linux-lvm-logical-volume-manager---devops-notes)
  - [🔁 Core Concepts](#-core-concepts)
    - [1. 🧱 PV – Physical Volume](#1--pv--physical-volume)
    - [2. 📦 VG – Volume Group](#2--vg--volume-group)
    - [3. 📄 LV – Logical Volume](#3--lv--logical-volume)
    - [🧰 Creating LVM Setup (Step-by-Step)](#-creating-lvm-setup-step-by-step)
  - [📏 Resizing LVs \& Filesystems](#-resizing-lvs--filesystems)
    - [🔼 Extend LV + Filesystem](#-extend-lv--filesystem)
    - [🔽 Reduce LV (Caution!)](#-reduce-lv-caution)
    - [📸 Snapshotting with LVM](#-snapshotting-with-lvm)
    - [🧪 Useful LVM Commands](#-useful-lvm-commands)
- [RAID (Redundant Array of independent Disks)](#raid-redundant-array-of-independent-disks)
  - [🔸 What is RAID?](#-what-is-raid)
  - [🔹 RAID Levels (0, 1, 5, 6, 10)](#-raid-levels-0-1-5-6-10)
  - [🔧 Creating Software RAID with `mdadm`](#-creating-software-raid-with-mdadm)
    - [Step-by-Step (RAID 1 Example)](#step-by-step-raid-1-example)
  - [🧪 Monitor RAID Arrays](#-monitor-raid-arrays)
  - [🔄 Backup Strategy with RAID](#-backup-strategy-with-raid)
    - [Combine RAID + Backup](#combine-raid--backup)
- [🧩 Automating \& Monitoring Mounts in Linux](#-automating--monitoring-mounts-in-linux)
  - [🔹 1. Auto-mounting with systemd](#-1-auto-mounting-with-systemd)
    - [🧠 Concept](#-concept)
    - [🔍 Why use it?](#-why-use-it)
    - [⚙️ How it works](#️-how-it-works)
    - [✅ Example: Auto-mount `/dev/sdb1` at `/data`](#-example-auto-mount-devsdb1-at-data)
  - [2. Auto-mounting with autofs(on-demand mounting)](#2-auto-mounting-with-autofson-demand-mounting)
    - [🧠 Concept](#-concept-1)
    - [🔍 Why use it?](#-why-use-it-1)
    - [⚙️ How it works](#️-how-it-works-1)
    - [✅ Example: NFS auto-mount on access](#-example-nfs-auto-mount-on-access)
  - [🔹 3. Monitoring Mounted Devices](#-3-monitoring-mounted-devices)
    - [📊 dstat — Real-time I/O stats](#-dstat--real-time-io-stats)
    - [📊 iostat — Disk I/O utilization](#-iostat--disk-io-utilization)
    - [📊 iotop — Process-level I/O monitoring](#-iotop--process-level-io-monitoring)
  - [🔹 4. Disk Health Alerts \& Smart Monitoring](#-4-disk-health-alerts--smart-monitoring)
    - [🧠 Concept](#-concept-2)
    - [✅ smartctl Disk Health Check](#-smartctl-disk-health-check)
    - [🕒 Cron Job for Daily Health Check](#-cron-job-for-daily-health-check)
    - [🔁 Auto Alert on Device Plug-in (udev)](#-auto-alert-on-device-plug-in-udev)
  - [✅ Summary: Which Tool to Use When](#-summary-which-tool-to-use-when)
  - [Auto-trigger actions on plug	|udev	|Automate scripts based on device events](#auto-trigger-actions-on-plugudevautomate-scripts-based-on-device-events)
- [🌐 Network Storage in Linux](#-network-storage-in-linux)
  - [🔹 1. NFS (Network File System)](#-1-nfs-network-file-system)
    - [🧠 What is NFS?](#-what-is-nfs)
    - [🧩 Setup NFS Server on Ubuntu](#-setup-nfs-server-on-ubuntu)
    - [📂 Configure Shared Directory](#-configure-shared-directory)
    - [🛠 Edit /etc/exports](#-edit-etcexports)
    - [🔗 Mount NFS Share (Client)](#-mount-nfs-share-client)
    - [📌 Permanent mount in /etc/fstab](#-permanent-mount-in-etcfstab)
  - [🔹 2. CIFS/SMB (Windows Shares)](#-2-cifssmb-windows-shares)
    - [🧠 What is CIFS?](#-what-is-cifs)
    - [🔗 Mount CIFS Share](#-mount-cifs-share)
    - [🔐 Secure Credentials File](#-secure-credentials-file)
    - [📌 Use in /etc/fstab](#-use-in-etcfstab)
    - [❌ Troubleshooting NFS/CIFS](#-troubleshooting-nfscifs)
  - [🔹 3. Cloud Block Storage Mounting](#-3-cloud-block-storage-mounting)
    - [📌 Summary Table](#-summary-table)
  - [Cloud disk mount	|AWS/GCP/Azure + mkfs, mount, lsblk](#cloud-disk-mountawsgcpazure--mkfs-mount-lsblk)
- [🔐 Filesystem Security \& Auditing in Linux](#-filesystem-security--auditing-in-linux)
  - [🔹 1. Permissions: `chmod`, `chown`, `umask`, Sticky Bit](#-1-permissions-chmod-chown-umask-sticky-bit)
    - [🧠 Why?](#-why)
    - [🛠 chmod (Change Mode)](#-chmod-change-mode)
  - [🛠 chown (Change Ownership)](#-chown-change-ownership)
  - [🧬 umask (Default Permission Mask)](#-umask-default-permission-mask)
    - [📌 Special Bits](#-special-bits)
  - [🔹 2. Read-only \& nosuid Mount Options](#-2-read-only--nosuid-mount-options)
    - [🧠 Why?](#-why-1)
    - [📌 Example /etc/fstab:](#-example-etcfstab)
  - [🔹 3. Mount Auditing Tools](#-3-mount-auditing-tools)
    - [🧠 Why?](#-why-2)
    - [📋 auditd](#-auditd)
    - [📋 inotify (Real-time file change monitor)](#-inotify-real-time-file-change-monitor)
  - [🔹 4. Backup \& Restore](#-4-backup--restore)
    - [🧠 Why?](#-why-3)
    - [🛠 rsync (Incremental, fast)](#-rsync-incremental-fast)
    - [🛠 tar (Archiving)](#-tar-archiving)
    - [🛠 dump \& restore (For ext2/ext3/ext4 block-level backups)](#-dump--restore-for-ext2ext3ext4-block-level-backups)
    - [🧠 Summary](#-summary)

---

## /proc, /sys & Virtual Filesystems

| Virtual FS |  Description |
|------------|--------------------------|
| `/proc`    | Kernel info like processes, memory, cmdline, etc. |
| `/sys`     | Exposes kernel objects and hardware config |
| `/dev`     | Contains dynamically created device files |

---

## File Types

| Type         | Symbol | Description |
|--------------|--------|-------------|
| Regular File | `-`    | Normal file |
| Directory    | `d`    | Folder      |
| Symlink      | `l`    | Shortcut    |
| Block Device | `b`    | Disk        |
| Char Device  | `c`    | Keyboard, Serial Port |
| Pipe (FIFO)  | `p`    | For inter-process communication |
| Socket       | `s`    | Network/socket communication |

---

## Symbolic vs Hard Links

| Feature      | Symbolic Link         | Hard Link                |
|--------------|-----------------------|--------------------------|
| Type         | Pointer to path       | Duplicate inode pointer  |
| Cross-device | ✅ Yes                | ❌ No                    |
| If target deleted | Broken link       | File still exists        |

---

## Understanding Block Devices: /dev/sdX, /dev/nvmeX, /dev/mapper/

| Type      | Example        | Description                      |
|-----------|----------------|----------------------------------|
| SATA Disk | `/dev/sda1`    | Standard disk partitions         |
| NVMe Disk | `/dev/nvme0n1p1` | High-performance SSDs          |
| LVM       | `/dev/mapper/vg-lv` | Logical volume mapping    |

---

## Mounting Manually Using `mount`

```bash
sudo mount /dev/sdb1 /mnt/mydisk
sudo umount /mnt/mydisk
```

## Disk Tools: `mount`, `umount`, `df`, `du`, `lsblk`, `blkid`, `findmnt`
|Command|	Purpose|
|-------|----------|
|mount	|Mount device|
|umount	|Unmount device|
|df -h	|Show disk usage|
|du -sh	|Show directory usage|
|lsblk	|List block devices|
|blkid	|Show UUID and FS type|
|findmnt|	Find mount points|

## Mount Options: `noexec`, `nosuid`, `ro`, `rw`, `bind`, `loop`, `defaults`
|Option	|Description|
|------|----------|
|ro/rw	|Read-only / Read-write
|noexec	|Prevent running binaries
|nosuid	|Disable SUID binaries
|bind	|Mount same FS elsewhere
|loop	|Mount ISO files
|defaults	|Standard options

## Mounting ISO, Loop Devices, Network Shares (NFS, CIFS)
```bash
# ISO Mount
sudo mount -o loop file.iso /mnt

# NFS Mount
sudo mount -t nfs server:/share /mnt

# CIFS (Windows share)
sudo mount -t cifs -o username=user,password=pass //server/share /mnt
```

## Tools: `fdisk`, `gdisk`, `parted`
Tool|	Use Case
|----|----------|
fdisk	|MBR partitioning
gdisk	|GPT partitioning
parted	|Both GPT + MBR, resizable

## Create/Resize/Delete Partitions
```bash
# Create a new partition
sudo fdisk /dev/sdb

# Resize a partition (GPT)
sudo parted /dev/sdb resizepart 1 100%

# Delete a partition
sudo fdisk /dev/sdb   # d to delete
```

## Filesystem Creation: `mkfs.ext4`, `mkfs.xfs`, `mkfs.btrfs`, `mkfs.vfat`
```bash
sudo mkfs.ext4 /dev/sdb1
sudo mkfs.xfs /dev/sdb1
sudo mkfs.btrfs /dev/sdb1
sudo mkfs.vfat /dev/sdb1
```

FS Type|	Command|	Use Case
|------|-----------|------------|
ext4	|mkfs.ext4	|General-purpose default Linux
xfs	|mkfs.xfs	|High performance
btrfs	|mkfs.btrfs	|Snapshots, RAID, compression
vfat	|mkfs.vfat	|USB drives, boot, Windows comp

## Filesystem Check & Repair: `fsck`, `tune2fs`, `e2fsck`, `xfs_repair`
```bash
# Basic fsck (auto-detect)
sudo fsck /dev/sdb1

# For ext4
sudo e2fsck -f /dev/sdb1

# View FS info
sudo tune2fs -l /dev/sdb1

# For XFS
sudo umount /dev/sdb1
sudo xfs_repair /dev/sdb1
```

Tool|	FS Type	|Purpose
|-------|---------|------|
fsck	|All	|Generic checker
e2fsck	|ext2/3/4	|Detailed ext checker
tune2fs	|ext	|Tune FS params
xfs_repair|	xfs	|XFS-only repair tool

# 🧱 Linux LVM (Logical Volume Manager) - DevOps Notes

> Manage disk storage flexibly using LVM in Linux. This is useful in DevOps, servers, VMs, and dynamic storage needs.

---

## 🔁 Core Concepts

### 1. 🧱 PV – Physical Volume
- Base layer for LVM
- Created from raw disk or partition
```bash
pvcreate /dev/sdb1
```

### 2. 📦 VG – Volume Group
- Pool of one or more PVs
- Acts as storage container
```bash
vgcreate my_vg /dev/sdb1
```

### 3. 📄 LV – Logical Volume
- Acts like a partition inside VG
- Can format, mount, resize, snapshot
```bash
lvcreate -L 10G -n my_lv my_vg
mkfs.ext4 /dev/my_vg/my_lv
mount /dev/my_vg/my_lv /mnt/data
```

### 🧰 Creating LVM Setup (Step-by-Step)
```bash
# 1. Create Partition (if needed)
fdisk /dev/sdb       # make a new partition type '8e' (Linux LVM)

# 2. Create PV
pvcreate /dev/sdb1

# 3. Create VG
vgcreate my_vg /dev/sdb1

# 4. Create LV
lvcreate -L 5G -n my_lv my_vg

# 5. Format LV
mkfs.ext4 /dev/my_vg/my_lv

# 6. Mount it
mkdir /mnt/mydata
mount /dev/my_vg/my_lv /mnt/mydata
```

## 📏 Resizing LVs & Filesystems
### 🔼 Extend LV + Filesystem
```bash
lvextend -L +2G /dev/my_vg/my_lv
resize2fs /dev/my_vg/my_lv         # for ext4
xfs_growfs /mnt/mydata             # for XFS (must be mounted)
```

### 🔽 Reduce LV (Caution!)
```bash
umount /mnt/mydata
e2fsck -f /dev/my_vg/my_lv
resize2fs /dev/my_vg/my_lv 2G
lvreduce -L 2G /dev/my_vg/my_lv
```

### 📸 Snapshotting with LVM
- Great for backups, testing, rollback.
```bash
# Create Snapshot
lvcreate -L 1G -s -n my_snap /dev/my_vg/my_lv

# Mount it (optional)
mount /dev/my_vg/my_snap /mnt/snapshot

# Delete Snapshot
lvremove /dev/my_vg/my_snap
```

### 🧪 Useful LVM Commands
|Task	|Command
|-------|----------|
Create PV	|pvcreate /dev/sdb1
View PVs	|pvs, pvdisplay
Create VG	|vgcreate my_vg /dev/sdb1
View VGs	|vgs, vgdisplay
Create LV	|lvcreate -L 5G -n my_lv my_vg
View LVs	|lvs, lvdisplay
Extend LV	|lvextend -L +2G /dev/my_vg/my_lv
Reduce LV	|lvreduce -L 2G /dev/my_vg/my_lv
Remove LV	|lvremove /dev/my_vg/my_lv
Remove VG	|vgremove my_vg
Remove PV	|pvremove /dev/sdb1

# RAID (Redundant Array of independent Disks)
## 🔸 What is RAID?

RAID combines multiple disks into a single logical unit for:
- **Performance** (striping)
- **Redundancy** (mirroring, parity)
- **Fault tolerance**

---

## 🔹 RAID Levels (0, 1, 5, 6, 10)

| RAID | Disks | Redundancy | Speed | Fault Tolerance | Use Case |
|------|-------|-------------|--------|------------------|-----------|
| 0    | ≥2    | ❌ None     | ✅ High (striping) | ❌ None | Speed only (e.g. cache) |
| 1    | 2     | ✅ Mirror   | 🔁 Same as 1 disk | ✅ 1 disk | Critical data, logs |
| 5    | ≥3    | ✅ Parity   | ✅ Good (striping + parity) | ✅ 1 disk | Balanced, web servers |
| 6    | ≥4    | ✅ Dual parity | Moderate | ✅✅ 2 disks | Large arrays, backup servers |
| 10   | ≥4    | ✅ Mirror + Stripe | ✅✅ High | ✅ 1 per mirror pair | Databases, production apps |

---

## 🔧 Creating Software RAID with `mdadm`
### Step-by-Step (RAID 1 Example)

```bash
# 1. Install mdadm (if not installed)
sudo apt install mdadm  # Debian/Ubuntu
sudo yum install mdadm  # RHEL/CentOS

# 2. Prepare disks
fdisk /dev/sdb  # create /dev/sdb1
fdisk /dev/sdc  # create /dev/sdc1

# 3. Create RAID array (e.g. RAID 1 - mirror)
sudo mdadm --create --verbose /dev/md0 --level=1 --raid-devices=2 /dev/sdb1 /dev/sdc1

# 4. Monitor RAID build progress
cat /proc/mdstat

# 5. Create filesystem
mkfs.ext4 /dev/md0

# 6. Mount it
mkdir /mnt/raid
mount /dev/md0 /mnt/raid

# 7. Save RAID configuration (important!)
mdadm --detail --scan >> /etc/mdadm/mdadm.conf  # Debian
mdadm --detail --scan >> /etc/mdadm.conf        # RHEL

# 8. Update initramfs (Debian)
update-initramfs -u

```

## 🧪 Monitor RAID Arrays
```bash
# Check RAID status
cat /proc/mdstat

# Detailed info
mdadm --detail /dev/md0

# Simulate disk failure
mdadm /dev/md0 --fail /dev/sdb1

# Remove failed disk
mdadm /dev/md0 --remove /dev/sdb1

# Add new disk
mdadm /dev/md0 --add /dev/sdd1
```

## 🔄 Backup Strategy with RAID
> RAID is NOT a backup. It’s high availability.

### Combine RAID + Backup
Goal|	Tool
|-----|------|
Daily backup	|rsync, rsnapshot, Bacula, Restic
Remote/offsite backup	|rclone, borg, cloud-based (S3, GCS)
Snapshot + Backup	|LVM snapshots + backup
Full-disk image	|Clonezilla, dd

---

# 🧩 Automating & Monitoring Mounts in Linux

>This document explains how to automate and monitor disk mounts using tools like `systemd`, `autofs`, `smartctl`, `iostat`, and more — with **deep-dive explanations**, **use cases**, and **practical examples**.

---

## 🔹 1. Auto-mounting with systemd

### 🧠 Concept
`systemd` can mount file systems at boot or as services.

### 🔍 Why use it?
- Auto-mount external drives, NFS shares, encrypted volumes at boot.
- Avoid manual mounting in server environments.

### ⚙️ How it works
Systemd uses `.mount` unit files.

### ✅ Example: Auto-mount `/dev/sdb1` at `/data`
```ini
# /etc/systemd/system/data.mount
[Unit]
Description=Mount Data Disk
After=network.target

[Mount]
What=/dev/sdb1
Where=/data
Type=ext4
Options=defaults

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl daemon-reexec
sudo systemctl enable data.mount
sudo systemctl start data.mount
```

## 2. Auto-mounting with autofs(on-demand mounting)
### 🧠 Concept
>autofs mounts devices only when accessed and unmounts after inactivity.
### 🔍 Why use it?
- Dynamic mounting for NFS, USB, remote volumes.
- Reduces idle mounts, useful for development or cloud VMs.

### ⚙️ How it works
- /etc/auto.master defines mount base.
- /etc/auto.* defines specific mount rules.

### ✅ Example: NFS auto-mount on access
```bash
# /etc/auto.master
/mnt/nfs /etc/auto.nfs --timeout=60

# /etc/auto.nfs
docs  -fstype=nfs,rw  server:/export/docs

sudo systemctl restart autofs
```

## 🔹 3. Monitoring Mounted Devices
### 📊 dstat — Real-time I/O stats
```bash
dstat -cdngy # -c: CPU stats, -d: disk stats, -n: network stat, -g: page states, -y: system stats
```

### 📊 iostat — Disk I/O utilization
```bash
iostat -xz 2

iostat %util #cpu and disk stat percent wise

iostat await # I/O wait time

```

### 📊 iotop — Process-level I/O monitoring
```bash
sudo iotop
```
- Identify processes causing high disk I/O.

## 🔹 4. Disk Health Alerts & Smart Monitoring
### 🧠 Concept
- Use smartctl for SMART-based disk health.
- Use cron/at for scheduled checks.
- Use udev to auto-detect new device plug-ins.

### ✅ smartctl Disk Health Check
```bash
sudo smartctl -a /dev/sda
```
### 🕒 Cron Job for Daily Health Check
```bash
0 3 * * * /usr/sbin/smartctl -a /dev/sda >> /var/log/smart.log
```

### 🔁 Auto Alert on Device Plug-in (udev)
- udev rule
```bash
# /etc/udev/rules.d/99-disk.rules
KERNEL=="sd[b-z]", ACTION=="add", RUN+="/usr/local/bin/notify-disk.sh"
```
- notify-disk.sh
```bash
#!/bin/bash
echo "Disk added: $DEVNAME" | mail -s "Disk Alert" admin@example.com
```

## ✅ Summary: Which Tool to Use When
Purpose|	Tool|	Why
|------|------|----
Boot-time static mounts	|systemd	|Reliable auto-mounting at boot
Dynamic on-demand mounts	|autofs	|Mount only when accessed
Realtime performance monitor	|dstat, iotop, iostat	|Detect slow or overused mounts
Disk health alerting	|smartctl + cron	|Prevent failure with early warning
Auto-trigger actions on plug	|udev	|Automate scripts based on device events
---

# 🌐 Network Storage in Linux

This document covers NFS, CIFS/SMB, and cloud block storage mounting (AWS, Azure, GCP), with real-world setup, commands, and troubleshooting.

---

## 🔹 1. NFS (Network File System)

### 🧠 What is NFS?
A protocol that allows file access over the network like a local drive.

---

### 🧩 Setup NFS Server on Ubuntu
```bash
sudo apt update
sudo apt install nfs-kernel-server
```

### 📂 Configure Shared Directory
```bash
sudo mkdir -p /srv/nfs/shared
sudo chown nobody:nogroup /srv/nfs/shared
```

### 🛠 Edit /etc/exports
```bash
/srv/nfs/shared  192.168.1.0/24(rw,sync,no_subtree_check)

# Restart NFS service

sudo exportfs -a
sudo systemctl restart nfs-kernel-server
```

### 🔗 Mount NFS Share (Client)
```bash
sudo apt install nfs-common
sudo mount -t nfs 192.168.1.10:/srv/nfs/shared /mnt
```

### 📌 Permanent mount in /etc/fstab
```bash
192.168.1.10:/srv/nfs/shared  /mnt  nfs  defaults  0  0
```

## 🔹 2. CIFS/SMB (Windows Shares)
### 🧠 What is CIFS?
> Common Internet File System used by Windows for sharing folders over a LAN.

### 🔗 Mount CIFS Share
```bash
sudo apt install cifs-utils
sudo mount -t cifs //192.168.1.20/shared /mnt -o username=user,password=pass
```

### 🔐 Secure Credentials File
```bash
# /etc/smb-credentials
username=myuser
password=mypass

chmod 600 /etc/smb-credentials
```

### 📌 Use in /etc/fstab
```bash
//192.168.1.20/shared  /mnt  cifs  credentials=/etc/smb-credentials,iocharset=utf8,uid=1000,gid=1000  0  0
```

### ❌ Troubleshooting NFS/CIFS
Problem|	Fix
|------|----------|
"Permission denied"	|Check exports file or CIFS credentials
"Stale file handle"	|Restart NFS service or unmount + remount
Timeout / mount hang	|Use soft option or check firewall (NFS port: 2049)
No such file/device	|Ensure nfs-common or cifs-utils is installed
Files not writable	|Check permissions on shared directory + mount UID/GID

## 🔹 3. Cloud Block Storage Mounting
> AWS, Azure, GCP etc (same process for all)

- Attach EBS to EC2 instance
- Identify with lsblk or dmesg | grep sd

- format disk
```bash
sudo mkfs.ext4 /dev/sdd
sudo mkdir /data #mount point
sudo mount /dev/sdd /data #mount the storage
```

- use fstab for persistent mounting
```bash
#/etc/fstab
/dev/xvdf /data ext4 defaults,nofail 0 0
```

 ### 📌 Summary Table
Task|Tool/Command
|---|------|
NFS server setup	|nfs-kernel-server, /etc/exports, exportfs
NFS mount	|mount -t nfs, nfs-common
SMB mount	|mount -t cifs, cifs-utils
Secure credentials	|/etc/smb-credentials
Persistent mounts	|/etc/fstab
Cloud disk mount	|AWS/GCP/Azure + mkfs, mount, lsblk
---

# 🔐 Filesystem Security & Auditing in Linux

> This document explains Linux file/directory permissions, secure mount options, auditing changes, and backup/restore strategies. All content is structured for hands-on + DevSecOps use.

---

## 🔹 1. Permissions: `chmod`, `chown`, `umask`, Sticky Bit

### 🧠 Why?
To control who can read/write/execute files and directories on multi-user systems.

---

### 🛠 chmod (Change Mode)
```bash
# Octal (e.g., 754 => rwxr-xr--)
chmod 754 file.sh

# Symbolic
chmod u+x file.sh
```

## 🛠 chown (Change Ownership)
```bash
# Change owner
chown user file

# Change owner & group
chown user:group file
```

## 🧬 umask (Default Permission Mask)
- Default mask for new files/directories
```bash
# View
umask
# Set temporarily
umask 027
```
- 027 → Default file perms = 666 - 027 = 640 (rw-r-----)

### 📌 Special Bits
| Bit        | Purpose                         | Example               |
| ---------- | ------------------------------- | --------------------- |
| **Sticky** | Only owner can delete in dir    | `/tmp` (`chmod +t`)   |
| **Setuid** | Run as file's owner (dangerous) | `/usr/bin/passwd`     |
| **Setgid** | Run with group ID of file       | Shared dev group dirs |

## 🔹 2. Read-only & nosuid Mount Options
### 🧠 Why?
>To harden the system against privilege escalation or accidental write/delete.

| Option   | Use Case                                           |
| -------- | -------------------------------------------------- |
| `ro`     | Read-only mount (no writes possible)               |
| `nosuid` | Prevent SUID/SGID binaries from working on this FS |
| `noexec` | Prevent execution of binaries                      |
| `nodev`  | Ignore device files on mount                       |

### 📌 Example /etc/fstab:
```bash
#/etc/fstab
/dev/sdb1  /mnt/secure  ext4  ro,nosuid,nodev,noexec  0  0
```

## 🔹 3. Mount Auditing Tools
### 🧠 Why?
> Track who accessed/modified files or created/deleted content.

### 📋 auditd
```bash
# Install
sudo apt install auditd
# Monitor a directory
auditctl -w /etc/ -p war -k etc-changes
# View logs
ausearch -k etc-changes
```

### 📋 inotify (Real-time file change monitor)
```bash
# Install
sudo apt install inotify-tools
# Monitor file creation/deletion in directory
inotifywait -m /var/www/html
```

## 🔹 4. Backup & Restore
### 🧠 Why?
> To ensure disaster recovery, migration, or rollback to earlier states.
### 🛠 rsync (Incremental, fast)
```bash
rsync -avz /source/ /backup/
# With delete sync
rsync -av --delete /source/ /backup/
```

### 🛠 tar (Archiving)
```bash
# Create compressed archive
tar -czf backup.tar.gz /home/user
# Extract
tar -xzf backup.tar.gz
```

### 🛠 dump & restore (For ext2/ext3/ext4 block-level backups)
```bash
# Dump
dump -0u -f /backup/root.dump /dev/sda1
# Restore
restore -r -f /backup/root.dump
```

### 🧠 Summary
| Task                     | Tool/Command                                    |
| ------------------------ | ----------------------------------------------- |
| Set file/directory perms | `chmod`, `chown`, `umask`, sticky/setuid/setgid |
| Secure mounts            | `ro`, `nosuid`, `noexec`, `nodev`               |
| Audit file changes       | `auditd`, `inotify`                             |
| Backup/Restore           | `rsync`, `tar`, `dump`, `restore`               |
