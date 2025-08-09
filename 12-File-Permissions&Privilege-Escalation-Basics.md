# Day 12: File Permissions & Privilege Escalation Basics
<img width="1200" height="400" alt="image" src="https://github.com/user-attachments/assets/889f5ffe-e362-4c06-a357-c83ab0bb131b" />

## Objective
Understand Linux file permissions in detail and how attackers (or penetration testers) can exploit them for privilege escalation.

---

## 1️⃣ Understanding File Permissions

In Linux, every file and directory has **three sets of permissions** for three types of users:

- **Owner (u)** – The user who owns the file.
- **Group (g)** – Users in the file's group.
- **Others (o)** – All other users on the system.

### Permission Types
- **r** → read
- **w** → write
- **x** → execute

Example output from `ls -l`:
```bash
-rwxr-x---  1 alice admin 1234 Aug  9 10:00 script.sh
```

Here:
- **Owner (alice)** → `rwx` (read, write, execute)
- **Group (admin)** → `r-x` (read, execute)
- **Others** → `---` (no permissions)

---

## 2️⃣ Changing Permissions

You can change file permissions using the `chmod` command.

```bash
chmod 755 file.txt   # rwxr-xr-x
chmod u+x script.sh  # Add execute for owner
chmod g-w file.txt   # Remove write from group
```

You can also use **octal notation**:
- `7` → `rwx`  
- `6` → `rw-`  
- `5` → `r-x`  
- `4` → `r--`  
- `0` → `---`

Example:
```bash
chmod 644 document.txt  # Owner: rw-, Group: r--, Others: r--
```

---

## 3️⃣ Privilege Escalation via Misconfigured Permissions

### a) World-Writable Files
Files with `chmod 777` can be modified by anyone — a serious security risk.

Find them with:
```bash
find / -type f -perm -0002 2>/dev/null
```

### b) SUID/SGID Bits
These special permission bits allow a file to be executed **with the privileges of the file’s owner or group**.

- **SUID**: Run as file owner
```bash
chmod u+s /path/to/file
```
- **SGID**: Run as file’s group
```bash
chmod g+s /path/to/file
```

List SUID binaries:
```bash
find / -perm -4000 2>/dev/null
```

Common SUID binaries attackers check:
```
/bin/bash
/bin/passwd
/usr/bin/sudo
/usr/bin/find
```

If any of these are misconfigured, they can allow **root shell execution**.

---

## 4️⃣ Pentester Tips
- Check `/etc/passwd` and `/etc/shadow` for incorrect permissions.
- Watch for writable configuration files for services running as root.
- SUID-enabled binaries with unintended features can often be abused.

Example exploit:
```bash
# If /usr/bin/find is SUID root
/usr/bin/find . -exec /bin/sh \; -quit
```

---

## 📢 Follow CyberSecPlayground
Daily Linux hacking lessons, privilege escalation techniques, and red team tips:
[https://t.me/cybersecplayground](https://t.me/cybersecplayground)
