### 1. **HTTP/HTTPS - Web Page Not Loading**  
**Commands:**  
- Check if a website is reachable:  
  ```
  curl -I http://example.com
  ```
- Test HTTPS connection:  
  ```
  curl -I https://example.com
  ```
- Check DNS resolution:  
  ```
  nslookup example.com
  ```
- Clear browser cache and disable extensions  

**Tools:**  
- Ping (`ping example.com`)  
- Browser Developer Tools (`F12` → Network Tab)  
- Online site status checkers like DownDetector  

---

### 2. **FTP/SFTP - File Transfer Issues**  
**Commands:**  
- Connect to FTP server:  
  ```
  ftp ftp.example.com
  ```
- Secure FTP connection:  
  ```
  sftp user@example.com
  ```
- Check firewall rules:  
  ```
  sudo iptables -L | grep ftp
  ```
- Switch to passive mode in FTP client settings  

**Tools:**  
- FileZilla  
- WinSCP  
- tcpdump or Wireshark  

---

### 3. **DNS - Website Not Resolving**  
**Commands:**  
- Check DNS resolution:  
  ```
  nslookup example.com
  ```
  ```
  dig example.com
  ```
- Flush DNS cache (Windows):  
  ```
  ipconfig /flushdns
  ```
- Change DNS settings to Google (8.8.8.8) or Cloudflare (1.1.1.1)  

**Tools:**  
- nslookup  
- dig  
- Wireshark  

---

### 4. **DHCP - No Internet or IP Conflict**  
**Commands:**  
- Check current IP configuration:  
  ```
  ipconfig /all
  ```
  ```
  ifconfig -a
  ```
- Release and renew IP (Windows):  
  ```
  ipconfig /release
  ipconfig /renew
  ```
- Restart the DHCP service (Linux):  
  ```
  sudo systemctl restart dhcpd
  ```

**Tools:**  
- DHCP server logs  
- Router admin panel  

---

### 5. **TCP/UDP - Slow or Unreliable Connections**  
**Commands:**  
- Check network latency:  
  ```
  ping -n 50 google.com
  ```
- Test TCP port connectivity:  
  ```
  telnet example.com 80
  ```
- Test UDP connection:  
  ```
  nc -u example.com 53
  ```

**Tools:**  
- Wireshark  
- iPerf  

---

### 6. **ICMP - Can’t Ping Other Devices**  
**Commands:**  
- Check if a device is reachable:  
  ```
  ping 8.8.8.8
  ```
- Trace the network route:  
  ```
  tracert example.com
  ```
  ```
  traceroute example.com
  ```
- Enable ICMP on Windows:  
  ```
  netsh advfirewall firewall add rule name="ICMP Allow" protocol=ICMPv4 dir=in action=allow
  ```

**Tools:**  
- Wireshark  
- PingPlotter  

---

### 7. **ARP - Network Devices Not Communicating**  
**Commands:**  
- View ARP table:  
  ```
  arp -a
  ```
- Clear ARP cache:  
  ```
  arp -d *
  ```
- Manually add an ARP entry:  
  ```
  arp -s 192.168.1.10 00-14-22-01-23-45
  ```

**Tools:**  
- Wireshark  
- ARPWatch  

---

### 8. **SNMP - Can't Monitor Network Devices**  
**Commands:**  
- Test SNMP connection:  
  ```
  snmpwalk -v2c -c public 192.168.1.1
  ```
- Check SNMP status on Linux:  
  ```
  systemctl status snmpd
  ```
- Open SNMP port on firewall:  
  ```
  sudo ufw allow 161/udp
  ```

**Tools:**  
- Zabbix  
- Nagios  

---

### 9. **SMTP/POP3/IMAP - Email Not Sending/Receiving**  
**Commands:**  
- Test SMTP server connectivity:  
  ```
  telnet smtp.example.com 25
  ```
- Check email queue:  
  ```
  mailq
  ```
- Restart mail service (Linux):  
  ```
  sudo systemctl restart postfix
  ```

**Tools:**  
- Mail Server Logs  
- MXToolBox  

---

### 10. **VoIP (SIP/UDP) - Calls Dropping**  
**Commands:**  
- Test SIP connection:  
  ```
  sngrep
  ```
- Check UDP packet loss:  
  ```
  ping -i 0.2 -s 1200 192.168.1.1
  ```
- Enable QoS for VoIP traffic in router settings  

**Tools:**  
- Wireshark  
- VoIP monitor tools like FreePBX  
