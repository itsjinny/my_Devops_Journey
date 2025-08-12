# 📝 Linux Log Management - Deep Dive
- [📝 Linux Log Management - Deep Dive](#-linux-log-management---deep-dive)
  - [🔹 1. Core Concepts of Logging](#-1-core-concepts-of-logging)
    - [📌 What is System Logging?](#-what-is-system-logging)
    - [📁 Types of Logs](#-types-of-logs)
    - [📍 Standard Log Locations in Linux](#-standard-log-locations-in-linux)
    - [🧠 Learn \& Explore](#-learn--explore)
      - [`/var/log/boot.log`](#varlogbootlog)
      - [`/var/log/dmesg`](#varlogdmesg)
      - [`/var/log/auth.log` or `/var/log/secure`](#varlogauthlog-or-varlogsecure)
      - [`/var/log/kern.log`](#varlogkernlog)
      - [`/var/log/faillog`](#varlogfaillog)
    - [✅ Pro Tips](#-pro-tips)
  - [🔹 2. Working with `journalctl` (Systemd Logs)](#-2-working-with-journalctl-systemd-logs)
    - [🧠 How `journald` Works](#-how-journald-works)
    - [📖 Viewing Logs with `journalctl`](#-viewing-logs-with-journalctl)
    - [📦 Persistent Journald Storage](#-persistent-journald-storage)
    - [🔍 Filtering Journal Logs](#-filtering-journal-logs)
  - [| 7        | debug   | Debugging messages      |](#-7---------debug----debugging-messages------)
    - [🔧 Manage, Rotate \& Clean Journal Logs](#-manage-rotate--clean-journal-logs)
  - [🔹 3. Traditional Logging: `rsyslog` / `syslog-ng`](#-3-traditional-logging-rsyslog--syslog-ng)
    - [🧠 How Syslog Protocol Works](#-how-syslog-protocol-works)
    - [⚙️ rsyslog: Configuration Basics](#️-rsyslog-configuration-basics)
    - [🔁 Remote Log Forwarding (DevSecOps Centralized Logging)](#-remote-log-forwarding-devsecops-centralized-logging)
    - [🧪 Filtering, Templates, and Custom File Destinations](#-filtering-templates-and-custom-file-destinations)
    - [🔍 Debugging rsyslog](#-debugging-rsyslog)
  - [🔹 4. Security \& Audit Logs](#-4-security--audit-logs)
    - [🔐 System Authentication Logs](#-system-authentication-logs)
    - [👥 Monitor Login/Logout Events](#-monitor-loginlogout-events)
    - [📦 `auditd` – Linux Auditing Framework](#-auditd--linux-auditing-framework)
    - [🧾 Configure `audit.rules`](#-configure-auditrules)
    - [🔍 Analyzing Logs with ausearch](#-analyzing-logs-with-ausearch)
  - [🔹 5. Log Rotation: logrotate](#-5-log-rotation-logrotate)
    - [🔄 Why Log Rotation Matters](#-why-log-rotation-matters)
    - [📂 Configuration Files](#-configuration-files)
    - [📦 Rotate Logs Manually](#-rotate-logs-manually)
    - [📝 Post-Rotate Scripts](#-post-rotate-scripts)
    - [🔍 Troubleshooting Tips](#-troubleshooting-tips)
  - [🔹 6. Remote Logging \& Centralization](#-6-remote-logging--centralization)
    - [📤 Forwarding Logs to Remote Server](#-forwarding-logs-to-remote-server)
    - [🔐 TLS Encryption with rsyslog](#-tls-encryption-with-rsyslog)
    - [📦 ELK Stack Basics (Elasticsearch, Logstash, Kibana)](#-elk-stack-basics-elasticsearch-logstash-kibana)
    - [🛰️ Log Agents: Filebeat \& Fluentd](#️-log-agents-filebeat--fluentd)
  - [🔹 7. Log Analysis \& Monitoring](#-7-log-analysis--monitoring)
    - [🔍 Searching Logs with CLI Tools](#-searching-logs-with-cli-tools)
      - [🔹 `grep`](#-grep)
    - [🌈 Colorizing Logs for Better Readability](#-colorizing-logs-for-better-readability)
    - [🌐 Web Server Log Analysis](#-web-server-log-analysis)
    - [🛡️ Monitoring Suspicious Activity](#️-monitoring-suspicious-activity)
  - [🔹 8. Hands-On Practice](#-8-hands-on-practice)
    - [🔧 1. Set Up and Rotate a Custom Log](#-1-set-up-and-rotate-a-custom-log)
      - [📁 Create Custom Log Directory \& File](#-create-custom-log-directory--file)
    - [🌀 Create a Logrotate Config](#-create-a-logrotate-config)
    - [🔁 2. Forward Logs from One VM to Another (rsyslog)](#-2-forward-logs-from-one-vm-to-another-rsyslog)
    - [⛓️ 3. Create journalctl Aliases](#️-3-create-journalctl-aliases)
    - [🔍 4. Monitor /etc/passwd with auditd](#-4-monitor-etcpasswd-with-auditd)
    - [📊 5. Visualize Logs with ELK Stack or Filebeat + Kibana](#-5-visualize-logs-with-elk-stack-or-filebeat--kibana)

## 🔹 1. Core Concepts of Logging

### 📌 What is System Logging?

System logging is the process of recording operating system and application-level events. These logs are critical in DevOps and DevSecOps for:

- ✅ **Debugging**: Track system/app failures.
- 🔒 **Security Auditing**: Detect unauthorized access or malicious activity.
- 📈 **Performance Monitoring**: Analyze resource usage trends.
- 🧠 **Compliance & Forensics**: Essential for audits and incident response.

> Think of logs as your system's **black box** — they tell you what happened, when, and by whom.

---

### 📁 Types of Logs

| Log Type     | Description                                     |
|--------------|-------------------------------------------------|
| System       | General OS-level operations (e.g., services)    |
| Auth         | Login attempts, sudo usage, user activity       |
| App          | Logs from services like nginx, apache, etc.     |
| Kernel       | Kernel events like hardware errors              |
| Firewall     | Packet drops, iptables actions                  |
| Audit        | Security and access auditing (via auditd)       |

---

### 📍 Standard Log Locations in Linux

| Path                        | Purpose                                                                 |
|-----------------------------|-------------------------------------------------------------------------|
| `/var/log/`                | Base directory for all system logs                                      |
| `/var/log/syslog`          | General system activity logs (Debian-based systems)                     |
| `/var/log/messages`        | System logs on RHEL/CentOS/Fedora systems                               |
| `/var/log/auth.log`        | Authentication logs (Debian/Ubuntu)                                     |
| `/var/log/secure`          | Authentication logs (RHEL/CentOS/Fedora)                                |
| `/var/log/kern.log`        | Kernel-specific messages (Debian/Ubuntu)                                |
| `/var/log/boot.log`        | Boot time logs from startup scripts                                     |
| `/var/log/dmesg`           | Boot-time hardware-related kernel ring buffer (useful for diagnostics)  |
| `/var/log/faillog`         | Failed login attempts                                                   |

> ⚠️ Log files and their structure might vary based on your distro and logging daemon (rsyslog, journald, syslog-ng).

---

### 🧠 Learn & Explore

#### `/var/log/boot.log`
- Shows messages during system startup.
- Useful for debugging slow or failed boots.

#### `/var/log/dmesg`
- Kernel ring buffer (also accessible via `dmesg` command).
- Great for spotting hardware issues, drivers, or mounting problems.

#### `/var/log/auth.log` or `/var/log/secure`
- Tracks all user logins, sudo activity, failed logins.
- Primary file for detecting brute-force attempts or privilege escalation.

#### `/var/log/kern.log`
- Logs kernel messages not shown in the standard syslog.
- Useful for kernel developers and diagnosing kernel panics or crashes.

#### `/var/log/faillog`
- Stores information about failed login attempts.
- View with `faillog` command.

---

### ✅ Pro Tips

- Use `tail -f /var/log/syslog` during troubleshooting for live updates.
- Rotate logs using `logrotate` to avoid disk space exhaustion.
- Secure logs using proper file permissions (`chmod`, `chown`).

---

## 🔹 2. Working with `journalctl` (Systemd Logs)

### 🧠 How `journald` Works

`systemd-journald` is the logging component of `systemd`. It collects logs from:
- The kernel (via `kmsg`)
- Services managed by systemd
- Standard output/error from applications
- Syslog messages (optional)

Journald stores logs in a binary format (not plain text) which allows for:
- Fast querying
- Rich metadata (PID, UID, service name, etc.)
- Structured filtering

By default, journald stores logs in **volatile memory (/run/log/)** — logs are lost after reboot unless configured for persistence.

---

### 📖 Viewing Logs with `journalctl`

```bash
journalctl              # View all logs
journalctl -u sshd      # View logs for a specific unit (e.g., sshd)
journalctl -b           # Logs from the current boot
journalctl -b -1        # Logs from the previous boot
```

### 📦 Persistent Journald Storage
> To make logs persistent across reboots, enable disk storage:
```bash
sudo mkdir -p /var/log/journal/
sudo systemd-tmpfiles --create --prefix /var/log/journal
sudo systemctl restart systemd-journald
```
- Now logs are saved under /var/log/journal/ even after reboot.

### 🔍 Filtering Journal Logs
```bash
journalctl --since "10 min ago"              # Logs from last 10 minutes
journalctl --since "2025-08-01 10:00"        # Logs from a specific time
journalctl --priority=3                      # Priority 3 (errors) and above
journalctl -p err                            # Shortcut for errors
journalctl -u nginx --since yesterday        # nginx logs since yesterday
journalctl | grep "authentication failure"   # Search for a keyword
```
- -p or --priority values range from 0 (emerg) to 7 (debug)

| Priority | Name    | Description             |
| -------- | ------- | ----------------------- |
| 0        | emerg   | System is unusable      |
| 1        | alert   | Immediate action needed |
| 2        | crit    | Critical condition      |
| 3        | err     | Error condition         |
| 4        | warning | Warning                 |
| 5        | notice  | Normal but significant  |
| 6        | info    | Informational messages  |
| 7        | debug   | Debugging messages      |
---

### 🔧 Manage, Rotate & Clean Journal Logs
```bash
journalctl --disk-usage                    # Show log size on disk
sudo journalctl --vacuum-time=2weeks       # Delete logs older than 2 weeks
sudo journalctl --vacuum-size=500M         # Keep only 500MB of logs
sudo journalctl --rotate                   # Manually rotate logs
sudo journalctl --flush                    # Flush runtime logs to disk
```

## 🔹 3. Traditional Logging: `rsyslog` / `syslog-ng`

### 🧠 How Syslog Protocol Works

The **syslog protocol** is a standard for message logging, used by most Linux/Unix systems.
- Logs have 2 key parts: **Facility** (source/type) and **Severity Level**.
- Messages can be written to files, displayed on terminals, or forwarded to a remote server.

Common Facilities:
- `auth`, `authpriv`, `daemon`, `kern`, `mail`, `user`, `local0-local7`

Severity Levels:
| Level | Name     | Description           |
|-------|----------|-----------------------|
| 0     | emerg    | System unusable       |
| 1     | alert    | Immediate action      |
| 2     | crit     | Critical conditions   |
| 3     | err      | Error                 |
| 4     | warning  | Warning               |
| 5     | notice   | Normal but important  |
| 6     | info     | Informational         |
| 7     | debug    | Debugging messages    |

---

### ⚙️ rsyslog: Configuration Basics

**Config file**: `/etc/rsyslog.conf` (main), additional rules in `/etc/rsyslog.d/*.conf`

Syntax format:
```lua
Examples:
authpriv.*    /var/log/secure
*.info        /var/log/messages
mail.*        -/var/log/mail.log   # "-" means async write (faster)
```

```bash
sudo systemctl restart rsyslog
```

### 🔁 Remote Log Forwarding (DevSecOps Centralized Logging)
To forward logs to a remote server:

- 1. Edit rsyslog.conf or /etc/rsyslog.d/50-default.conf:
```bash
*.* @192.168.1.10:514           # UDP (insecure)
*.* @@192.168.1.10:514          # TCP (more reliable)
```

- 2. Enable rsyslog as client & restart:
```bash
sudo systemctl enable rsyslog
sudo systemctl restart rsyslog
```

- 3. On the remote syslog server, allow receiving:
```bash
# /etc/rsyslog.conf
module(load="imudp")
input(type="imudp" port="514")

module(load="imtcp")
input(type="imtcp" port="514")
```

### 🧪 Filtering, Templates, and Custom File Destinations
- You can create custom templates for logs:
```bash
template(name="SimpleFormat" type="string" string="%msg%\n")
```
- Filter logs based on content:
```bash
if $msg contains "failed login" then /var/log/login_failures.log
```
- Split logs to separate files:
```bash
authpriv.*    /var/log/auth.log
cron.*        /var/log/cron.log
```

### 🔍 Debugging rsyslog
- Use logger to manually generate test logs:
```bash
logger -p local0.notice "This is a test log"
```
- And trace logs using:
```bash
sudo journalctl -u rsyslog
```

## 🔹 4. Security & Audit Logs

### 🔐 System Authentication Logs

Linux tracks all auth-related events for security auditing and incident response.

- `/var/log/auth.log` (Debian/Ubuntu)
- `/var/log/secure` (RHEL/CentOS/Fedora)

These files record:
- SSH login attempts (success/failure)
- `sudo` usage logs
- `su` command usage
- PAM (Pluggable Authentication Module) messages

Example commands:
```bash
# View failed SSH attempts
grep "Failed password" /var/log/auth.log

# View sudo usage
grep "sudo" /var/log/auth.log

# Monitor su attempts
grep "session opened" /var/log/auth.log | grep su
```

### 👥 Monitor Login/Logout Events
- Use the following tools to track session events:
```bash
last        # Shows login/logout history from /var/log/wtmp
lastlog     # Shows last login of all users
who         # Displays currently logged-in users
```
- To monitor login anomalies:
```bash
# Check for root login over SSH
grep "Accepted" /var/log/auth.log | grep "root"

# Track multiple failed attempts
grep "Failed password" /var/log/auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -nr
```

### 📦 `auditd` – Linux Auditing Framework
- `auditd` is a powerful daemon that tracks file accesses, system calls, and user actions.
```bash
sudo apt install auditd        # Debian/Ubuntu
sudo yum install audit         # RHEL/CentOS
```
- **Key Files**
`/etc/audit/auditd.conf` – main config

`/etc/audit/rules.d/audit.rules` – audit rules

`/var/log/audit/audit.log` – audit logs

### 🧾 Configure `audit.rules`
>Sample Rules
```bash
# Monitor access to /etc/shadow
-w /etc/shadow -p wa -k shadow_watch

# Track changes to /etc/passwd
-w /etc/passwd -p wa -k passwd_changes

# Monitor all sudo usage
-a always,exit -F arch=b64 -S execve -F path=/usr/bin/sudo -k sudo_log

# Watch specific user activity (UID 1001)
-a always,exit -F arch=b64 -F uid=1001 -S execve -k user_1001_exec
```
**Flags**
- -w = watch file/dir
- -p = permissions (r=read, w=write, x=execute, a=attribute change)
- -k = keyword/tag-S = syscall (execve, open, unlink, etc.)
```bash
#apply rules
sudo augenrules --load
sudo systemctl restart auditd
```
### 🔍 Analyzing Logs with ausearch
```bash
# Search logs by keyword
ausearch -k shadow_watch

# See all audit logs for a specific time
ausearch --start recent

# Filter by user ID
ausearch -ua 1001

# Filter by executable
ausearch -x /usr/bin/sudo
```

## 🔹 5. Log Rotation: logrotate

### 🔄 Why Log Rotation Matters

Without log rotation:
- Logs can grow indefinitely, consuming disk space
- System performance degrades due to large file reads
- Backups and analysis become difficult

**Log rotation** prevents this by:
- Archiving old logs
- Compressing them
- Deleting old backups after a retention period

---

### 📂 Configuration Files

`logrotate` is configured via:

- **Global config**: `/etc/logrotate.conf`
- **Per-service configs**: `/etc/logrotate.d/`

🔧 Global config example (`/etc/logrotate.conf`):
```conf
weekly                # Rotate weekly
rotate 4              # Keep 4 old logs
create                # Create new empty log after rotation
compress              # Compress with gzip
include /etc/logrotate.d
```
- 📄 Example in /etc/logrotate.d/nginx:
```conf
/var/log/nginx/*.log {
    daily
    missingok
    rotate 14
    compress
    delaycompress
    notifempty
    create 0640 www-data adm
    sharedscripts
    postrotate
        [ -s /run/nginx.pid ] && kill -USR1 `cat /run/nginx.pid`
    endscript
}
```
- **🛠 Key Directives Explained**

|Directive	|Description|
|-----------|-----------|
daily/weekly/monthly	|Rotation frequency
rotate N	|Keep N archives (old logs)
compress	|Use gzip to compress rotated logs
delaycompress	|Compress only from second rotation onward
notifempty	|Skip rotation if log file is empty
create	|Create new log file with permissions after rotation
missingok	|Don’t error if file missing
postrotate	|Run custom command after rotation

### 📦 Rotate Logs Manually
- To test rotation:
```bash
sudo logrotate -d /etc/logrotate.conf       # Dry run
sudo logrotate -f /etc/logrotate.conf       # Force rotation
```

### 📝 Post-Rotate Scripts
```bash
systemctl reload rsyslog
```
### 🔍 Troubleshooting Tips
- Use logrotate -d to debug config issues
- Check logs in /var/lib/logrotate/status
- Ensure permissions allow logrotate to read/write log files

## 🔹 6. Remote Logging & Centralization

Modern DevOps/DevSecOps requires centralized log collection for:

- 📊 Correlation and analysis
- 🔐 Security auditing
- 🔁 Faster troubleshooting
- ☁️ Long-term cloud storage

---

### 📤 Forwarding Logs to Remote Server

**Using rsyslog** on the client:
```bash
# /etc/rsyslog.conf or /etc/rsyslog.d/remote.conf
*.* @@remote-log-server.example.com:514   # TCP (use @ for UDP)
```
```bash
#Restart rsyslog
sudo systemctl restart rsyslog
```
- On the remote server, allow incoming syslog:
```bash
# Enable receiving on port 514
sudo nano /etc/rsyslog.conf
# Uncomment:
module(load="imtcp")
input(type="imtcp" port="514")
```

### 🔐 TLS Encryption with rsyslog
**To enable secure log transport:**
- Generate self-signed or CA-signed certificates
- Update rsyslog config on both client and server:
```conf
# Example TLS config for client:
*.* @@(o)remote.example.com:6514
$DefaultNetstreamDriverCAFile /etc/rsyslog.d/ca.pem
$ActionSendStreamDriverMode 1
$ActionSendStreamDriverAuthMode anon
```
### 📦 ELK Stack Basics (Elasticsearch, Logstash, Kibana)
**ELK = Powerful log aggregation and visualization stack.**
| Component     | Role                           |
| ------------- | ------------------------------ |
| Elasticsearch | Stores structured log data     |
| Logstash      | Parses and ingests log streams |
| Kibana        | Visualizes logs in dashboards  |

**Logs can be shipped via:**

- Logstash (direct input)

- Filebeat → Logstash → Elasticsearch

### 🛰️ Log Agents: Filebeat & Fluentd
**🧩 Filebeat**
- Lightweight log shipper by Elastic
- Monitors log files and forwards them
```yaml
# filebeat.yml snippet
filebeat.inputs:
- type: log
  paths:
    - /var/log/syslog

output.logstash:
  hosts: ["logstash.example.com:5044"]
```
**🔄 Fluentd**
- Flexible log collector, open source

- Supports many plugins (TCP, Kafka, HTTP, etc.)

- Can parse, buffer, and route logs to multiple destinations

```c
<source>
  @type tail
  path /var/log/syslog
  pos_file /var/log/fluentd.pos
  tag syslog
  format syslog
</source>

<match syslog>
  @type elasticsearch
  host elasticsearch.example.com
</match>
```

## 🔹 7. Log Analysis & Monitoring

Once logs are collected, the real value comes from **searching, analyzing**, and **monitoring** them in real time for system health, security, and performance.

---

### 🔍 Searching Logs with CLI Tools

#### 🔹 `grep`
Search for strings/patterns in logs.
```bash
grep "error" /var/log/syslog
grep -i "failed" /var/log/auth.log
```
🔹 `awk`:
Extract specific fields from log lines.

```bash
awk '{print $1, $5}' /var/log/syslog
```
🔹 `sed`: Stream edit or filter patterns.
```bash
sed -n '/error/p' /var/log/syslog
```
### 🌈 Colorizing Logs for Better Readability
🔹 `ccze`
Real-time colorful log viewing.

```bash
tail -f /var/log/syslog | ccze
```
🔹 `lnav (Logfile Navigator)`
Interactive log browsing and filtering.
```bash
lnav /var/log/syslog
```
- Supports SQL-like queries.

- Auto-detects formats like syslog, Apache, JSON.

### 🌐 Web Server Log Analysis
🔹 `goaccess`
Terminal-based or HTML report for Nginx/Apache logs.
```bash
goaccess /var/log/nginx/access.log -o report.html --log-format=COMBINED
```
🔹 `awstats`
Powerful, GUI-based log analyzer for web traffic.
```bash
# Needs Apache + CGI setup
# Parses access logs to generate graphs, traffic sources, etc.
```

### 🛡️ Monitoring Suspicious Activity
🔹 `fail2ban`
Monitors logs for brute-force attacks and bans malicious IPs.

```bash
/var/log/fail2ban.log
journalctl -u fail2ban
```
> Sample fail2ban jail:
```ini
[sshd]
enabled = true
port = ssh
filter = sshd
logpath = /var/log/auth.log
maxretry = 5
```
## 🔹 8. Hands-On Practice

This section focuses on **real-world, hands-on tasks** for log management. These exercises simulate DevOps + DevSecOps monitoring workflows.

---

### 🔧 1. Set Up and Rotate a Custom Log

#### 📁 Create Custom Log Directory & File
```bash
sudo mkdir -p /var/log/customapp
sudo touch /var/log/customapp/app.log
sudo chown syslog:adm /var/log/customapp/app.log
```
### 🌀 Create a Logrotate Config
```bash
sudo nano /etc/logrotate.d/customapp
```
```ini
/var/log/customapp/*.log {
    daily
    rotate 7
    compress
    missingok
    notifempty
    postrotate
        systemctl reload rsyslog
    endscript
}
```
**✅ Now run manually:**
```bash
sudo logrotate -f /etc/logrotate.d/customapp
```
### 🔁 2. Forward Logs from One VM to Another (rsyslog)
**🖥️ On Remote Log Server:**
```bash
sudo nano /etc/rsyslog.conf
# Uncomment or add:
module(load="imudp")
input(type="imudp" port="514")
module(load="imtcp")
input(type="imtcp" port="514")

# Save logs to /var/log/remote/
$template RemoteLogs,"/var/log/remote/%HOSTNAME%/%PROGRAMNAME%.log"
*.* ?RemoteLogs
```

```bash
#Enable and restart:
sudo systemctl restart rsyslog
```
**🧑‍💻 On Client VM:**
```bash
sudo nano /etc/rsyslog.d/50-default.conf
```
**Add**
```bash
*.* @@<REMOTE_SERVER_IP>:514
```
**Restart**
```bash
sudo systemctl restart rsyslog
```
### ⛓️ 3. Create journalctl Aliases
**Example: Monitor SSH!**
```bash
alias logs-ssh='journalctl -u sshd -f -n 50'
```
**Example: View Reboots**
```bash
alias boots='journalctl --list-boots'
```
**Add to ~/.bashrc or ~/.zshrc:
```bash
source ~/.bashrc
```
### 🔍 4. Monitor /etc/passwd with auditd
**Add Audit Rule:**
```bash
sudo auditctl -w /etc/passwd -p wa -k passwd-watch
```
**Check Logs:**
```bash
sudo ausearch -k passwd-watch
```
**Make it Persistent:**
```bash
sudo nano /etc/audit/rules.d/passwd.rules

#in the rules add below
-w /etc/passwd -p wa -k passwd-watch
```
**Reload**
```bash
sudo augenrules --load
```
### 📊 5. Visualize Logs with ELK Stack or Filebeat + Kibana
**Option 1: ELK Stack (local or Docker)**
- ElasticSearch stores logs.

- Logstash ingests & parses logs.

- Kibana visualizes dashboards.

**Option 2: Filebeat + Kibana
Lightweight agent that sends logs to Elastic/Kibana.**
```bash
sudo apt install filebeat
sudo filebeat modules enable system
sudo filebeat setup
sudo systemctl start filebeat
```
**Kibana URL: http://localhost:5601**

**✅ Pro Tip: Combine logrotate + rsyslog forwarding + Filebeat + Kibana = production-grade log pipeline.**