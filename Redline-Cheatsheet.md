# Redline & IOC Search Collector Cheatsheet

---

## Overview

**Redline** is a powerful incident response tool by FireEye that collects and analyzes forensic data from Windows systems. It helps identify compromises by analyzing processes, files, registry, event logs, and more.

**IOC (Indicator of Compromise)** files are collections of forensic artifacts like file hashes, file strings, registry keys, IP addresses, and domains that help detect malicious activity.

---

## Redline Main Tabs

| Tab Name       | Description                                                     |
|----------------|-----------------------------------------------------------------|
| **Dashboard**  | Summary view showing analysis progress, findings, and alerts.  |
| **Collectors** | Interface to create and configure data collectors for live or offline systems. |
| **Analysis**   | View and explore imported analysis sessions, including timelines, hits, and detailed forensic data. |
| **IOC Search** | Load and manage IOC files, perform automated searches against collected data to find matches. |
| **Timeline**   | Visualize events chronologically, filter by keyword, date, or event type to understand attack progression. |
| **Process**    | List running or collected processes with details like handles, loaded DLLs, and network activity. |
| **File System**| Explore files and directories captured during data collection. |
| **Registry**   | View registry keys and values collected, useful to detect persistence or configuration changes. |
| **Event Logs** | Access collected Windows event logs including Security, System, and Application logs. |
| **Scheduled Tasks** | Review tasks scheduled on the system, often abused by attackers for persistence. |

---

## Redline Key Features

- **Data Collection Sources:**
  - Running processes and their handles
  - Memory sections and loaded DLLs
  - File system artifacts (files, directories)
  - Windows Registry entries
  - Event Logs (system, security, application)
  - Scheduled tasks and services
  - Network connections and ports
  - Timeline for event chronology

- **Handles:**  
  References to OS objects like files or registry keys by processes; useful to identify malicious process interactions.

- **Unsigned DLL Detection:**  
  Unsigned DLLs loaded in memory can indicate malware presence.

- **Scheduled Tasks:**  
  Common persistence mechanism abused by attackers; check for suspicious or unknown tasks.

- **Event Logs Analysis:**  
  Look for unusual Event IDs, error messages, or attacker-generated events.

- **Timeline:**  
  Useful for reconstructing the attack timeline by filtering keywords, event types, and timestamps.

---

## IOC Editor & IOC Search Collector

- **IOC Files (.ioc):**  
  XML-based files containing structured IOCs such as:
  - File hashes (MD5, SHA1, SHA256)
  - File names, sizes, and strings
  - Registry keys and values
  - Domains and IP addresses

- **IOC Editor (FireEye):**  
  Tool to create and edit IOC files.  
  Editable fields include Name, Author, Description, and Content (IOC values).  
  GUID, creation and modification timestamps are auto-generated.

- **IOC Search Collector in Redline:**  
  Allows importing IOC files for automated scanning during data collection.  
  Matches artifacts found on the target system against IOC entries to identify malicious indicators.

---

## Common Terms & Indicators

| Term                     | Description                                            |
|--------------------------|--------------------------------------------------------|
| **IOC (Indicator of Compromise)** | Artifacts signaling potential security breaches.        |
| **Handles**              | Process references to files, registry keys, or ports. |
| **Unsigned DLL**         | DLL files without a digital signature; suspect malware.|
| **Scheduled Task**       | Windows tasks scheduled to run automatically; often abused by attackers. |
| **Event Logs**           | Logs containing system, security, and application events. |
| **File Strings**         | Text snippets extracted from files, useful as IOCs.    |
| **File Hash (SHA-256, MD5)** | Unique cryptographic identifier of a file’s content.  |
| **Masquerading**         | Using legitimate file names to hide malware.           |
| **Subsystem**            | Execution environment of a process/file (e.g., Windows GUI, Console). |

---

## Best Practices

- Always collect sufficient and relevant data to improve analysis accuracy.
- Use granular and specific IOCs to reduce false positives.
- Review timeline and event logs to understand attacker behavior and sequence.
- Combine IOC Search Collector with manual investigation for thorough hunting.
- Keep IOC files updated with latest threat intelligence.

---

## Useful Resources

- [Redline User Guide](https://fireeye.market/assets/apps/211364/documents/877936_en.pdf)  
- [IOC Editor User Guide](https://fireeye.market/assets/apps/S7cWpi9W//9cb9857f/ug-ioc-editor.pdf)  

---

*Cheatsheet compiled to assist incident responders and threat hunters using FireEye Redline and IOC Editor tools.*
