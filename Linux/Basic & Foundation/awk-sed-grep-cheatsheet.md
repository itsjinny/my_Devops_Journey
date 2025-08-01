# 🛠️ AWK, SED & GREP Cheatsheet

---

## 🔍 AWK (Field & Record Processor)

### 📌 Field & Record Separators
```awk
BEGIN { FS="," }      # Input Field Separator
BEGIN { OFS="," }     # Output Field Separator
BEGIN { RS="\n" }     # Input Record Separator
BEGIN { ORS=" " }     # Output Record Separator
```
---
### 🧮 Records & Fields
```awk
awk '/text/' file.txt         # Contains 'text'
awk '/^te/' file.txt          # Starts with 'te'
awk '/te$/' file.txt          # Ends with 'te'
awk '/te.t/' file.txt         # Regex match
awk '/te[xt]/' file.txt       # Character class match
```

### 🔎 Searching with AWK
```awk
awk '/text/' file.txt         # Contains 'text'
awk '/^te/' file.txt          # Starts with 'te'
awk '/te$/' file.txt          # Ends with 'te'
awk '/te.t/' file.txt         # Regex match
awk '/te[xt]/' file.txt       # Character class match
```

### 🔄 Control Structures
```awk
if (condition) { ... }
if (c1) {...} else if (c2) {...} else {...}
while (condition) { ... }
do { ... } while (condition)
for (i = 1; i <= 10; i++) { ... }
for (item in array) { ... }
```

### 🔧 Practical Examples
```awk
awk '{ print $0 }' file.txt
awk '{ print $1, $3 }' file.txt
awk '/pattern/ { print }' file.txt
awk 'NR % 2 == 0 { print }' file.txt
awk '{ sum += $1 } END { print sum }' file.txt
awk '{ total += $1 } END { print total/NR }' file.txt
```
## 📈 Log Analysis & Monitoring
### 📌 Extract Fields
```awk
awk '{print $1}' access.log                # IP addresses
awk '{print $9}' access.log | sort | uniq -c  # HTTP status codes
```
### 🕐 Time Filters
```awk
awk '/2023-10-01 12:00/,/2023-10-01 13:00/' /var/log/syslog

# Last 10 mins logs
awk -v d1="$(date -d '10 minutes ago' '+%b %d %H:%M')" \
    -v d2="$(date '+%b %d %H:%M')" '$0 > d1 && $0 < d2' /var/log/auth.log
```
### 🚨 Security Filtering
```awk
awk '$9 ~ /5[0-9]{2}/ {print $9}' access.log | sort | uniq -c   # 5xx
awk '$9 == 404 {print $7}' access.log | sort | uniq -c          # 404 by URL
```
### 🚨Failed SSH
```awk
awk '/Failed password/ {print $(NF-3)}' /var/log/auth.log | sort | uniq -c

# Invalid user login attempts
awk '/Invalid user/ {print $NF}' /var/log/auth.log | sort | uniq -c
```
## 🧠 DevOps Monitoring
### High memory usage
```awk
ps aux | awk '$4 > 5.0 {print $0}'
```
### Memory per user
```awk
ps aux | awk '{user[$1] += $4} END {for (u in user) print u, user[u]"%"}'
```
### Disk usage > 60%
```awk
df -h | awk 'NR>1 && $5+0 > 60 {print $1, $6, $5}'
```
### Request rate/min
```awk
cat log.txt | awk '{print $4}' | sort | cut -d: -f1-2 | uniq -c
```
---
---

# 🔧 SED (Stream Editor)
### 📌 Basic Substitution
```awk
sed '/lgo/p' log.txt
sed 's/lgo/bar/' log.txt         # First occurrence
sed 's/lgo/bar/g' log.txt        # All occurrences
sed '2s/lgo/bar/' log.txt        # Line-specific
```
### 🧹 Line Operations
```awk
sed '1d' log.txt                 # Delete first line
sed '1,4d' log.txt               # Delete line 1–4
sed '/log/d' log.txt             # Delete lines with 'log'
```
### 💾 In-place Editing
```awk
sed -i 's/log/bar/g' log.txt
sed -i.bak 's/log/bar/g' log.txt   # Create backup
```
### 🧪 Multiple Commands
```awk
sed -e 's/bar/foo/' -e 's/text/replace/' log.txt
```
### 🔁 Insert, Append, Change
```awk
sed '2i hello' log.txt           # Insert before line
sed '2a hello' log.txt           # Append after line
sed '2c Replaced' log.txt        # Change whole line
```
### 🧠 Advanced Patterns
```awk
sed -n '/^ERROR.*/p' logs.txt
sed '/^$/d' file.txt             # Delete blank lines
sed 's/test/TEST/2' file.txt     # Replace 2nd occurrence only
sed 's/[0-9]\+/**&**/g' file.txt # Use & for match
sed '/pattern/r insert.txt' file.txt
```
---
---
# 🕵️ GREP (Global Regex Print)
### 🔍 Basic Usage
```awk
grep -i "error" file.txt         # Case insensitive
grep -c "error" file.txt         # Count matches
grep -n "error" file.txt         # Show line numbers
grep -v "error" file.txt         # Inverse match
```
### 📁 Recursive & File Filters
```awk
grep -r "error" .                    # Recursive search
grep -r "error" . --exclude="*.json" # exclude json files
grep -r "error" . --exclude="*.log"  # exclude log files
```
### ⚙️ Advanced Patterns
```awk
grep "^[0-9]\{3\}" file.txt
grep -E "foo|bar|txt" file.txt
grep -e "P1" -e "P2" file.txt
grep -o "error" file.txt         # Only matched word
```
### ⏳ Contextual Search
```awk
grep -A 2 -B 2 "error" file.txt  # Show 2 lines before/after
```
### 📦 Special Cases
```awk
zgrep "error" /var/log/log.1.gz
```
### IP matching
```awk
grep -Eo '((25[0-5]|2[0-4][0-9]|[0-9]|[1-9][0-9]|1[0-9]{2})\.){3}(25[0-5]|2[0-4][0-9]|[0-9]|[1-9][0-9]|1[0-9]{2})' file.txt
```
### Email extraction
```awk
grep -Eo "[a-zA-Z_\\.%-+]+@(?:[a-zA-Z0-9-]+\\.)+[a-zA-Z]{2,}" file.txt
```
