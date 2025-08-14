# Purpose
Document the process of setting up a safe, isolated SOC home lab using **Kali Linux** (attacker) and **Windows 10 Pro** (defender) for security testing, SIEM analysis, and malware simulation.

---

## Prerequisites
- Virtualization software (VMware Workstation or VirtualBox) installed.
- **Kali Linux** ISO: [https://www.kali.org/get-kali/](https://www.kali.org/get-kali/)
- **Windows 10 Pro** ISO: [https://www.microsoft.com/software-download/windows10](https://www.microsoft.com/software-download/windows10)

---

## Kali Linux Setup (NAT Phase)
- Ensure required tools are installed:
  ```bash
  sudo apt update && sudo apt install nmap metasploit-framework -y
  ```
## Windows 10 Pro Setup (NAT Phase)
- Install & set up Splunk Enterprise.
- Install the Splunk Add-on for Sysmon.
- Install & set up Sysmon with the OLAF modular configuration.
- Ingest Sysmon logs into Splunk using a modified inputs.conf file:
  - This file contains the configuration to add the Sysmon event log path for direct ingestion.
  - Place the file in:
    ``` C:\Program Files\Splunk\etc\system\local\inputs.conf ```
  - The exact inputs.conf file used in this setup will be included in this repository for reference.

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
