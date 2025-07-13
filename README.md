# 🛡️ Kali Linux - Offensive Security Testing Lab

This document presents hands-on offensive testing activities carried out using **Kali Linux** as the attacker machine. The exercises included reconnaissance, vulnerability scanning, and brute-force attacks. All activities were logged and detected using defensive tools like **Suricata**.

---

##    Target Systems

| Hostname | IP Address     | Role               |
|----------|----------------|--------------------|
| DVWA     | 192.168.56.109 | Vulnerable Web App |
| Windows  | 192.168.56.111 | Windows Target     |
| Kali     | 192.168.56.102 | Attacker           |

---

##   Attacks Performed & Alerts Generated

### 1. 🔓 **SMB Brute-Force Attack**
- **Tool Used**: `Hydra v9.5`
- **Target**: `192.168.56.111` (Windows)
- **Protocol**: `SMB (Port 445)`
- **Command**:
  ```bash
  hydra -l admin -P /usr/share/wordlists/rockyou.txt smb://192.168.56.111
  ```

**Suricata Alert (fast.log)**:
- **Signature**: ET SCAN Possible Nmap User-Agent Observed  
- **Alert Priority**: 3  
- **Source IP**: 192.168.56.102  
- **Destination IP**: 192.168.56.111  
- **Classification**: Web Application Attack  
- **Timestamp**: 2025-06-29

---

### 2.   **Web Vulnerability Scan with Nikto**
- **Tool Used**: `Nikto`
- **Target**: `192.168.56.109` (DVWA)
- **Protocol**: HTTP (Port 80)
- **Command**:
  ```bash
  nikto -h http://192.168.56.109
  ```

**Scan Results**:
-  X-Content-Type-Options header missing
-  Multiple instances of directory indexing
-  Insecure HTTP methods allowed (e.g., PUT, DELETE)

---

##   Lessons Learned

- Nikto is useful for **rapid web vulnerability discovery** and identifying unsafe HTTP configurations.
- Hydra brute-force testing showed how to **enumerate weak login credentials**.
- Nmap revealed **real-world attack surfaces** including open services and version enumeration.
- Proper logging, file naming, and documentation **make reviewing attack patterns easier**.

---

##   Security Recommendations

- Disable **unnecessary HTTP methods** (e.g., PUT, DELETE).
- Implement **account lockout mechanisms** to defend against brute-force attacks.
- Ensure services like **SSH and FTP** are secured or disabled if not in use.
- Harden public-facing servers using **least privilege principles**.
- Monitor network traffic and review IDS/IPS alerts like those from **Suricata fast.log**.

---

##   Screenshots Included in This Repo

- `Suricata-Detection-SMB-FastLog.png`
- `Suricata-Detection-ICMP-Sweep.png`
- `Suricata-Detection-Nikto-Vuln-Scan.png`
- `Hydra-BruteForce-Command-Output.png`
- `Nikto-Scan-Result.png`
