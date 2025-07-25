# Day 7 – Bash Variables, Arguments & Dynamic Scripting
<img width="1200" height="400" alt="image" src="https://github.com/user-attachments/assets/889f5ffe-e362-4c06-a357-c83ab0bb131b" />

To go beyond static scripts, you must master **variables** and **arguments** in Bash. These tools turn one-off commands into scalable, repeatable, dynamic payloads and automation tools.

---

## 🧠 Why Use Variables?

Variables allow you to store data (like usernames, ports, IPs, payloads) and reference them throughout your script.

Example:
```bash
name="CyberSec"
echo "Welcome, $name!"
```

💻 Script Arguments: $1, $2, $@
When running scripts, you can pass in values externally:

```
#!/bin/bash
echo "User: $1"
echo "Password: $2"
```
Run with:
```
./login.sh admin admin123
```
| Symbol | Meaning         |
| ------ | --------------- |
| `$1`   | First argument  |
| `$2`   | Second argument |
| `$@`   | All arguments   |

🔁 With Loops
Example script: multi_ping.sh
```
#!/bin/bash
for ip in "$@"; do
  echo "Pinging $ip..."
  ping -c 1 $ip
done
```

Run it:
```
./multi_ping.sh 192.168.1.1 192.168.1.2 192.168.1.3
```
🧪 Task of the Day
Build a script called enum.sh:
```
#!/bin/bash
target=$1
echo "[*] Running scan on $target"
nmap -Pn -sS $target
```
Make it executable:
```
chmod +x enum.sh
./enum.sh 10.10.10.10
```
Extend it to accept port ranges or scripts

## 🔐 Real Use Case
### Let’s say you want to:

- Auto-recon targets from a list   
- Brute-force web login pages   
- Drop backdoors on multiple IPs   
With `$1`, `$2`, `$@`, and for loops — your Bash script becomes multi-target, adaptable, and scalable.  

📡 Learn advanced Bash and hacking workflows here:
➡️ https://t.me/cybersecplayground

---

Be ready for **Day 8**, where we’ll dive into **environment variables**, `.bashrc` abuse, and how attackers persist through shell manipulation.



