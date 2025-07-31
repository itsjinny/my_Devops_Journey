# Service Management
---
---
## Systemctl
---

```awk
sudo systemctl list-units -t services   #List services

sudo systemctl status nginx #Check status and latest log entries for a service

sudo systemctl stop nginx   #Stop a service

sudo systemctl start nginx  #Start a service

sudo systemctl restart nginx    #Restart a service

sudo systemctl enable nginx #start service on boot

sudo systemctl disable nginx #Don't start service along with boot

sudo systemctl mask nginx #Masks the service - Prevents from starting

sudo systemctl unmask nginx #unmasks the service

sudo systemctl get-default #list the default runlevel of system

sudo systemctl set-default multi-user.target #change the runlevel

sudo systemctl list-unit-files #list unit files(configuration files that defines and manages services) of the services

sudo systemctl list-dependencies <unit-file> #lists the dependency of unit-files

e.g. sudo systemctl list-dependencies dbus.service

sudo systemctl list-sockets #Lists all active sockets

sudo systemctl list-jobs #List all active jobs

sudo systemctl list-units #show the status of active and loaded unit files
```

### Creating a Self Made service
```awk
#my process binary: /usr/bin/local/my_script.sh

#Add file my_service.service to /etc/systemd/system

#Add below snippet

[Unit]
Description=My Service
After=network.target

[Service]
User=tom
ExecStart=/bin/bash /usr/bin/local/my_script.sh

[Install]
WantedBy=multi-user.target
```
---

## Jorurnalctl(log analysis)
```awk
journalctl #shows the service logs

journalctl --no-pager #display the entire logs without pager

journalctl -r #display logs in reverse order i.e. last log at first

journalctl -n 25 #display latest 25 logs only

journalctl -f #Real time logs

journalctl --utc #show log timing in utc format

journalctl -k #Show only kernel message logs

journalctl -u nginx #show logs for specific service

journalctl -b #show boot logs or use --list-boots

journalctl -b -2 # show logs for booted session 0 is the current booted session -1 is last and so on

journalctl --since=yesterday --until=now # show logs for specific time interval

journalctl --since "2020-07-10 15:10:00" --until "2020-07-12"

journalctl _PID=1234 #filter logs based on the UID, GID, PID

journalctl -xe #show last few logs with extra information

journalctl --disk-usage #the disk space that logs are taking

journalctl -p3 #show logs priority wise
```

## Journalctl logs type priority

```awk
Priority	Code
0	        emerg
1	        alert
2	        crit
3	        err
4	        warning
5	        notice
6	        info
7	        debug
```

## Clearing journal logs
```awk
sudo systemctl --rotate #Marks the current active logs as archive

1. sudo systemctl --vacuum-time=2d #clear the logs of a specific duration

2. sudo systemctl --vacuum-size=100M #cleans the logs untill the specified size of logs are remaining

3. sudo systemctl --vacuum-files=5 #clean the files untill only 5 log file are remaining
```

## Automating Log cleaning
```awk
#To Automate the process, we need to edit the configuration file /etc/systemd/journald.conf

#below Directives Setting in the configuration files can automate the log cleaning

1. SystemMaxUse: Max disk space logs can take
2. SystemMaxFileSize: Max size of an INDIVIDUAL log file
3. SystemMaxFiles: Maz number of log files
```