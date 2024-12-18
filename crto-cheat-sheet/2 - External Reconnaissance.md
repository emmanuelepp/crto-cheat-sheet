### 🔍 **External Reconnaissance**

If you need initial access to a target network, reconnaissance is essential for gathering useful information for exploitation or data access.

### **Types of Reconnaissance:**

1. **Organizational:**
   - Information about employees, structure, locations, and business relationships.
2. **Technical:**
   - Systems such as websites, mail servers, remote access solutions, and defenses (firewalls, antivirus, etc.).

### **Methods:**

- **Passive:** Use of external sources such as Google, LinkedIn, or Shodan without directly interacting with the target network.
- **Active:** Involves direct interaction, such as visiting websites or port scanning, which is riskier as it may alert the target.

**Note:** During active reconnaissance, use a proxy or VPN to protect your public IP.

---

### **DNS Records**

### Using DNS Records for Reconnaissance

1. **A Record Search:**

   Run a `dig` command to identify IP addresses associated with the domain:

   ```bash
   dig cyberbotic.io
   ```

   - Example: `172.67.205.143` and `104.21.90.222` resolve to Cloudflare.

2. **Identify IP Ownership:**

   Use `whois` to determine if the IPs belong to third parties or the target (e.g., Cloudflare, AWS, Azure, ISPs):

   ```bash
   whois 172.67.205.143
   ```

3. **Confirm Third-Party Services:**

   - Many organizations use SaaS providers like Office 365 (`autodiscover.target-domain`) for email or storage. This may allow access without compromising internal networks.

4. **Subdomain Discovery:**

   Tools like `dnscan` can reveal additional services:

   ```bash
   ./dnscan.py -d cyberbotic.io -w subdomains-100.txt
   ```

   - Example: Detected subdomains like `www` and `mail` can provide entry points.

5. **Email Security:**

   Verify SPF, DMARC, and DKIM to assess email spoofing possibilities using tools like `Spoofy`:

   ```bash
   python3 spoofy.py -d cyberbotic.io -o stdout
   ```

6. **Important Note:**
   - Confirm with the client the rules for testing providers like AWS or Azure before conducting tests.
   - Note that IPs like `10.10.15.100` may correspond to internal addresses useful for identifying specific infrastructure.

---

### **Google Dorks**

[Google Hacking Database (GHDB) - Google Dorks, OSINT, Recon](https://www.exploit-db.com/google-hacking-database)

### **Key Operators**

1. **`site:`**

   Search results from a specific domain:

   ```bash
   site:apple.com
   ```

   - Example: All indexed pages from `apple.com`.

2. **`intitle:`**

   Search for pages with specific words in the title:

   ```bash
   intitle:apple
   ```

   - Example: Pages with "apple" in the title.

3. **`inurl:`**

   Find pages with specific words in the URL:

   ```bash
   inurl:login
   ```

   - Example: URLs containing "login," useful for identifying access portals.

4. **`intext:`**

   Search for specific words in the page content:

   ```bash
   intext:password
   ```

   - Example: Pages containing "password" in the body.

5. **`filetype:`**

   Find files of specific types:

   ```bash
   site:example.com filetype:pdf
   ```

   - Example: All PDFs on `example.com`. Combine it with other operators:

   ```bash
   site:example.com filetype:xlsx intitle:budget
   ```

   - Finds spreadsheets related to budgets.

6. **`#..#` (Number Range):**

   Filter results within a number range:

   ```bash
   site:example.com 2020..2022
   ```

   - Example: Results containing numbers between 2020 and 2022.

7. **`-` (Exclusion):**

   Exclude terms or subdomains:

   ```bash
   site:apple.com -www -support
   ```

   - Example: Results from `apple.com` excluding the `www` and `support` subdomains.

---

### **Sensitive Files**

1. **Exposed Credentials:**

   ```bash
   filetype:txt intext:"username" intext:"password"
   ```

2. **Configuration Directories:**

   Find common configuration files:

   ```bash
   inurl:config filetype:xml OR filetype:ini
   ```

3. **Exposed Backups:**

   Search for backup files:

   ```bash
   inurl:backup OR inurl:bak filetype:zip OR filetype:tar
   ```

4. **Exposed Cameras:**

   Identify accessible IP cameras:

   ```bash
   intitle:"WebcamXP" OR intitle:"Live View / - AXIS"
   ```

5. **Admin Panels:**

   Find administration portals:

   ```bash
   inurl:admin OR inurl:login OR inurl:dashboard
   ```

6. **Server Errors:**

   Search for pages with specific errors:

   ```bash
   "Index of /" OR "Error 403" OR "Error 404"
   ```

---
