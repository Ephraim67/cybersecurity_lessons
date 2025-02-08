**Real-world troubleshooting examples** for common network protocols:

---

## **🛠 1. HTTP/HTTPS - Web Page Not Loading**
**Problem:** You can’t access a website.  

✅ **Troubleshooting Steps:**  
🔹 **Check if the website is down** → Use `ping example.com` or visit [DownDetector](https://www.downdetector.com/).  
🔹 **Try HTTPS instead of HTTP** → If `http://example.com` doesn’t load, try `https://example.com`.  
🔹 **Check your DNS settings** → Run `nslookup example.com` to see if it resolves.  
🔹 **Disable browser extensions** → Some extensions block HTTP/HTTPS requests.  

---

## **🛠 2. FTP/SFTP - File Transfer Issues**
**Problem:** You can’t upload or download files via FTP/SFTP.  

✅ **Troubleshooting Steps:**  
🔹 **Check FTP Server Connection** → Try `ftp ftp.example.com`.  
🔹 **Verify Username/Password** → Incorrect credentials can cause login failure.  
🔹 **Check Firewall Rules** → Ports **20, 21 (FTP)** or **22 (SFTP)** might be blocked.  
🔹 **Use Passive Mode** → Some firewalls block **Active FTP** connections.  

---

## **🛠 3. DNS - Website Not Resolving**
**Problem:** You type `www.example.com`, but the browser says "Server Not Found."  

✅ **Troubleshooting Steps:**  
🔹 **Try Another DNS Server** → Change to Google’s DNS (`8.8.8.8` and `8.8.4.4`).  
🔹 **Run `nslookup example.com`** → If it fails, the DNS server is likely down.  
🔹 **Flush DNS Cache** → Run `ipconfig /flushdns` (Windows) or `sudo systemd-resolve --flush-caches` (Linux).  
🔹 **Check `/etc/hosts` File** → Ensure the domain isn’t incorrectly mapped.  

---

## **🛠 4. DHCP - No Internet or IP Conflict**
**Problem:** You get "IP address conflict" or "No valid IP assigned."  

✅ **Troubleshooting Steps:**  
🔹 **Check if DHCP is enabled** → Run `ipconfig /all` (Windows) or `ifconfig -a` (Linux/Mac).  
🔹 **Release & Renew IP** → `ipconfig /release` and `ipconfig /renew`.  
🔹 **Restart the Router** → It might be failing to assign IPs.  
🔹 **Set a Static IP** → Try manually assigning an IP if DHCP fails.  

---

## **🛠 5. TCP/UDP - Slow or Unreliable Connections**
**Problem:** You experience **slow loading speeds** or **frequent disconnects.**  

✅ **Troubleshooting Steps:**  
🔹 **Test TCP vs. UDP** → TCP is reliable but slow; UDP is faster but may drop packets.  
🔹 **Run a Speed Test** → Use [Speedtest.net](https://www.speedtest.net/) to check latency.  
🔹 **Check for Packet Loss** → Run `ping -n 50 google.com` and look for lost packets.  
🔹 **Disable QoS (Quality of Service)** → Some routers prioritize certain traffic over others.  

---

## **🛠 6. ICMP - Can’t Ping Other Devices**
**Problem:** You try to ping another device, but it doesn’t respond.  

✅ **Troubleshooting Steps:**  
🔹 **Ensure the Target is Online** → Try pinging `8.8.8.8` (Google DNS).  
🔹 **Disable Windows Firewall** → ICMP might be blocked (`netsh advfirewall set allprofiles state off`).  
🔹 **Check Router Settings** → Some routers disable ICMP by default.  
🔹 **Use `tracert` or `traceroute`** → Find where packets are getting lost.  

---

## **🛠 7. ARP - Network Devices Not Communicating**
**Problem:** Devices on the same network can’t talk to each other.  

✅ **Troubleshooting Steps:**  
🔹 **Check the ARP Table** → Run `arp -a` to see IP-MAC mappings.  
🔹 **Clear ARP Cache** → Run `arp -d *` to remove incorrect entries.  
🔹 **Manually Add ARP Entry** → `arp -s <IP> <MAC>` if a device isn’t found.  
🔹 **Check for Duplicate IPs** → Two devices with the same IP will cause conflicts.  

---

## **🛠 8. SNMP - Can't Monitor Network Devices**
**Problem:** Your monitoring system can’t collect data from network devices.  

✅ **Troubleshooting Steps:**  
🔹 **Ensure SNMP is Enabled** → Check router/switch settings.  
🔹 **Use the Right Community String** → SNMP v1/v2 uses “public” or custom strings.  
🔹 **Check Firewall Rules** → SNMP uses ports **161 and 162**.  
🔹 **Run an SNMP Test** → Use `snmpwalk -v2c -c public <device-IP>`.  

---

## **🛠 9. SMTP/POP3/IMAP - Email Not Sending/Receiving**
**Problem:** You can't send or receive emails.  

✅ **Troubleshooting Steps:**  
🔹 **Check SMTP Server Connection** → Run `telnet smtp.example.com 25`.  
🔹 **Verify Email Client Settings** → Ensure correct ports:  
  - SMTP → **25, 465 (secure), 587**  
  - POP3 → **110, 995 (secure)**  
  - IMAP → **143, 993 (secure)**  
🔹 **Check Email Queue** → Delayed messages might be stuck in the queue.  
🔹 **Enable Authentication** → SMTP servers often require username/password.  

---

## **🛠 10. VoIP (SIP/UDP) - Calls Dropping**
**Problem:** You experience **choppy audio or dropped calls.**  

✅ **Troubleshooting Steps:**  
🔹 **Check Internet Speed** → VoIP requires **at least 100 Kbps per call**.  
🔹 **Use Wired Instead of Wi-Fi** → Wi-Fi can cause jitter and delay.  
🔹 **Prioritize VoIP Traffic** → Enable **QoS (Quality of Service)** in router settings.  
🔹 **Check NAT and Firewall Rules** → SIP (VoIP) uses ports **5060 and 5061**.  

---

### **🔍 Summary of Troubleshooting Tips**
| Protocol | Common Issue | Troubleshooting Steps |
|----------|-------------|-----------------------|
| **HTTP/HTTPS** | Website not loading | Check DNS, try HTTPS, disable extensions |
| **FTP/SFTP** | Can’t upload/download | Use Passive Mode, check firewall |
| **DNS** | Website not resolving | Change DNS, flush cache, run `nslookup` |
| **DHCP** | No IP assigned | Restart router, renew IP, set static IP |
| **TCP/UDP** | Slow/unstable network | Test packet loss, disable QoS |
| **ICMP** | Can’t ping | Check firewall, use `tracert` |
| **ARP** | Devices can’t communicate | Clear ARP cache, check IP conflicts |
| **SNMP** | Can't monitor devices | Enable SNMP, check firewall (port 161) |
| **SMTP/POP3/IMAP** | Can’t send/receive email | Verify ports, enable authentication |
| **VoIP (SIP/UDP)** | Choppy/dropped calls | Use wired connection, enable QoS |
