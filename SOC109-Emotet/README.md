# SOC109 - Emotet Malware Detected

## Alert Summary
- **Event ID:** 85
- **Alert Name:** SOC109 - Emotet Malware Detected
- **Event Time:** Mar 22, 2021, 09:06 PM
- **Source Address:** 172.16.17.45
- **Source Hostname:** RichardPRD
- **File Name:** 1word.doc
- **File Hash (MD5):** 349d13ca99ab03869548d75b99e5a1d0
- **File Size:** 188.95 Kb
- **Device Action:** Cleaned

## Tools Used
- LetsDefend SIEM
- VirusTotal

## Investigation
1. Reviewed the alert details - File "1word.doc" was detected on host RichardPRD 
   (172.16.17.45) and flagged under rule SOC109 - Emotet Malware Detected. 
   Device Action showed "Cleaned," meaning the endpoint already quarantined 
   the file before I started investigating.

2. Checked the file hash (349d13ca99ab03869548d75b99e5a1d0) against VirusTotal.
   50/61 vendors flagged it and classified it as trojan.w97m/emotet 
   (threat categories: trojan, downloader, dropper).

3. Searched network logs for host RichardPRD around the alert time. 
   No outbound connection matching the alert timeframe (Mar 22, 2021, 
   09:06 PM) was found

4. Based on the VirusTotal detection's macro analysis also showed signs of obfuscation, 
   suspicious OLE automation calls, error suppression, and auto-execution 
   on document open — all consistent with malicious macro behavior.

5. Based on file reputation on VirusTotal, we concluded that this was 
   a genuine Emotet infection attempt.

## Verdict
**True Positive**

Reasoning: VirusTotal flagged the file with a 50/61 detection ratio and 
macro-level analysis showing obfuscation and auto-execution behavior 
consistent with Emotet. No outbound C2 activity was found in proxy logs 
for the alert timeframe, and Device Action already showed "Cleaned" — 
indicating the threat was real but contained before communication with 
a C2 server.

## MITRE ATT&CK
- **T1566.001 – Phishing: Spearphishing Attachment** (malicious 1word.doc 
  delivered to endpoint)
- **T1204.002 – User Execution: Malicious File** (auto-open macro behavior)
- **T1059 – Command and Scripting Interpreter** (macro-based execution)
- **T1047 – Windows Management Instrumentation** (calls-wmi behavior tag — 
  used for stealthy execution)
  
## Screenshots
<img width="1904" height="543" alt="01-Alert" src="https://github.com/user-attachments/assets/9783acc2-04c6-436b-8e4b-967f918a4f2a" />
<br><br>
<img width="1917" height="743" alt="02-VirusTotal" src="https://github.com/user-attachments/assets/a6ba4383-5bb9-4b49-840f-434bd5bf4d52" />
<br><br>
<img width="1909" height="560" alt="03-Log-Analysis" src="https://github.com/user-attachments/assets/9354bd62-abba-4944-84a8-da0e50acc8dd" />
<br><br>
