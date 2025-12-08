# Day 28 — Linux Network Sniffing & Packet Capture for Hackers
<img width="1200" height="400" alt="image" src="https://github.com/user-attachments/assets/ef69a3d6-dfd2-4ead-b57a-a92e1ca96eca" />

Network traffic analysis is one of the most critical skills for hackers, defenders, and red‑team operators.  
Today we cover **tcpdump**, **Tshark**, live packet analysis, filtering, and real‑world exploitation techniques.

Network sniffing and packet capture involve intercepting network traffic and analyzing the data packets that traverse a network. While it is an essential technique for ethical hackers to secure networks, malicious actors use it to steal sensitive information such as passwords and credentials, particularly over unsecured public Wi-Fi networks.

### How Sniffing Works    
Attackers often employ techniques like ARP poisoning to redirect network traffic through their devices, allowing them to capture packets not originally intended for them. Unencrypted data within these packets (like unencrypted login credentials) can be read in plain text.

---

## 1. Interface Enumeration
```
ip a
tcpdump -D
```
Common interfaces:
- eth0 → server NIC  
- wlan0 → wireless  
- any → capture from all interfaces  

---

## 2. Basic tcpdump Usage

Capture all packets:
```
tcpdump -i eth0
```

Write packets to file:
```
tcpdump -i eth0 -w dump.pcap
```

Limit packet count:
```
tcpdump -i eth0 -c 100
```

---

## 3. Filtering Packets Like a Pro

Only TCP:
```
tcpdump -i eth0 tcp
```

Only port 22:
```
tcpdump -i eth0 port 22
```

Only HTTP:
```
tcpdump -i eth0 'tcp port 80'
```

Target a specific host:
```
tcpdump host 10.10.10.50
```

---

## 4. Extracting Credentials, Tokens & Secrets

Cleartext packet extraction:
```
tcpdump -A -i eth0 'tcp port 80'
```

DNS query capture:
```
tcpdump -i any port 53
```

Useful for:
- API key leaks  
- IoT device passwords  
- Misconfigured internal services  

---

## 5. Tshark Deep Packet Analysis

List supported protocols:
```
tshark -G protocols
```

Follow TCP streams:
```
tshark -r dump.pcap -z follow,tcp,ascii,0
```

Filter HTTP requests:
```
tshark -r dump.pcap -Y "http.request"
```

Export extracted objects:
```
tshark -r dump.pcap --export-objects http,./extracted
```

---

## 6. Real-Time Monitoring

Live DNS monitor:
```
tcpdump -i any -n port 53
```

Detect SSH brute‑force:
```
tcpdump -i any 'tcp port 22 and (tcp[tcpflags] & tcp-syn != 0)'
```

Detect beaconing:
```
tcpdump -i any udp and port 53
```

---

## 7. Detect Suspicious Activity

Outbound SYN packets to unknown networks:
```
tcpdump -i any 'tcp[tcpflags] & tcp-syn != 0 and not net 10.0.0.0/8'
```

Large packet exfiltration:
```
tcpdump -i eth0 'tcp and (len > 1000)'
```

---

## 8. Hacker Workflow Example

```
# 1. Capture traffic
tcpdump -i eth0 -w full.pcap

# 2. Extract HTTP requests
tshark -r full.pcap -Y "http.request"

# 3. Export files
tshark -r full.pcap --export-objects http,./loot
```

---

## 9. Operator Tips

- Always store your captures; they are gold later.  
- Filters keep captures small and efficient.  
- Tshark is perfect for headless servers.  
- DNS, HTTP, SSH = highest signal sources.  
- Monitor live during privilege escalation attempts.


If you enjoy this project and want daily hacking Tips, Linux security breakdowns, exploit write-ups, red team techniques, CVE PoCs, and advanced cybersecurity content, follow our community:

🔗 Telegram: [@cybersecplayground](https://t.me/cybersecplayground)     
🔗 GitHub: [https://github.com/cybersecplayground](https://github.com/cybersecplayground)     
🔗 YouTube: [CyberSecPlayground — video walkthroughs & tutorials](https://www.youtube.com/@cyberSecPlayground)    
🔗 Medium: [Offensive security articles & deep-dives](https://medium.com/@cybersecplayground)    

Your support helps keep the 30-Day Linux for Hackers Course free and updated.
⭐ Star the repo — contribute — join the channel — level up.

