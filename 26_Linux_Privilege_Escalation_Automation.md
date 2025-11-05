# Day 26 – Automating Linux Privilege Escalation Checks
![30day_linux](https://github.com/user-attachments/assets/efaacc4c-4553-40ab-ae14-7ba4c9498329)

## Objective
Learn how to automate privilege escalation checks on Linux using scripts and well-known tools like `linpeas`, `lse`, and custom Bash one-liners. Focus on collecting misconfigurations efficiently and safely during post-exploitation or pentesting.

---

## 1️⃣ Why Automate Privilege Escalation?
Manual enumeration is time-consuming. Automated scripts speed up information gathering by listing misconfigurations, weak permissions, and exploitable binaries.  
However, **always review script outputs manually** before attempting exploitation.

---

## 2️⃣ Key Tools
### 🔹 LinPEAS
LinPEAS is a popular script for privilege escalation checks.
```bash
wget https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh
chmod +x linpeas.sh
./linpeas.sh
```
It checks for:
- SUID/SGID binaries
- Writable `/etc/passwd`
- Crontabs and PATH hijacks
- Capabilities, Docker, NFS, and kernel exploits

### 🔹 Linux Smart Enumeration (LSE)
A modular and cleaner enumeration tool:
```bash
wget https://github.com/diego-treitos/linux-smart-enumeration/releases/latest/download/lse.sh
chmod +x lse.sh
./lse.sh -l2
```
LSE provides detailed but safe enumeration without exploit attempts.

---

## 3️⃣ Writing Your Own Automation Script
You can build your own script combining simple commands:
```bash
#!/bin/bash
echo "[*] User info:"
id
echo "[*] Sudo permissions:"
sudo -l
echo "[*] SUID binaries:"
find / -perm -4000 2>/dev/null
echo "[*] Writable /etc files:"
find /etc -writable 2>/dev/null
```
Save as `enum.sh` and run with:
```bash
bash enum.sh > results.txt
```

---

## 4️⃣ Useful One-Liners
```bash
find / -type f -perm -4000 2>/dev/null       # Find SUID binaries
sudo -l                                      # Check sudo permissions
cat /etc/passwd | grep sh$                   # Users with shell access
getcap -r / 2>/dev/null                      # List files with capabilities
ls -la /root                                 # Try if root directory is accessible
```

---

## 5️⃣ Privilege Escalation Targets
- Misconfigured `sudo` rules (e.g., NOPASSWD)  
- Writable scripts executed as root (cron/systemd)  
- SUID binaries allowing command execution  
- Files with dangerous capabilities (`setcap cap_setuid+ep`)  
- Weak environment variable paths

---

## 6️⃣ Best Practices
- Run automated tools with **low noise** flags to avoid detection.
- Redirect output to local files for review (`tee`, `>`, or `2>/dev/null`).
- Keep your custom script modular — add functions per category (users, services, permissions).
- Always respect legal & ethical boundaries.

---

## 7️⃣ Challenge (Lab Task)
1. Run `linpeas.sh` and `lse.sh` on a lab VM.  
2. Compare results — note what each finds differently.  
3. Add one of your custom checks (like world-writable configs).  
4. Report findings in a simple table format (tool → finding → impact).

---

⚠️ Ethics & Safety: Run these tools **only on systems you own or are authorized to test**. Unauthorized scanning or exploitation is illegal.

---

📢 Follow CyberSecPlayground on Telegram for automated pentesting scripts and privilege escalation labs: https://t.me/cybersecplayground
