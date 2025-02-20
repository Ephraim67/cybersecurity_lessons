## 🔹 **1️⃣ MySQL Scanning (Database Enumeration)**
### ✅ **Find Open MySQL Ports**
```bash
nmap -p 3306 --open 192.168.1.10
```
🔹 `--open` → Shows only hosts with MySQL running  

### ✅ **Detect MySQL Version**
```bash
nmap -sV -p 3306 192.168.1.10
```
🔹 `-sV` → Identifies MySQL version  

### ✅ **Brute-force MySQL Login**
```bash
nmap --script=mysql-brute -p 3306 192.168.1.10
```
🔹 Uses common usernames/passwords to attempt login  

### ✅ **List MySQL Databases**
```bash
nmap --script=mysql-databases -p 3306 --script-args mysqluser=root,mysqlpass=root 192.168.1.10
```
🔹 Requires valid credentials (`mysqluser`, `mysqlpass`)  

---

## 🔹 **2️⃣ Nginx Scanning (Web Server Recon)**
### ✅ **Find Open HTTP/HTTPS Ports**
```bash
nmap -p 80,443,8080,8443 --open 192.168.1.10
```
🔹 Scans for common web ports  

### ✅ **Get Web Server Info & Headers**
```bash
nmap --script=http-headers,http-title -p 80,443 192.168.1.10
```
🔹 `http-title` → Retrieves website title  
🔹 `http-headers` → Extracts security headers  

### ✅ **Detect Known Nginx Vulnerabilities**
```bash
nmap --script=http-vuln-cve2021-44228,http-vuln-exchange -p 80,443 192.168.1.10
```
🔹 Scans for **Log4j (CVE-2021-44228)** and **Microsoft Exchange bugs**  

### ✅ **Check for Directory Listings**
```bash
nmap --script=http-enum -p 80,443 192.168.1.10
```
🔹 Detects hidden directories and files  

---

## 🔹 **3️⃣ Kubernetes Scanning (Cluster Recon)**
### ✅ **Find Open Kubernetes Ports**
```bash
nmap -p 6443,10250 --open 192.168.1.10
```
🔹 `6443` → Kubernetes API Server  
🔹 `10250` → Kubelet API  

### ✅ **Detect Kubernetes API Version**
```bash
nmap -sV -p 6443 192.168.1.10
```
🔹 Identifies Kubernetes API version  

### ✅ **Check for Anonymous Access to API**
```bash
nmap --script=http-kubernetes-api -p 6443 192.168.1.10
```
🔹 Checks if the API allows **unauthenticated requests**  

### ✅ **Scan Kubelet API for Exploitable Endpoints**
```bash
nmap --script=http-kubernetes-pods -p 10250 192.168.1.10
```
🔹 Lists active **Pods** if Kubelet API is exposed  

---

## 🔹 **4️⃣ JBox Server Scanning**
### ✅ **Find Open JBox Ports**
```bash
nmap -p 5000,8080 --open 192.168.1.10
```
🔹 `5000` → Default JBox admin interface  
🔹 `8080` → Web application port  

### ✅ **Enumerate JBox Web Panel**
```bash
nmap --script=http-title,http-enum -p 5000 192.168.1.10
```
🔹 Detects login panels and exposed endpoints  

---

## 🔹 **5️⃣ Windows Server & RADIUS Scanning**
### ✅ **Find Open Windows Ports**
```bash
nmap -p 135,139,445,3389 --open 192.168.1.10
```
🔹 `135,139,445` → SMB  
🔹 `3389` → Remote Desktop Protocol (RDP)  

### ✅ **Detect Windows Version**
```bash
nmap -O 192.168.1.10
```
🔹 `-O` → OS fingerprinting  

### ✅ **Check SMB for Vulnerabilities**
```bash
nmap --script=smb-vuln* -p 445 192.168.1.10
```
🔹 Checks for **EternalBlue (MS17-010), SMBGhost (CVE-2020-0796)**  

### ✅ **Scan RADIUS Authentication Server**
```bash
nmap -p 1812,1813 --open 192.168.1.10
```
🔹 `1812` → RADIUS authentication  
🔹 `1813` → RADIUS accounting  

### ✅ **Test RADIUS Authentication**
```bash
nmap --script=radius-brute -p 1812 --script-args=userdb=users.txt,passdb=pass.txt 192.168.1.10
```
🔹 Brute-forces **RADIUS login**  

---

## 🔹 **6️⃣ Apache Cassandra Scanning**
### ✅ **Find Open Cassandra Ports**
```bash
nmap -p 9042,9160 --open 192.168.1.10
```
🔹 `9042` → Native transport port (CQL)  
🔹 `9160` → Thrift API  

### ✅ **Detect Cassandra Version**
```bash
nmap -sV -p 9042 192.168.1.10
```
🔹 `-sV` → Version detection  

### ✅ **Check for Anonymous Access**
```bash
nmap --script=cassandra-info -p 9042 192.168.1.10
```
🔹 Detects **open or misconfigured Cassandra instances**  

### ✅ **Attempt CQL Injection**
```bash
nmap --script=cassandra-brute -p 9042 192.168.1.10
```
🔹 Attempts default username/password combos  

---

## 🔹 **7️⃣ Apache & Nginx Exploit-Specific Scans**
### ✅ **Check for WebDAV Misconfigurations**
```bash
nmap --script=http-iis-webdav-vuln -p 80,443 192.168.1.10
```
🔹 Detects **WebDAV** misconfigurations on Apache/Nginx  

### ✅ **Detect Proxy Misconfigurations**
```bash
nmap --script=http-proxy-detect -p 3128,8080 192.168.1.10
```
🔹 Finds **open proxy servers**  

### ✅ **Check for Apache Struts Vulnerabilities**
```bash
nmap --script=http-vuln-cve2017-5638 -p 80,443 192.168.1.10
```
🔹 Scans for **Apache Struts CVE-2017-5638**  

---

## 🔥 **Wrapping It Up**
Here’s a **full network pentesting workflow** using Nmap:

### ✅ **Step 1: Discover Live Hosts**
```bash
nmap -sn 192.168.1.0/24
```
### ✅ **Step 2: Scan for Open Ports**
```bash
nmap -p- -T4 192.168.1.10
```
### ✅ **Step 3: Identify Running Services**
```bash
nmap -sV -O 192.168.1.10
```
### ✅ **Step 4: Target-Specific Scanning**
- **MySQL:** `nmap --script=mysql-brute -p 3306 192.168.1.10`
- **Kubernetes:** `nmap --script=http-kubernetes-api -p 6443 192.168.1.10`
- **Nginx:** `nmap --script=http-title,http-headers -p 80,443 192.168.1.10`
- **Cassandra:** `nmap --script=cassandra-info -p 9042 192.168.1.10`
