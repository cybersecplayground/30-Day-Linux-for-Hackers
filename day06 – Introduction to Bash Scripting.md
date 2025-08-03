# Day 6 – Introduction to Bash Scripting
<img width="1200" height="400" alt="image" src="https://github.com/user-attachments/assets/889f5ffe-e362-4c06-a357-c83ab0bb131b" />

Bash scripting is a critical skill in offensive security. Whether you're automating recon, building post-exploitation payloads, or chaining tools together — Bash is your friend.

---

## 🧠 Why Bash?

- Available on almost every Linux system  
- Lightweight and fast  
- Great for quick automations and on-the-fly modifications  
- Can run commands silently, log output, and chain complex attacks

---

## 🔧 Simple Bash Script Example

```bash
#!/bin/bash

echo "[+] Starting Recon..."
whoami
ip a
```
Save as recon.sh, then:
```
chmod +x recon.sh
./recon.sh
```

## 💡 Key Bash Concepts

| Concept       | Description                                  |
| ------------- | -------------------------------------------- | 
| `#!/bin/bash` | Shebang: tells Linux how to execute the file |
| `echo`        | Prints output                                |
| `$(command)`  | Executes a shell command                     |
| `$1`, `$2`    | Access arguments passed to the script        |
| `>` / `>>`    | Redirect output to file (overwrite / append) |
| `\|` | pipe output to another command |

## 🧪 Task of the Day
Create a file scan.sh with:
```
#!/bin/bash
echo "[*] User Info:"
whoami

echo "[*] System IP:"
ip a | grep inet

echo "[*] Listening Ports:"
ss -tuln
```
Execute it:
```
chmod +x scan.sh
./scan.sh
```
Modify it: Add `hostname`, `uptime`, or any info you want to auto-collect

## 🚩 Real-World Usage
After exploitation, you want:

- Quick system inventory   
- Logged output of recon  
- A quiet way to run tools in sequence  

Instead of typing all that manually, you just drop a `loot.sh`.

### 📡 Get more daily tasks and tools from:
➡️ https://t.me/cybersecplayground


---

Be ready for **Day 7**, where we’ll break down **variables, input arguments**, and how to build **dynamic Bash scripts** used in real ops.

