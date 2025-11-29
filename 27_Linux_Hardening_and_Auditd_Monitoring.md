# Day 27 – Linux Hardening & Auditd Monitoring
<img width="1200" height="400" alt="image" src="https://github.com/user-attachments/assets/e440d65c-03ff-44d0-9d92-4ab6abddedbc" />

## Objective
Introduce essential Linux hardening techniques and teach how to monitor critical system activity using `auditd`, a powerful auditing framework for security teams, IR responders, and pentesters simulating defenders.


---

## 1️⃣ Why Hardening Matters
A hardened system reduces the attack surface, limits privilege escalation paths, and provides logs that matter during investigations.  
Hardening is **proactive defense** — stopping attacks before they happen.

---

## 2️⃣ Install & Enable Auditd
```bash
sudo apt install auditd audispd-plugins -y
sudo systemctl enable --now auditd
```

Check status:
```bash
auditctl -s
```

---

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

---

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

---

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

---

## 6️⃣ Pentester Perspective
Hardened systems:
- Reduce common privilege escalation paths  
- Leave meaningful logs  
- Increase attacker cost  
- Detect noise quickly  

Understanding defender configs = better Red Teaming.

---

## 7️⃣ Lab Task
1. Install auditd on a VM.  
2. Add rules from section 3.  
3. Perform system actions (chmod, passwd, sudo).  
4. Use ausearch to trace everything.  
5. Write a short report on high-value findings.

---

📢 Follow CyberSecPlayground on Telegram for more labs:  
https://t.me/cybersecplayground
