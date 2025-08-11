# Day 13 – Environment Variables & PATH Hijacking
<img width="1200" height="400" alt="image" src="https://github.com/user-attachments/assets/889f5ffe-e362-4c06-a357-c83ab0bb131b" />

## 📝 Overview
Today we explore **Linux environment variables**, focusing on how they work, how attackers can abuse them, and how to protect against PATH hijacking.

---

## 1️⃣ What Are Environment Variables?
Environment variables store configuration values used by processes and the shell.

Example:
```bash
echo $HOME
echo $USER
echo $PATH
```

- `$HOME` → The current user's home directory.
- `$USER` → The username of the logged-in user.
- `$PATH` → Directories the shell searches for executable files.

---

## 2️⃣ Modifying Environment Variables
You can set or modify environment variables using:
```bash
export VAR=value
export PATH=$PATH:/opt/tools
```

Persistent changes can be made in:
- `~/.bashrc` (user-specific)
- `/etc/profile` (system-wide)

---

## 3️⃣ Security Risks – PATH Hijacking
If an attacker can write to a directory listed **before** system binary paths in `$PATH`, they can replace legitimate commands with malicious ones.

Example attack:
```bash
echo -e '#!/bin/bash\necho hacked' > /tmp/ls
chmod +x /tmp/ls
export PATH=/tmp:$PATH
ls
```
This will execute the fake `ls` instead of `/bin/ls`.

---

## 4️⃣ Defensive Measures
- Keep `$PATH` minimal and ordered with system binaries first (`/usr/bin:/bin`).
- Avoid writable directories in `$PATH`.
- Use absolute paths for critical scripts.

---

## 5️⃣ Pentester Tip
When testing, check for:
```bash
echo $PATH
```
If you find writable directories in `$PATH`, it could lead to command execution.

---
## Tags
`#linux` `#hacking` `#pentesting` `#infosec`

📢 Follow **CyberSecPlayground** on Telegram for daily Linux hacking lessons, pentesting tips, and real-world security tricks!
