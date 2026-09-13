Windows Persistence Investigation 

1. Project Overview

This project simulates a basic Security Operations Center (SOC) investigation of Windows persistence mechanisms.

The objective was to identify programs configured to automatically execute when a user logs into Windows, investigate their file locations, and validate executable digital signatures.



2. Investigation Scenario

A Windows workstation was selected for investigation to determine whether suspicious persistence mechanisms were configured to automatically execute programs during user login.

As a SOC analyst, I investigated Windows Registry Run Keys and startup programs for potentially suspicious entries.


3. Investigation Objectives

* Identify programs configured to start automatically.
* Examine Windows Registry Run Keys.
* Investigate executable file paths.
* Validate digital signatures.
* Identify potentially suspicious persistence mechanisms.
* Document evidence and findings.
* Determine whether suspicious persistence was identified.


4. Tools Used

* Windows
* PowerShell
* Windows Registry
* Get-CimInstance
* Get-Item
* Get-AuthenticodeSignature
* GitHub


5. Investigation

5.1 Enumerating Windows Startup Programs

The first step was to identify programs configured to start automatically.

Command Used

Get-CimInstance Win32_StartupCommand | Select-Object Name, Command, Location, User

What the command does

This command retrieves Windows startup programs and displays:

* Program name
* Command used to launch the program
* Startup location
* Associated user

Evidence

<img width="960" height="510" alt="power shell" src="https://github.com/user-attachments/assets/ba1d6bca-579d-458a-b012-7138d41ff315" />



Observation

Several startup entries were identified, including Microsoft OneDrive, Microsoft Edge Update, Canva, Adobe Acrobat Synchronizer, and Microsoft Edge.

The entries were reviewed for suspicious paths, filenames, and execution commands.

⸻

5.2 Investigating Microsoft OneDrive

The OneDrive startup entry was identified as:

C:\Users\User\AppData\Local\Microsoft\OneDrive\OneDrive.exe /background

The file was examined to determine its location, size, creation time, and last modification time.

Command Used

Get-Item "C:\Users\User\AppData\Local\Microsoft\OneDrive\OneDrive.exe" | Select-Object FullName, Length, CreationTime, LastWriteTime

What the command does

Get-Item retrieves information about a specific file.

The command collected:

* Full file path
* File size
* Creation time
* Last write time

Evidence

Screenshot: 02-onedrive-file-details.png

<img width="960" height="510" alt="ps2" src="https://github.com/user-attachments/assets/bd3bfa66-2409-4203-b094-df2b994b1cd0" />


Digital Signature Validation

The executable’s digital signature was then checked.
<img width="960" height="510" alt="power shell1" src="https://github.com/user-attachments/assets/d9783007-d13a-41ac-a5b8-68503691b07f" />


Command Used

Get-AuthenticodeSignature "C:\Users\User\AppData\Local\Microsoft\OneDrive\OneDrive.exe"

Result

Status: Valid

Evidence

Screenshot: 03-onedrive-signature.png

<img width="960" height="510" alt="power shell 2" src="https://github.com/user-attachments/assets/dc3defa3-24a5-43b2-bc1c-d609fc1f0753" />

Analyst Assessment

The executable was located in the expected Microsoft OneDrive directory and returned a valid digital signature.

Based on the evidence collected, the OneDrive startup entry appears consistent with legitimate Microsoft software.

⸻

5.3 Investigating Microsoft Edge Update

The Microsoft Edge Update startup entry was identified as:

C:\Users\User\AppData\Local\Microsoft\EdgeUpdate\1.3.265.7\MicrosoftEdgeUpdateCore.exe

Command Used

Get-Item "C:\Users\User\AppData\Local\Microsoft\EdgeUpdate\1.3.265.7\MicrosoftEdgeUpdateCore.exe" | Select-Object FullName, Length, CreationTime, LastWriteTime

What the command does

This command retrieves basic metadata about the Microsoft Edge Update executable, including its path, file size, creation time, and last modification time.

Evidence

<img width="960" height="510" alt="power shell1" src="https://github.com/user-attachments/assets/7beb6e6e-69ec-4c02-be8d-bf87cd293e2c" />


Insert screenshot here.

Digital Signature Validation

Command Used

Get-AuthenticodeSignature "C:\Users\User\AppData\Local\Microsoft\EdgeUpdate\1.3.265.7\MicrosoftEdgeUpdateCore.exe"

Result

Status: Valid

Evidence

<img width="960" height="510" alt="ps2" src="https://github.com/user-attachments/assets/f76a9018-1d3c-4f0e-ab7c-02949aa0de60" />


Insert screenshot here showing Status : Valid.

Analyst Assessment

The executable was located within the Microsoft Edge Update directory and returned a valid digital signature.

Based on the evidence collected, the entry appears legitimate.

⸻

5.4 Investigating Canva

The Canva startup entry was identified as:

C:\Users\User\AppData\Local\Programs\Canva\Canva.exe

Command Used

Get-Item "C:\Users\User\AppData\Local\Programs\Canva\Canva.exe" | Select-Object FullName, Length, CreationTime, LastWriteTime

What the command does

The command retrieves metadata from the Canva executable, including its full path, file size, creation time, and last modification time.

Digital Signature Validation

Command Used

Get-AuthenticodeSignature "C:\Users\User\AppData\Local\Programs\Canva\Canva.exe"

Result

Status: Valid

Insert screenshot here showing Status : Valid.

Analyst Assessment

The Canva executable was located in its expected application directory and returned a valid digital signature.

Based on the evidence collected, the entry appears legitimate.

⸻

6. Evidence Summary

Entry	Investigation	Result
OneDrive	File path + digital signature	🟢 Valid
Microsoft Edge Update	File path + digital signature	🟢 Valid
Canva	File path + digital signature	🟢 Valid

⸻

7. Findings

Three startup entries were investigated in detail: Microsoft OneDrive, Microsoft Edge Update, and Canva.

The investigated executables were located in expected application directories and returned a digital signature status of Valid.

No suspicious persistence was identified among the entries examined.

⸻

8. Indicators of Compromise (IOCs)

No confirmed Indicators of Compromise (IOCs) were identified during the investigation.

⸻

9. MITRE ATT&CK Mapping

The investigation focused on Windows startup persistence mechanisms.

The relevant MITRE ATT&CK technique is:

T1547 — Boot or Logon Autostart Execution

The investigation examined startup entries that can cause applications to execute automatically when a user logs on.

⸻

10. Remediation Recommendations

If a suspicious startup entry is discovered during a similar investigation:

* Verify the executable’s digital signature.
* Investigate the file path and publisher.
* Calculate and investigate the file hash.
* Review associated processes and network activity.
* Disable or remove confirmed malicious persistence.
* Preserve evidence before remediation where appropriate.
* Monitor the system for further suspicious activity.

⸻

11. Conclusion

A Windows persistence investigation was performed using PowerShell.

Windows startup programs were enumerated and selected entries were investigated by examining their file paths, metadata, and digital signatures.

The investigated OneDrive, Microsoft Edge Update, and Canva executables returned a digital signature status of Valid and appeared consistent with legitimate software.

No confirmed malicious persistence was identified among the entries examined.

This project demonstrates basic Windows persistence investigation, PowerShell usage, digital-signature validation, evidence collection, and SOC documentation.

⸻

12. Skills Demonstrated

* Windows Security Investigation
* PowerShell
* Windows Registry Run Keys
* Startup Program Analysis
* File Metadata Analysis
* Digital Signature Validation
* Evidence Collection
* SOC Investigation Methodology
* MITRE ATT&CK Mapping
* GitHub Documentation
