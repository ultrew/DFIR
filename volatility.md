
# Volatility Memory Forensics Cheatsheet

---

## Overview
Volatility is a free memory forensics tool developed and maintained by Volatility Foundation, widely used by malware and SOC analysts in blue teams for detection and monitoring.  
It is written in Python and consists of python plugins and modules designed to analyze memory dumps in a plug-and-play manner.  
Available on Windows, Linux, and Mac OS.

From the Volatility Foundation Wiki:  
> "Volatility is the world's most widely used framework for extracting digital artifacts from volatile memory (RAM) samples. The extraction techniques are performed completely independent of the system being investigated but offer visibility into the runtime state of the system."

Volatility3 (Python 3) is recommended.  
GitHub repo: https://github.com/volatilityfoundation/volatility3

---

## Installation
- Volatility is pure Python and works on Windows, Linux, and Mac.
- For TryHackMe AttackBox users, Volatility is pre-installed.
- Windows users can download pre-packaged executables (.whl) from releases:  
  https://github.com/volatilityfoundation/volatility3/releases/tag/v1.0.1
- To run from source, install dependencies:
  - Python 3.5.3+
  - pefile 2017.8.1+ (https://pypi.org/project/pefile/)
  - Optional but recommended: yara-python 3.8.0+ (https://github.com/VirusTotal/yara-python), capstone 3.0.0+ (https://www.capstone-engine.org/download.html)
- Clone repo:  
  `git clone https://github.com/volatilityfoundation/volatility3.git`
- Test installation:  
  `python3 vol.py -h`
- For Linux/Mac memory files, download symbol files:  
  https://github.com/volatilityfoundation/volatility3#symbol-tables

---

## Memory Extraction Tools
- Common tools for extracting memory from bare-metal:
  - FTK Imager
  - Redline
  - DumpIt.exe
  - win32dd.exe / win64dd.exe
  - Memoryze
  - FastDump
- Extraction can be time-consuming on bare-metal.
- Most tools output `.raw` files (except Redline which has its own format).
- Virtual machine memory files depend on hypervisor:
  - VMWare: `.vmem`
  - Hyper-V: `.bin`
  - Parallels: `.mem`
  - VirtualBox: `.sav` (partial)

---

## Plugins Overview (Volatility3)
- Volatility3 dropped OS profiles; it auto-detects host OS/build.
- Plugin syntax changed: specify OS before plugin name (e.g., `windows.info`, `linux.info`, `mac.info`).
- Plugin list is shorter than Volatility2 but sufficient.
- Use `-h` or help menu to explore plugins.

---

## Identifying Image Info and Profiles
- Volatility2 requires identifying OS profiles (deprecated in Volatility3).
- Use `imageinfo` plugin in Volatility2 to get possible OS profiles (not always accurate).
- Use OS-specific info plugins in Volatility3:
  ```bash
  python3 vol.py -f <file> windows.info
  ```

---

## Listing Processes and Network Connections

| Plugin     | Description                                                                 | Syntax                                    |
|------------|-----------------------------------------------------------------------------|-------------------------------------------|
| `pslist`   | Lists processes from doubly-linked list (like Task Manager).                | `python3 vol.py -f <file> windows.pslist`|
| `psscan`   | Scans memory for `_EPROCESS` structures, finds hidden/unlinked processes.   | `python3 vol.py -f <file> windows.psscan`|
| `pstree`   | Lists processes hierarchically by parent PID.                              | `python3 vol.py -f <file> windows.pstree`|
| `netstat`  | Lists network connections (may be unstable on old Windows).                 | `python3 vol.py -f <file> windows.netstat`|
| `dlllist`  | Lists DLLs loaded by processes (useful to find suspicious DLLs).             | `python3 vol.py -f <file> windows.dlllist`|

- For unstable `netstat`, consider using `bulk_extractor` to extract PCAP from memory:  
  https://tools.kali.org/forensics/bulk-extractor

---

## Hunting and Detection Plugins
- `malfind`: Detects injected code/processes with RWE or RX memory and no file on disk. Useful for code injection and shellcode detection.  
  ```bash
  python3 vol.py -f <file> windows.malfind
  ```
- `yarascan`: Scan memory using YARA rules (can specify rules inline or via file).  
  ```bash
  python3 vol.py -f <file> windows.yarascan
  ```

---

## Advanced Memory Forensics and Rootkit Detection

### Hooking Techniques (focus on SSDT hooking)
- SSDT Hooks: Common rootkit evasion by hooking system call table.
- Use `ssdt` plugin to detect hooks:  
  ```bash
  python3 vol.py -f <file> windows.ssdt
  ```

### Driver Modules
- `modules`: List loaded kernel modules (drivers).  
  ```bash
  python3 vol.py -f <file> windows.modules
  ```
- `driverscan`: Scan for drivers in kernel memory (may reveal hidden drivers).  
  ```bash
  python3 vol.py -f <file> windows.driverscan
  ```

### Other useful advanced plugins (may be Volatility2 or third-party)
- `modscan`, `driverirp`, `callbacks`, `idt`, `apihooks`, `moddump`, `handles`

---

## Conclusion and Further Reading
- Volatility memory forensics is vast; this cheatsheet covers only basics.
- For deep dives, consult *The Art of Memory Forensics* book.
- Additional resources:
  - https://github.com/volatilityfoundation/volatility/wiki
  - https://github.com/volatilityfoundation/volatility/wiki/Volatility-Documentation-Projec
  - https://digital-forensics.sans.org/media/Poster-2015-Memory-Forensics.pdf
  - https://eforensicsmag.com/finding-advanced-malware-using-volatility/
- Continue SOC Level 1 path for practical challenges.

---

*Generated by ChatGPT*
