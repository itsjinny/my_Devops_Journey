
## 🐧 Basic Linux Commands

### 🔍 File & Directory Navigation
```
pwd                  # Print current working directory
ls                   # List directory contents
ls -l                # Long listing
ls -a                # Show hidden files
cd /path/to/dir      # Change directory
cd ~                 # Go to home directory
cd ..                # Go up one directory
```

### 📁 File & Directory Management
```
touch file.txt               # Create empty file
mkdir new_folder             # Create new directory
mkdir -p a/b/c               # Create nested directories
rm file.txt                  # Delete file
rm -r folder/                # Delete directory recursively
cp file1.txt file2.txt       # Copy file
cp -r dir1/ dir2/            # Copy directory
mv file1.txt file2.txt       # Rename or move file
```

### 📄 Viewing File Contents
```
cat file.txt                 # Display file contents
tac file.txt                 # Display file in reverse
head file.txt                # First 10 lines
tail file.txt                # Last 10 lines
tail -f file.txt             # Monitor live appended output
less file.txt                # Scrollable file viewer
more file.txt                # Similar to less (older)
```

### 🔎 Searching Files & Text
```
find /path -name "file.txt"      # Search for file
grep "text" file.txt             # Search text in file
grep -r "text" /path             # Recursive grep
grep -i "text" file.txt          # Case-insensitive grep
```

### 🛠️ File Permissions
```
chmod +x script.sh           # Make file executable
chmod 755 file.sh            # Set file permissions
chown user:group file.txt    # Change file owner
```

### 📦 Package Management (Debian-based)
```
sudo apt update              # Update package list
sudo apt upgrade             # Upgrade installed packages
sudo apt install package     # Install a package
sudo apt remove package      # Remove a package
```

### 🧠 Process & System Info
```
ps aux                       # List all processes
top                          # Real-time system monitor
htop                         # Enhanced version of top
kill PID                     # Kill process by PID
kill -9 PID                  # Force kill process
```

### 🧑‍💻 User Management
```
whoami                       # Current username
id                           # Show user ID and groups
adduser username             # Add new user
passwd username              # Set user password
su - username                # Switch user
```

### 🌐 Networking
```
ip a                         # Show IP addresses
ifconfig                     # Show network interfaces (older)
ping google.com              # Test network connectivity
netstat -tulnp               # Show listening ports
ss -tulnp                    # Modern alternative to netstat
curl ifconfig.me             # Get public IP
```

### 🧹 Disk & File Usage
```
df -h                        # Show disk usage
du -sh folder/               # Folder size summary
du -ah                       # Show sizes of all files
```

### ⏳ Date & Time
```
date                         # Current date and time
cal                          # Calendar
timedatectl                  # View/set system time (modern)
```

### 🔄 Archiving & Compression
```
tar -cvf archive.tar file    # Create tar archive
tar -xvf archive.tar         # Extract tar archive
tar -czvf file.tar.gz file   # Create gzip compressed archive
tar -xzvf file.tar.gz        # Extract gzip archive
```

### 🔄 System Reboot & Shutdown
```
sudo shutdown now            # Shutdown immediately
sudo reboot                  # Reboot system
```

### 💡 Misc
```
echo "Hello"                 # Print to screen
history                      # Show command history
clear                        # Clear terminal
alias ll="ls -la"            # Create alias
```
