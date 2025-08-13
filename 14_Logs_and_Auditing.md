# Day 14 – Logs & System Auditing
<img width="1200" height="400" alt="image" src="https://github.com/user-attachments/assets/889f5ffe-e362-4c06-a357-c83ab0bb131b" />

Mastering Linux Logs: A Hidden Goldmine for Admins & Ethical Hackers
## Objective  
Learn how Linux logs activities, how to query them, and ways attackers might use or evade them.
In the world of Linux, logging systems are often ignored—yet they hold the keys to diagnosing critical issues, uncovering security breaches, and tracing unauthorized access. For system administrators and cybersecurity professionals, logs provide the who, what, and why behind system failures and intrusions.

For penetration testers and ethical hackers, logs are the digital fingerprints that can expose your activity. The most effective attacks leave no trace, making log manipulation a crucial skill. But before you can cover your tracks, you must deeply understand Linux’s logging architecture.

With systemd now the standard in most Linux distributions, traditional syslog has been replaced by journalctl—a powerful yet often misunderstood tool. Whether you're defending a system or testing its resilience, mastering journalctl and modern logging techniques is essential for staying undetected and maintaining control.

---

## 1️⃣ Common Log Locations  
| Path                  | Description                      |
|-----------------------|----------------------------------|
| `/var/log/auth.log`   | Login attempts and sudo usage    |
| `/var/log/syslog`     | System-wide events and messages  |
| `/var/log/kern.log`   | Kernel-level debugging info      |
| `journalctl`          | Central log viewer on systemd    |


## ⚡️ Some Examples:
🔸Initial Reconnaissance
```
journalctl -q –since “24 hours ago”
```
-This command quietly retrieves the last 24 hours of logs. The -q flag suppresses informational messages, reducing the noise and potential traces of your activities.

🔸Investigating User Activities
```
journalctl _UID=1000 –since “24 hours ago”
```
-This reveals that user “air” (UID 1000) has been actively using the system and has used “sudo” several times.

---

## 2️⃣ Enumerating Logs  
```bash
grep "Failed" /var/log/auth.log       # Identify failed logins
tail -n 50 /var/log/syslog            # View recent system messages
journalctl -p err                     # Show only error-level logs
```

Use timestamps to map attacker movement or detect anomalies.

---

## 3️⃣ Attacker Use Cases  
- **Log manipulation:**  
  ```bash
  echo "" > /var/log/auth.log
  ```
- **Timestamp fuzzing:**  
  Setting incorrect system time to confuse timeline analysis.
- **Log redirection:**  
  Using custom scripts to divert logging into attacker-controlled files.

---

## 4️⃣ Task of the Day  
1. Inspect authentication failures:
    ```bash
    grep "Failed" /var/log/auth.log
    ```
2. Monitor system events real-time:
    ```bash
    tail -f /var/log/syslog
    ```
3. Correlate logs with potential attack stages using timestamps.

---

##  Pentester Tip  
Logs are the eyes of defenders. Tampering with them is powerful, but leaves its own trace — stealth matters.

---

**Follow CyberSecPlayground** on Telegram for more Linux hacking, profiling, and evasive techniques:  
[https://t.me/cybersecplayground](https://t.me/cybersecplayground)
