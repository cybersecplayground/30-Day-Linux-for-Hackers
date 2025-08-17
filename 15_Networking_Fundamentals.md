# Day 15 – Networking Fundamentals for Hackers
<img width="1200" height="400" alt="image" src="https://github.com/user-attachments/assets/889f5ffe-e362-4c06-a357-c83ab0bb131b" />

## Objective  
Understand essential Linux networking commands, how attackers enumerate networks, and common techniques to monitor traffic.

---

## 1️⃣ Key Networking Commands  
- `ifconfig` or `ip a` → Show network interfaces  
- `ping <target>` → Test connectivity  
- `netstat -tulnp` or `ss -tulnp` → Show listening ports  
- `traceroute <target>` → Trace route to a host  
- `dig <domain>` → DNS lookup  
- `curl ifconfig.me` → Reveal your public IP  

---

## 2️⃣ Monitoring Connections  
```bash
ss -tnp             # Show active TCP connections
netstat -antp       # Alternative view
tcpdump -i eth0     # Capture packets on eth0
```
Pentesters use these to spot suspicious connections or sniff credentials.

---

## 3️⃣ Attacker Use Cases  
- Identify open ports for lateral movement  
- Map internal network routes  
- Discover DNS misconfigurations  
- Exfiltrate data over covert connections  

---

## 4️⃣ Task of the Day  
1. List all open ports on your system using `ss -tulnp`.  
2. Use `tcpdump` to capture packets while browsing.  
3. Try `dig` to enumerate DNS records for a domain.  

---

## ⚔️ Pentester Tip  
Networking knowledge is the backbone of hacking. Every exploit travels over the wire — know the wire, control the attack.

---

**Follow CyberSecPlayground** on Telegram for more Linux hacking, networking, and pentesting techniques:  
[https://t.me/cybersecplayground](https://t.me/cybersecplayground)
