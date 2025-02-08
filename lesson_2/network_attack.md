### **Hands-on Scenario: Investigating a Suspicious Network Attack**  

In this scenario, you are a security analyst investigating a suspected DDoS attack against a web server. Your goal is to:  
1. **Capture traffic** from the attack.  
2. **Analyze packet headers** for suspicious patterns.  
3. **Detect anomalies** like IP spoofing or port scans.  
4. **Trace the attack source.**  

---

## **Step 1: Capture Network Traffic**  
You will use `tcpdump` to capture packets.

### **A. Start Capturing Traffic**  
Run this command on the server being attacked:  
```bash
sudo tcpdump -i eth0 -w attack_traffic.pcap
```
This captures all packets and saves them to a `.pcap` file for analysis.

### **B. Capture Only HTTP Traffic (Port 80)**
```bash
sudo tcpdump -i eth0 port 80 -w http_attack.pcap
```
This is useful if the attack is targeting a web server.

### **C. Capture SYN Flood Attack**
If you suspect a SYN flood (common in DDoS), capture only TCP SYN packets:
```bash
sudo tcpdump -i eth0 "tcp[tcpflags] & tcp-syn != 0" -w syn_flood.pcap
```

---

## **Step 2: Analyze Packets in Wireshark**  
1. Open Wireshark and load the `.pcap` file.  
2. Apply filters to look for suspicious activity:  
   - **Find the top attacking IPs:**  
     ```
     Statistics → Conversations → IPv4
     ```
   - **Filter for a specific IP:**  
     ```
     ip.src == 192.168.1.10
     ```
   - **Check for abnormal TTL values:**  
     ```
     ip.ttl < 30
     ```
   - **Find high packet rates from a single IP (DDoS sign):**  
     ```
     ip.src == 203.0.113.45
     ```

---

## **Step 3: Extract IP Header Information Using Scapy**  
Now, analyze the `.pcap` file using Python and Scapy.

### **A. Read the Packet Capture**
```python
from scapy.all import *

packets = rdpcap("attack_traffic.pcap")

for pkt in packets:
    if IP in pkt:
        print(f"Source IP: {pkt[IP].src}")
        print(f"Destination IP: {pkt[IP].dst}")
        print(f"TTL: {pkt[IP].ttl}")
        print(f"Protocol: {pkt[IP].proto}")
        print("-" * 50)
```

### **B. Detect IP Spoofing**
Check if the source IP is private or suspicious:
```python
private_ips = ["10.", "192.168.", "172.16.", "127."]
for pkt in packets:
    if IP in pkt and any(pkt[IP].src.startswith(p) for p in private_ips):
        print(f"Possible Spoofed IP Detected: {pkt[IP].src}")
```

---

## **Step 4: Identify a DDoS Attack Pattern**
A **DDoS attack** usually has:  
- A high number of packets from the same IP.  
- A low TTL (botnets use low TTL).  
- Many SYN packets without completing a handshake.  

### **A. Find Top Attacking IPs**
```python
from collections import Counter

ip_counter = Counter(pkt[IP].src for pkt in packets if IP in pkt)
top_attackers = ip_counter.most_common(10)

print("Top 10 Attacking IPs:")
for ip, count in top_attackers:
    print(f"{ip}: {count} packets")
```

### **B. Detect SYN Flood Attack**
```python
syn_count = Counter(pkt[IP].src for pkt in packets if TCP in pkt and pkt[TCP].flags == 'S')
top_syn_attackers = syn_count.most_common(5)

print("Top 5 SYN Flood Attackers:")
for ip, count in top_syn_attackers:
    print(f"{ip}: {count} SYN packets")
```

---

## **Step 5: Tracing the Attack Source**  
### **A. Use WHOIS Lookup**
Find ownership details of an attacking IP:  
```bash
whois 203.0.113.45
```

### **B. Perform a Traceroute to the IP**
See the network path to the attacker:  
```bash
traceroute 203.0.113.45
```

### **C. Check GeoIP Location**
If you have the MaxMind GeoIP database installed:
```python
import geoip2.database

reader = geoip2.database.Reader("GeoLite2-City.mmdb")
response = reader.city("203.0.113.45")

print(f"City: {response.city.name}, Country: {response.country.name}")
```

---

## **Step 6: Mitigation Steps**
### **A. Block Attacker’s IP**
Use firewall rules to block identified attack sources:
```bash
sudo iptables -A INPUT -s 203.0.113.45 -j DROP
```

### **B. Enable SYN Flood Protection**
```bash
echo 1 > /proc/sys/net/ipv4/tcp_syncookies
```

### **C. Set Rate Limiting**
Limit requests per second:
```bash
sudo iptables -A INPUT -p tcp --dport 80 -m limit --limit 10/second -j ACCEPT
```

---

## **Final Analysis**
Once the attack is mitigated:  
1. **Report suspicious IPs** to abuse databases.  
2. **Check logs** for further patterns.  
3. **Improve firewall and IDS rules** for better protection. 
