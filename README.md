# VirusTotal-vs-Joe-Sandbox-Comparison-Suspicious-Executable-Analysis-

![VirusTotal](https://img.shields.io/badge/-VirusTotal-394EFF?style=for-the-badge&logo=VirusTotal&logoColor=white)   |   ![Joe Sandbox](https://img.shields.io/badge/Joe%20Sandbox-0052CC?style=for-the-badge)

#### 
A direct comparison of results produced by VirusTotal (referenced in the sandbox report at 2% detection) against the full Joe Sandbox behavioral analysis.

| ![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=GitHub&logoColor=white) | Suspicious-Executable-Analysis-Detected-via-Sysmon-Telemetry-VirusTotal-Static-Behavioral-Analysis 

https://github.com/digdeflab-bsd/Suspicious-Executable-Analysis-Detected-via-Sysmon-Telemetry-VirusTotal-Static-Behavioral-Analysis/edit/main/README.md

---

| ![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=GitHub&logoColor=white) | Suspicious-Executable-Analysis-Joe-Sandbox-Wireshark-Traffic-Capture

https://github.com/digdeflab-bsd/Suspicious-Executable-Analysis-Joe-Sandbox-Wireshark-Traffic-Capture/edit/main/README.md

---

### Detection Rates

| Scanner | Detection | Label | Notes |
|---|---|---|---|
| VirusTotal (aggregate) | 2% | No specific label provided | Approximately 1-2 engines out of ~70 flagged the file |
| ReversingLabs (via sandbox) | 0% | Clean | Applied to both the installer and all 87 dropped files |
| Joe Sandbox overall score | 14/100 | CLEAN (detection field) | Behavioral scoring, not signature-based |
| Joe Sandbox confidence | 40% | Suspicious/Malicious in subcategories | Evader and Miner categories rated malicious |

---
### What VirusTotal ![VirusTotal](https://img.shields.io/badge/-VirusTotal-394EFF?style=for-the-badge&logo=VirusTotal&logoColor=white) Provides

VirusTotal performs primarily static analysis: it runs the file against 70+ antivirus engine signatures, checks file hashes against known malware databases, and assesses embedded URLs. At 2% detection, only 1-2 engines flagged ISOMate.exe. This likely represents a heuristic or generic PUA (Potentially Unwanted Application) flag rather than a confirmed malware identification.

**Strengths of VirusTotal for this sample:** Fast, broad signature coverage. Quickly confirms the file is not a known malware variant. URL reputation checks for all embedded strings (all rated safe by Avira URL Cloud).

**Limitations of VirusTotal for this sample:** Static analysis cannot observe the PowerShell USB enumeration behavior. It cannot observe the BitLocker module loading. It cannot analyze the TLS-encrypted traffic payload. It cannot assess the significance of 3,030 Sleep() calls.

---
### What Joe Sandbox  ![Joe Sandbox](https://img.shields.io/badge/Joe%20Sandbox-0052CC?style=for-the-badge) Provides

Joe Sandbox performs full dynamic behavioral analysis in an isolated Windows environment. It intercepts API calls, captures network traffic, monitors file and registry activity, and applies behavioral signatures (Sigma, Suricata, custom rules).

**Strengths of Joe Sandbox for this sample:** Captured the non-interactive PowerShell process spawn. Identified the BitLocker module loading. Recorded all 87 dropped files with hashes. Captured DNS and TCP network traffic. Applied MITRE ATT&CK mapping.

**Limitations of Joe Sandbox for this sample:** Analysis timed out at 7 minutes. Report was truncated due to size limits on NtCreateKey, NtOpenKeyEx, NtProtectVirtualMemory, NtQueryValueKey, and NtSetInformationFile calls. Not all processes were fully analyzed. Confidence in the final score is only 40%.

---
### Summary Comparison

| Dimension | VirusTotal Result | Joe Sandbox Result |
|---|---|---|
| Overall verdict | Nearly clean (2% detection) | Clean score but suspicious behaviors observed |
| Signature-based detection | 1-2 engines flagged (heuristic) | No malware config, no YARA, no Suricata matches |
| Behavioral analysis | Not performed | PowerShell USB enumeration, BitLocker module load |
| Network analysis | URL reputation only (all safe) | Live TLS traffic to 45.32.86.219 confirmed |
| Dropped file analysis | Limited | All 87 files hashed and scanned (all clean) |
| Actionability | Low: nearly clean verdict suggests safe | Medium: behavioral flags require policy decision |
| False negative risk | Higher: cannot detect behavior-only threats | Lower: behavioral engine captures runtime activity |

---
**Conclusion on comparison:** For this specific sample, VirusTotal and Joe Sandbox agree that ISOMate.exe is not a known malware variant. However, Joe Sandbox's behavioral analysis surfaces concerns that VirusTotal's static approach fundamentally cannot detect. The USB enumeration and BitLocker module loading would be completely invisible to VirusTotal, yet they are the most organizationally significant findings. This illustrates a core limitation of signature-based scanning for commercial software with potentially invasive behavior: the file is not malware, but it may not be acceptable in a corporate environment. A defender who relied solely on VirusTotal would not have visibility into these behaviors.
