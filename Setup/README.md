# Purpose
Document the process of setting up a safe, isolated SOC home lab using **Kali Linux** (attacker) and **Windows 10 Pro** (defender) for security testing, SIEM analysis, and malware simulation.

---

## Prerequisites
- Virtualization software (VMware Workstation or VirtualBox) installed.
- **Kali Linux** ISO: [https://www.kali.org/get-kali/](https://www.kali.org/get-kali/)
- **Windows 10 Pro** ISO: [https://www.microsoft.com/software-download/windows10](https://www.microsoft.com/software-download/windows10)

---

## VM Creation Specs
**Kali Linux**
- 2 CPU cores  
- 2–4 GB RAM  
- 20 GB virtual disk  
- Network Adapter: NAT (initially), then Host-Only/Internal

**Windows 10 Pro**
- 2–4 CPU cores  
- 4–8 GB RAM  
- 40 GB virtual disk  
- Network Adapter: NAT (initially), then Host-Only/Internal

---

## Kali Linux Setup (NAT Phase)
- Ensure required tools are installed:
  ```bash
  sudo apt update && sudo apt install nmap metasploit-framework -y
  ```
## Windows 10 Pro Setup (NAT Phase)
### Splunk Installation
- Download Splunk Enterprise for Windows from: https://www.splunk.com/en_us/download/splunk-enterprise.html
- Run the installer and:
  - Accept license terms.
  - Choose the default installation path.
  - Set a strong admin password.
  - Allow Splunk to start at boot.

### Sysmon Installation
- Download Sysmon from Microsoft Sysinternals: https://learn.microsoft.com/sysinternals/downloads/sysmon
- Download the OLAF modular configuration: https://github.com/olafhartong/sysmon-modular
- Open Command Prompt as Administrator in the Sysmon folder.
- Install Sysmon with the OLAF configuration:
``` sysmon.exe -accepteula -i sysmonconfig.xml ```

### Sysmon Log Ingestion into Splunk
- Use a modified inputs.conf file to automatically ingest Sysmon logs.
- Place the file in:
``` C:\Program Files\Splunk\etc\system\local\inputs.conf ```
- Example inputs.conf content:
```
[WinEventLog://Microsoft-Windows-Sysmon/Operational]
index = sysmon
disabled = 0
renderXml = false
```

---

## Network Configuration (Isolation Phase)
- Configure both VMs to use the same internal network.
- In VMware, this is called a LAN segment.
- Assign static IP addresses:
  - Windows 10 Pro → 192.168.20.10
  - Kali Linux → 192.168.20.11
- Test connectivity:
  - Windows → Kali: Ping should work immediately.
  - Kali → Windows: Ping may be blocked initially by Windows Defender Firewall.

---

## Verification
- Confirm both machines can communicate within the internal network segment.
- From Kali, run:
``` ping 192.168.20.10 ```
- From Windows, run:
``` ping 192.168.20.11 ```

---

## Snapshot Creation
- Once both systems are fully configured and verified, take a snapshot of each VM.
- This snapshot will serve as the clean baseline before performing any malware analysis or attack simulations.
