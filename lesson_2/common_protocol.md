**14 common network protocols** and their functions:

---

## **1️⃣ HTTP (Hypertext Transfer Protocol)**
- **Function:** Transfers web pages from a server to a browser.  
- **Port:** **80**  
- **Example:** When you visit `http://example.com`, your browser uses HTTP to fetch the page.  

---

## **2️⃣ HTTPS (Hypertext Transfer Protocol Secure)**
- **Function:** A secure version of HTTP that encrypts data using SSL/TLS.  
- **Port:** **443**  
- **Example:** When logging into online banking, `https://` ensures your credentials are encrypted.  

---

## **3️⃣ FTP (File Transfer Protocol)**
- **Function:** Transfers files between a client and a server.  
- **Ports:** **20 (Data), 21 (Control)**  
- **Example:** Used to upload files to a web server or download software.  

---

## **4️⃣ SFTP (Secure File Transfer Protocol)**
- **Function:** A secure version of FTP that uses SSH for encryption.  
- **Port:** **22**  
- **Example:** Used for secure file transfers on Linux servers.  

---

## **5️⃣ DNS (Domain Name System)**
- **Function:** Resolves domain names (e.g., `google.com`) to IP addresses.  
- **Port:** **53**  
- **Example:** When you type `www.example.com`, DNS translates it to an IP like `192.168.1.1`.  

---

## **6️⃣ DHCP (Dynamic Host Configuration Protocol)**
- **Function:** Assigns IP addresses automatically to devices on a network.  
- **Ports:** **67 (Server), 68 (Client)**  
- **Example:** Your router assigns your laptop an IP address dynamically.  

---

## **7️⃣ TCP (Transmission Control Protocol)**
- **Function:** Ensures reliable and ordered delivery of data packets.  
- **Port:** **N/A (Used in multiple protocols)**  
- **Example:** Used in HTTP, FTP, and email to prevent data loss.  

---

## **8️⃣ UDP (User Datagram Protocol)**
- **Function:** Provides fast but **unreliable** data transfer (no error checking).  
- **Port:** **N/A (Used in multiple protocols)**  
- **Example:** Used in live streaming, VoIP, and online gaming for low-latency communication.  

---

## **9️⃣ ICMP (Internet Control Message Protocol)**
- **Function:** Sends error messages and operational information.  
- **Port:** **N/A** (Works within IP)  
- **Example:** Used by **ping** and **traceroute** to diagnose network issues.  

---

## **🔟 ARP (Address Resolution Protocol)**
- **Function:** Maps an IP address to a MAC address on a local network.  
- **Port:** **N/A**  
- **Example:** When your computer wants to send data to another device, it finds the MAC address using ARP.  

---

## **1️⃣1️⃣ SNMP (Simple Network Management Protocol)**
- **Function:** Monitors and manages network devices (e.g., routers, switches).  
- **Port:** **161/162**  
- **Example:** Used by IT teams to track device performance.  

---

## **1️⃣2️⃣ SMTP (Simple Mail Transfer Protocol)**
- **Function:** Sends emails between mail servers.  
- **Port:** **25, 465 (Secure), 587**  
- **Example:** Used when you send an email via Outlook or Gmail.  

---

## **1️⃣3️⃣ POP3 (Post Office Protocol v3)**
- **Function:** Downloads emails from a mail server to a local device.  
- **Port:** **110, 995 (Secure)**  
- **Example:** Used to retrieve emails and delete them from the server.  

---

## **1️⃣4️⃣ IMAP (Internet Message Access Protocol)**
- **Function:** Syncs emails across multiple devices while keeping them on the server.  
- **Port:** **143, 993 (Secure)**  
- **Example:** Gmail and Outlook use IMAP to keep emails available on different devices.  

---

### **🛠 Quick Comparison of Email Protocols**
| Protocol | Function | Port |
|----------|---------|------|
| SMTP | Sends emails | 25, 465, 587 |
| POP3 | Downloads emails (removes from server) | 110, 995 |
| IMAP | Syncs emails across devices | 143, 993 |
