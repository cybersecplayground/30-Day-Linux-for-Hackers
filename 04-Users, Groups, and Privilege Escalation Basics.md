# Day 4 – Users, Groups, and Privilege Escalation Basics
<img width="1200" height="400" alt="image" src="https://github.com/user-attachments/assets/889f5ffe-e362-4c06-a357-c83ab0bb131b" />

Understanding how users and groups function in Linux is **crucial** for both defending and attacking systems. Every privilege escalation scenario starts with knowing **who you are** and **what you can access**.

---

## 👤 User Overview

- Each user has a **UID** (user ID).  
    - `root` = UID 0  
    - Normal users = UID 1000+  
- Defined in `/etc/passwd`
- Home directories usually under `/home/username`

Check your user:
```bash
whoami
id
```
### 👥 Group Overview
Groups define permissions for collections of users

`Primary group` = assigned when user is created     
`Supplementary groups` = additional privileges

See your groups:   
```
groups
```

### 🔍 Key Security-Related Groups
| Group    | Why It Matters                          |
| -------- | --------------------------------------- |
| `sudo`   | Can execute root commands               |
| `docker` | Possible container breakout             |
| `adm`    | Access to logs (can contain creds)      |
| `lxd`    | Launch root containers with images      |
| `shadow` | Read password hashes (if misconfigured) |


#### 🧪 Task of the Day
Run:
```
whoami
id
groups
sudo -l
```
Review your group memberships

Check what sudo commands (if any) you can run without a password

⚠️ Real-World Attack Scenarios
User is in sudo group → escalate via sudo su

`Docker group` → create container with mounted host → root shell

Sudo misconfig (NOPASSWD):

```
(ALL : ALL) NOPASSWD: ALL
```
You can run anything as root with:

```
sudo /bin/bash
```

🔗 Learn more daily:
📡 https://t.me/cybersecplayground


Be ready for **Day 5** — we’ll cover package managers and how they can be abused or weaponized.

