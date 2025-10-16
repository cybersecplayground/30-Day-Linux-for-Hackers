# Day 25 – Linux Forensics & Incident Response Basics
![30day_linux](https://github.com/user-attachments/assets/b41d2536-26d2-4abf-a21e-d54853866f86)

## Objective
Introduce practical Linux forensics and incident response (IR) techniques that are useful immediately after a suspected compromise. Focus is on safe evidence collection, key artifacts, triage commands, and basic analysis. Use only in authorized contexts.

---

## 1️⃣ Triage: Preserve Evidence First
- **Don't reboot** the system unless absolutely necessary — volatile evidence (memory, ephemeral network connections) will be lost.  
- If you must interact, **work from a separate admin host** and use remote read-only commands where possible.  
- Create a timeline: note detection time, user actions, and commands run during triage.

---

## 2️⃣ Volatile Data Collection (memory & network)
```bash
# Capture running processes and network connections
ps aux > /tmp/ir_ps.txt
ss -tunap > /tmp/ir_ss.txt

# Dump open files and sockets
lsof -nP > /tmp/ir_lsof.txt

# Capture running kernel messages
dmesg > /tmp/ir_dmesg.txt
```
For memory captures, use `avml`, `lime`, or `volatility` tools in a lab; avoid pulling large memory dumps unless required and done safely.

---

## 3️⃣ Key Artifacts to Collect
- `/var/log/auth.log`, `/var/log/syslog`, `journalctl` output  
- User histories: `~/.bash_history`, `~/.sh_history` (may be overwritten)  
- Crontabs: `/etc/crontab`, `/etc/cron.d/*`, `crontab -l -u <user>`  
- SSH keys and `~/.ssh/authorized_keys`  
- SUID/SGID binaries: `find / -perm -4000 -o -perm -2000 2>/dev/null`  
- Recently modified files: `find / -mtime -7 -type f 2>/dev/null`  

---

## 4️⃣ Timeline Building (file and event times)
Use `stat` and `journalctl` to map events to absolute times:
```bash
stat /path/to/suspicious_file
journalctl --since "2025-10-01" --until "2025-10-07"
```
Convert all times to UTC in your notes to avoid timezone confusion.

---

## 5️⃣ Suspicious Indicators to Look For
- New user accounts or unexpected `sudo` entries (`cat /etc/sudoers` and `sudo -l` for affected users)  
- New systemd units or timers (`ls /etc/systemd/system`, `systemctl list-timers --all`)  
- Unexpected listening ports or reverse shells (`ss -tunap`, `ps aux | grep -E "bash|python|perl|nc"`)  
- Modified binaries or tampered logs (check file hashes against known-good copies)

---

## 6️⃣ Hashing & Evidence Integrity
Create cryptographic hashes of collected artifacts and store them in a secure location:
```bash
sha256sum /tmp/ir_ps.txt > /tmp/ir_ps.txt.sha256
sha256sum /var/log/auth.log > /tmp/auth.log.sha256
```
Record who collected evidence, date/time, and chain-of-custody notes.

---

## 7️⃣ Quick Analysis Tips
- Grep for suspicious strings: IPs, domains, `bash -i`, `netcat`, or base64 payloads.  
- Use `strings` on suspicious binaries and `file` to detect architecture.  
- Use `git` or `diff` to compare configuration directories vs backups.

---

## 8️⃣ Containment & Remediation (short-term)
- Isolate the host from networks (airgap or VLAN) while preserving evidence.  
- Change credentials in upstream services if lateral movement suspected.  
- Apply temporary firewall rules (`iptables`/`ufw`) to block C2 IPs while investigating.

---

## 9️⃣ Reporting & Handover
Produce a concise incident report: timeline, artifacts collected (with hashes), actions taken, and recommended remediation. Handover evidence and notes to SOC or IR team for deeper analysis.

---

## 10️⃣ Further Reading & Tools
- Volatility/Volatility3, AVML, LiME (memory capture)  
- GRR Rapid Response, OSQuery for endpoint telemetry  
- The SANS DFIR resources and incident response playbooks

---

⚠️ Ethics & Safety: Only perform IR on systems you own or are authorized to analyze. Unauthorized access or evidence collection is illegal.

---

📢 Follow CyberSecPlayground on Telegram for IR playbooks, scripts, and weekly writeups: https://t.me/cybersecplayground
