### **Investigating IP Packet Headers for Forensics and Hacking**  

Analyzing IP packet headers is essential for network forensics, ethical hacking, and cybersecurity investigations. It helps identify attack patterns, trace sources of malicious traffic, and understand protocol behavior.  

---

## **1. Capturing IP Packets**  
To analyze packet headers, first, capture network traffic using tools like **Wireshark**, **tcpdump**, or **Scapy**.  

### **Wireshark Capture**  
Run Wireshark and start capturing packets on a network interface. Use a filter to focus on specific traffic, such as:  
```
ip.addr == 192.168.1.10
tcp.port == 80
```

### **Tcpdump Capture** (Linux/macOS)  
Capture all packets on an interface (e.g., eth0):  
```
sudo tcpdump -i eth0 -nn -vvv
```
Capture packets to a file for analysis:  
```
sudo tcpdump -i eth0 -w capture.pcap
```
Filter for IP packets from a specific source:  
```
sudo tcpdump -i eth0 src 192.168.1.10
```

---

## **2. Understanding the IP Packet Header**  

### **Structure of an IPv4 Header (20 Bytes Minimum)**  

| Field | Size (bits) | Description |
|--------|------------|-------------|
| **Version** | 4 | IPv4 or IPv6 |
| **IHL (Header Length)** | 4 | Length of the IP header |
| **Type of Service (TOS)** | 8 | Priority of the packet |
| **Total Length** | 16 | Full packet size (header + data) |
| **Identification** | 16 | Unique packet ID |
| **Flags** | 3 | Controls fragmentation |
| **Fragment Offset** | 13 | Position of fragment in message |
| **Time to Live (TTL)** | 8 | Packet lifetime |
| **Protocol** | 8 | Transport protocol (e.g., TCP=6, UDP=17, ICMP=1) |
| **Header Checksum** | 16 | Error detection |
| **Source IP Address** | 32 | Sender's IP |
| **Destination IP Address** | 32 | Receiver's IP |
| **Options (if any)** | Variable | Used for security, timestamps, etc. |

---

## **3. Extracting IP Header Fields**
Once packets are captured, extract and analyze the IP header fields.

### **Extracting IP Headers Using Wireshark**  
1. Open **Wireshark** and load a capture file (`.pcap`).
2. Apply an IP filter to narrow results:  
   ```
   ip.src == 192.168.1.10
   ```
   ```
   ip.dst == 8.8.8.8
   ```
3. Expand the **Internet Protocol (IP) section** to view:  
   - Source/Destination IP  
   - TTL  
   - Protocol (TCP/UDP/ICMP)  
   - Flags (Fragmentation)  

---

### **Extracting IP Headers Using Tcpdump**
View packets in real time:  
```
sudo tcpdump -i eth0 -nn -vvv
```
Analyze packets in detail:  
```
sudo tcpdump -r capture.pcap -nn -vvv
```

---

### **Extracting IP Headers Using Scapy (Python)**
Scapy allows forensic analysis and hacking investigations by programmatically extracting packet headers.

Install Scapy if not already installed:  
```
pip install scapy
```

Extract and print IP header details:
```python
from scapy.all import *

# Read a captured packet file
packets = rdpcap("capture.pcap")

for pkt in packets:
    if IP in pkt:
        print(f"Source IP: {pkt[IP].src}")
        print(f"Destination IP: {pkt[IP].dst}")
        print(f"TTL: {pkt[IP].ttl}")
        print(f"Protocol: {pkt[IP].proto}")
        print(f"Header Length: {pkt[IP].ihl * 4} bytes")
        print(f"Identification: {pkt[IP].id}")
        print("-" * 50)
```

---

## **4. Investigating Suspicious or Malicious Traffic**
### **A. Analyzing TTL for Spoofed Packets**
- Normal TTL values:  
  - Windows: **128**  
  - Linux/macOS: **64**  
  - Routers: **255**  
- If TTL is **lower than expected**, it indicates multiple hops or a potential **man-in-the-middle attack**.  
- Example in Wireshark: `ip.ttl < 50`  

---

### **B. Checking Fragmentation Attacks**
Fragmentation attacks occur when malicious users send manipulated fragments to bypass firewalls.  

Detect in Wireshark:  
```
ip.flags == 1
```
Analyze with Scapy:
```python
for pkt in packets:
    if IP in pkt and pkt[IP].flags == "MF":
        print(f"Fragmented Packet from {pkt[IP].src}")
```

---

### **C. Identifying Spoofed IPs**
Attackers often use **IP spoofing** to hide their identity.  
- Check if the source IP is private (RFC 1918):  
  ```
  ip.src == 192.168.0.0/16 or ip.src == 10.0.0.0/8
  ```
- If an external packet has a private IP, it's likely spoofed.  

---

### **D. Detecting DDoS and Scanning Attacks**
- Large numbers of packets from a **single IP** → Possible **DDoS attack**  
  ```
  ip.src == 192.168.1.10 and frame.number > 1000
  ```
- High TCP SYN packets → Possible **Port Scan**  
  ```
  tcp.flags.syn == 1 and tcp.flags.ack == 0
  ```

---

## **5. Extracting Data from Suspicious Packets**
If an attacker sends payloads in packets, extract and analyze the data.

### **Using Scapy to Extract Data**
```python
for pkt in packets:
    if Raw in pkt:
        print(pkt[Raw].load)
```

### **Using Wireshark to Extract Data**
1. Use the **Follow TCP Stream** feature.
2. Look for **HTTP requests, login credentials, or suspicious payloads**.

---

## **6. Tracing IPs and Attackers**
If an attack source is identified, trace it using WHOIS, GeoIP, or traceroute.

### **A. Whois Lookup**
```
whois 8.8.8.8
```
Find out ownership details of an IP.

### **B. Traceroute**
```
traceroute 8.8.8.8
```
Shows the path of packets through the network.

### **C. GeoIP Lookup**
Find the geographical location of an IP:
```python
import geoip2.database

reader = geoip2.database.Reader("GeoLite2-City.mmdb")
response = reader.city("8.8.8.8")

print(response.city.name, response.country.name)
```

---

## **7. Conclusion**
Investigating IP headers is essential for network forensics and ethical hacking.  
- **Capture network traffic** with Wireshark or tcpdump.  
- **Extract header fields** to analyze TTL, source/destination IPs, and flags.  
- **Detect spoofing, fragmentation, and DDoS attacks** using filters and scripts.  
- **Trace attackers** using WHOIS, GeoIP, and traceroute.  
