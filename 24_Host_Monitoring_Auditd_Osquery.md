# Day 24 – Host-based Monitoring: auditd & osquery
<img width="1200" height="400" alt="image" src="https://github.com/user-attachments/assets/889f5ffe-e362-4c06-a357-c83ab0bb131b" />

## Objective
Introduce host-based monitoring using `auditd` (kernel auditing) and `osquery` (SQL-based endpoint visibility). Learn quick deployment, common rules to watch for compromise, and basic detection use-cases. All examples are for authorized lab environments and production hardening.

---

## 1️⃣ Why Host-based Monitoring?
Network logs are useful, but attackers often leave traces on the host. `auditd` provides kernel-level audit events (file access, execs, attrib changes). `osquery` gives flexible querying of system state (processes, packages, scheduled jobs) and is great for continuous monitoring and hunting.

---

## 2️⃣ auditd – Quick Start
Install and enable `auditd` (Debian/Ubuntu):
```bash
sudo apt update && sudo apt install auditd audispd-plugins -y
sudo systemctl enable --now auditd
```
Basic commands:
```bash
auditctl -l            # list current rules
ausearch -m EXECVE     # search exec events
ausearch -f /etc/passwd  # file access events
ausearch -ts recent   # recent events
aureport -x            # summary of executed commands
```
Add example rules (lab/test):
```bash
# Watch for changes to passwd/shadow
sudo auditctl -w /etc/passwd -p wa -k passwd_changes
sudo auditctl -w /etc/shadow -p wa -k shadow_changes

# Log executions of su/sudo
sudo auditctl -a always,exit -F arch=b64 -S execve -F path=/usr/bin/sudo -k sudo_exec
```
Logs are in `/var/log/audit/audit.log` and can be forwarded to SIEM.

---

## 3️⃣ osquery – Quick Start
Install osquery (packages available for many distros). Basic usage:
```bash
# Interactive shell
osqueryi

# Example queries
osqueryi "SELECT pid, name, path, cmdline FROM processes WHERE on_disk = 0;"
osqueryi "SELECT name, path FROM crontab WHERE user != 'root';"
```
osquery can run scheduled queries and return results to a central logger.

---

## 4️⃣ Detection Use-Cases & Rules
- Watch for new systemd unit files in `/etc/systemd/system` or `~/.config/systemd/user`.  
  - `auditd` rule on `/etc/systemd/system` and `/lib/systemd/system`.  
- Detect creation of `authorized_keys` or changes to `~/.ssh/authorized_keys`.  
- Alert on executions of suspicious binaries (`execve` events where cmdline contains `bash -i` or `nc`).  
- Monitor unexpected network listeners: `osquery` query on `listening_ports` table.  

Example osquery query for listening ports:
```sql
SELECT pid, address, port, process_name FROM listening_ports;
```

---

## 5️⃣ Tuning & Performance
- Keep audit rules focused to avoid performance/saturation. Use keys (`-k`) to filter.  
- Use osquery packs for scheduled queries and limit frequency.  
- Forward logs to SIEM or ELK for aggregation and alerting.

---

## 6️⃣ Practical Tasks (lab)
1. Install `auditd` and add a rule to watch `/etc/passwd`. Check `/var/log/audit/audit.log` after making a test edit.  
2. Install `osquery` and run queries to list scheduled tasks and listening ports.  
3. Create an osquery schedule to run a query every minute and inspect results.  
4. Configure auditd to log execve events for `/usr/bin/sudo` and generate a report with `ausearch`/`aureport`.

---

## 7️⃣ Defensive Recommendations
- Centralize host logs to SIEM.  
- Baseline outputs from osquery and alert on deviations.  
- Use FIM alongside auditd for integrity checks.  
- Regularly review and prune audit rules to retain signal.

---

## 8️⃣ References
- `man auditctl`, `man ausearch`, `osquery` docs and packs.  
- Deploy guides for production `auditd` and osquery fleet management (FleetDM).

---

⚠️ Ethics & Safety: Use these tools on systems you control or are authorized to monitor.

---

📢 Follow CyberSecPlayground for practical monitoring playbooks and detection queries: https://t.me/cybersecplayground
