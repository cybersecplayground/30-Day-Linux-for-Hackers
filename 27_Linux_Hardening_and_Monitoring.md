# Day 27 – Linux Hardening & Auditd , Monitoring , Output Redirection, Logging for Hackers
<img width="1200" height="400" alt="image" src="https://github.com/user-attachments/assets/e440d65c-03ff-44d0-9d92-4ab6abddedbc" />

## Objective
Introduce essential Linux hardening techniques and teach how to monitor critical system activity using `auditd` and capturing output, logging everything, and monitoring systems in real-time without breaking stealth , a powerful auditing framework for security teams, IR responders, and pentesters simulating defenders.




## 1️⃣ Why Hardening Matters
A hardened system reduces the attack surface, limits privilege escalation paths, and provides logs that matter during investigations.  
Hardening is **proactive defense** — stopping attacks before they happen.



## 2️⃣ Install & Enable Auditd
```bash
sudo apt install auditd audispd-plugins -y
sudo systemctl enable --now auditd
```

Check status:
```bash
auditctl -s
```



## 3️⃣ Critical Audit Rules (High-Value Targets)

### 🔹 Monitor `/etc/passwd` and `/etc/shadow`
```bash
-w /etc/passwd -p wa -k passwd_changes
-w /etc/shadow -p wa -k shadow_changes
```

### 🔹 Monitor privilege escalation binaries
```bash
-w /usr/bin/sudo -p x -k sudo_exec
-w /bin/su -p x -k su_exec
```

### 🔹 Track binary execution
```bash
-a always,exit -F arch=b64 -S execve -k exec_log
```

### 🔹 Detect changes in perms/ownership
```bash
-a always,exit -F arch=b64 -S chmod,chown,fchmod,fchown -k perms_changes
```

Reload rules:
```bash
sudo augenrules --load
```


## 4️⃣ Reviewing Audit Logs
```bash
ausearch -k exec_log
ausearch -k passwd_changes
aureport --summary
aureport -x --summary
```

These reveal:
- Unexpected program execution  
- Privilege escalation attempts  
- File modification attempts  
- Recon or lateral movement behavior  


## 5️⃣ Additional Hardening Steps

### 🔹 Disable unnecessary services
```bash
systemctl disable --now service_name
```

### 🔹 Secure SSH
- Disable root login  
- Disable passwords  
- Use keys + fail2ban  

### 🔹 Tighten sudo rules
```bash
visudo
```

### 🔹 Kernel protections (sysctl)
```bash
kernel.kptr_restrict=2
kernel.dmesg_restrict=1
fs.protected_symlinks=1
```

## 6️⃣ Output Redirection (>, >>, 2>, &>)    

**Write output to a file**
```
ls -la > output.txt
```
→ Overwrites existing content.   

**Append instead of overwrite**       
```
ls -la >> output.txt
```

**Capture errors only**       
```
ls /root 2> errors.log
```

## 7️⃣ `tee` Command — The Hacker’s Real-Time Logger
`tee` lets you save output to a file while still seeing it on the terminal.    
Perfect for recon, exploit debugging, brute-force monitoring, and automation    
**Basic usage**    
```
nmap -sV target.com | tee scan.log
```

## 8️⃣ Live Monitoring with `tail`, `less +F`, `watch`, and `multitail`

**Watch a log in real time**
```
tail -f /var/log/auth.log
```

**Better than tail:**
```
less +F /var/log/syslog
```

**Repeated command execution**
```
watch -n 1 "ss -tupan | grep 443"
```

**Multi-pane log watching**
(install if needed)
```
multitail /var/log/syslog /var/log/auth.log
```

## 9️⃣ Capture Tool Output Invisibly (Useful for CTFs & Red Team)

**Redirect everything silently:**
```
command > /dev/null 2>&1
```
Perfect for hiding commands during enumeration.


## 1️⃣0️⃣ Scripted Logging for Recon Tools

Example: log every subfinder run with timestamp.
```
#!/bin/bash
timestamp=$(date +"%Y-%m-%d-%H-%M")
subfinder -d example.com | tee -a logs/subs-$timestamp.txt
```

## 1️⃣1️⃣ System Activity Monitoring for Hackers

**Monitor processes**
```
top
htop
atop
```

**Monitor network**
```
iftop
nethogs
```

**Monitor filesystem activity**
```
iotop
```
These help understand if your exploit crashed a service or caused load.


## 1️⃣2️⃣ Monitor file changes

**Powerful for privilege escalation and persistence detection.**
```
watch -n 1 "ls -l /etc/passwd"
```

**Or continuous diff:**
```
diff <(ls /etc) <(ls /etc)
```



## 1️⃣3️⃣ Silent Logging for Persistence

**Collect system data without drawing attention.**
```
ps aux >> /tmp/.cache 2>/dev/null
id >> /tmp/.cache
whoami >> /tmp/.cache
uname -a >> /tmp/.cache
```
## Pentester Perspective
Hardened systems:
- Reduce common privilege escalation paths  
- Leave meaningful logs  
- Increase attacker cost  
- Detect noise quickly  

Understanding defender configs = better Red Teaming.



## Lab Task
1. Install auditd on a VM.  
2. Add rules from section 3.  
3. Perform system actions (chmod, passwd, sudo).  
4. Use ausearch to trace everything.  
5. Write a short report on high-value findings.



📢 Follow CyberSecPlayground on Telegram for more labs:  
https://t.me/cybersecplayground
