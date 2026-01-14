# 🔍 Finding Malicious Indicators on a Compromised Windows Server  
**End-to-End Incident Response & Digital Forensics Case Study**

---

## 📌 Overview

In this lab, I performed a full **incident response and forensic investigation** on a Windows Server that was actively compromised by an attacker. The goal was to identify **indicators of compromise (IoCs)** using host-based analysis, memory forensics, persistence inspection, file system artifacts, and network traffic analysis.

This project mirrors real-world SOC and DFIR workflows and demonstrates how multiple data sources are correlated to confirm malicious activity.

---

## 🧪 Lab Environment & Topology

**Systems Involved**
- **Compromised Windows Server** – Victim system
- **Sniffer Server (Kali Linux)** – Network traffic analysis

Both systems were connected to the same network segment.

### 📸 Lab Topology
![Lab Topology](top.png)

---

## 🛠️ Tools & Technologies Used

- netstat  
- PsList (Sysinternals)  
- Process Explorer (Sysinternals)  
- Autoruns (Sysinternals)  
- DumpIt  
- Volatility Framework 2.5  
- Wireshark  

---

## 🔎 Investigation Walkthrough

---

### 🔹 Step 1: Identifying Active Network Connections

Used `netstat -an` to view all listening ports and active connections on the compromised server.

📸 **Screenshot:**  
![netstat output](edrive.png)

---

### 🔹 Step 2: Filtering for Established Connections

Filtered results to show only established connections.

📸 **Screenshot:**  
![Established connections](established.png)

---

### 🔹 Step 3: Removing IPv6 Link-Local Noise

Filtered out IPv6 link-local traffic (`fe80::`) to reduce noise.

📸 **Screenshot:**  
![IPv6 filtered](findv.png)

---

### 🔹 Step 4: Identifying External Attacker IP

Confirmed the external IP associated with port 1604.

📸 **Screenshot:**  
![Port 1604 IP](1604.png)

---

### 🔹 Step 5: Correlating Network Connections to PIDs

Mapped established connections to process IDs using `netstat -ano`.

📸 **Screenshot:**  
![PID correlation](ano.png)

---

### 🔹 Step 6: Enumerating Running Processes

Used PsList to review active processes on the system.

📸 **Screenshot:**  
![PsList output](pslist.png)

---

### 🔹 Step 7: Validating Suspicious Processes

Used Process Explorer to inspect suspicious processes and verify network connections.

📸 **Screenshots:**  
![Process Explorer](procexp.png)  
![Resolve Address Disabled](resaddress.png)

---

### 🔹 Step 8: Identifying Additional Malicious Processes

Reviewed `winhelper.exe` and `msn.exe` network activity.

📸 **Screenshots:**  
![winhelper.exe connection](winhelper.png)  
![msn.exe connection](msn.png)

---

### 🔹 Step 9: Persistence Mechanism Analysis

Used Autoruns to identify startup persistence.

📸 **Screenshot:**  
![Autoruns output](autoruns.png)

**Persistence Locations Identified**
- HKLM\Software\Microsoft\Windows NT\CurrentVersion\Winlogon\Userinit  
- HKLM\Software\Microsoft\Windows NT\CurrentVersion\Run  
- HKCU\Software\Microsoft\Windows NT\CurrentVersion\Winlogon  
- Administrator Startup Folder  

---

### 🔹 Step 10: Capturing System Memory

Captured volatile memory using DumpIt.

📸 **Screenshot:**  
![DumpIt memory capture](dumpit.png)

---

### 🔹 Step 11: Preparing Memory Image

Renamed memory image for compatibility with forensic tools.

📸 **Screenshot:**  
![Renamed memory image](renameraw.png)

---

### 🔹 Step 12: Memory Analysis with Volatility

Enumerated processes directly from RAM using Volatility.

📸 **Screenshots:**  
![Volatility help](standalone.png)  
![Volatility pslist](win200.png)

---

### 🔹 Step 13: File System Artifact Review

Identified recently created malicious executables on disk.

📸 **Screenshot:**  
![Recent files](recentfiles.png)

---

### 🔹 Step 14: Network Traffic Analysis (Sniffer Server)

Prepared the sniffer server and verified capture files.

📸 **Screenshot:**  
![Kali sniffer](kalisniffer.png)

---

### 🔹 Step 15: Capturing Live Network Traffic

Started Wireshark capture on interface `eth0`.

📸 **Screenshot:**  
![Wireshark eth0](eth0.png)

---

### 🔹 Step 16: Isolating Attacker-to-Victim Traffic

Applied Wireshark filters to isolate malicious traffic.

📸 **Screenshot:**  
![Attacker traffic](attackv.png)

---

### 🔹 Step 17: Negative Findings Validation

Confirmed no traffic during specific capture windows.

📸 **Screenshot:**  
![No traffic](noip.png)

---

### 🔹 Step 18: Confirming Attacker Disconnection

Observed TCP FIN and RST packets confirming attacker disconnect.

📸 **Screenshot:**  
![Attacker disconnected](attackerdis.png)

---

## 📊 Final Indicators of Compromise (IoCs)

### Network
- External IPs:
  - 175.45.176.199
  - 175.45.176.200
- Ports:
  - 1604
  - 22
  - 995
  - 2222

### Processes
- explora.exe  
- winhelper.exe  
- msn.exe  

### Files
- MSN.EXE  
- winhelper.exe  

---

## 🎯 Why This Project Matters

This project demonstrates:
- Real incident response methodology
- Host and memory forensics
- Persistence detection
- Network traffic analysis
- Evidence correlation across multiple sources

This is the type of analysis performed by **SOC Analysts, DFIR Analysts, and Incident Responders** in production environments.

---

## ✅ Key Takeaway

The investigation confirmed a **deep, multi-layered compromise** involving:
- Active attacker communication
- Multiple persistence mechanisms
- Memory-resident malware
- Disk-based artifacts
- Verified attacker disconnection

This project represents a **complete, end-to-end DFIR case study** suitable for a professional cybersecurity portfolio.
