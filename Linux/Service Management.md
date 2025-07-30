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
