
# Volatility Memory Forensics Cheatsheet (Windows / Linux / Mac)

---

## Overview

**Volatility** is an open-source framework for memory forensics — extract runtime artifacts from memory (RAM) dumps — supports:
✅ Windows  
✅ Linux  
✅ MacOS

Used by:
- SOC Analysts
- Threat Hunters
- Malware Analysts
- Incident Responders

**Volatility 3** → Python 3 → [https://github.com/volatilityfoundation/volatility3](https://github.com/volatilityfoundation/volatility3)

---

## Installation

```bash
git clone https://github.com/volatilityfoundation/volatility3.git
cd volatility3
pip install pefile yara-python capstone
python3 vol.py -h
```

---

## Memory Dump Sources

| Source        | File Type |
|---------------|-----------|
| FTK Imager    | .raw      |
| DumpIt        | .raw      |
| Memoryze      | .raw/.img |
| AVML (Linux)  | .lime     |
| LiME (Linux)  | .lime     |
| VMWare        | .vmem     |
| Hyper-V       | .bin      |

---

## Core Plugins by OS

| Category               | Windows                           | Linux                          | MacOS                        |
|------------------------|-----------------------------------|-------------------------------|-----------------------------|
| System Info            | windows.info                      | linux.info                    | mac.info                    |
| Process List           | windows.pslist, pstree, psscan    | linux.pslist, linux.pstree     | mac.pslist                   |
| Command-line Args      | windows.cmdline                    | linux.bash                     | mac.bash                     |
| Network                | windows.netstat, sockscan          | linux.netstat, linux.ifconfig   | mac.netstat                  |
| Files & Handles        | windows.filescan, handles          | linux.lsof                     | mac.filescan, mac.lsof       |
| DLLs / Modules         | dlllist, modules, modscan          | linux.elfs, linux.modules      | mac.modules                  |
| Malware Detection      | malfind, yarascan                  | yarascan                       | yarascan                     |
| Registry / Config      | printkey, svcscan, shimcache       | (Linux config manual)          | mac.defaults_read            |
| Scheduled Tasks        | schtasks, joblinks                 | crontab (manual)               | mac.launchd                  |
| User Activity          | shimcache, userassist, shellbags   | bash_history, zsh_history      | bash_history, zsh_history    |
| Dumping Artifacts      | procdump, dlldump, moddump         | linux.proc_maps                | mac.proc_maps                |
| Rootkit Detection      | ssdt, callbacks, driverscan        | linux.check_modules            | mac.check_modules            |

---

## Example Windows Commands

```bash
python3 vol.py -f mem.raw windows.info
python3 vol.py -f mem.raw windows.pslist
python3 vol.py -f mem.raw windows.pstree
python3 vol.py -f mem.raw windows.cmdline
python3 vol.py -f mem.raw windows.filescan
python3 vol.py -f mem.raw windows.handles
python3 vol.py -f mem.raw windows.netstat
python3 vol.py -f mem.raw windows.malfind
python3 vol.py -f mem.raw windows.yarascan --yara-rules myrules.yar
python3 vol.py -f mem.raw windows.ssdt
python3 vol.py -f mem.raw windows.procdump --pid <pid> -D ./dumped/
```

---

## Example Linux Commands

```bash
python3 vol.py -f mem.lime linux.info
python3 vol.py -f mem.lime linux.pslist
python3 vol.py -f mem.lime linux.pstree
python3 vol.py -f mem.lime linux.bash
python3 vol.py -f mem.lime linux.lsof
python3 vol.py -f mem.lime linux.netstat
python3 vol.py -f mem.lime linux.elfs
python3 vol.py -f mem.lime linux.yarascan --yara-rules linux_rules.yar
```

---

## Example MacOS Commands

```bash
python3 vol.py -f mac.mem mac.info
python3 vol.py -f mac.mem mac.pslist
python3 vol.py -f mac.mem mac.pstree
python3 vol.py -f mac.mem mac.bash
python3 vol.py -f mac.mem mac.lsof
python3 vol.py -f mac.mem mac.netstat
python3 vol.py -f mac.mem mac.yarascan --yara-rules mac_rules.yar
```

---

## Further Resources

- *The Art of Memory Forensics*  
- Volatility Wiki → https://github.com/volatilityfoundation/volatility/wiki  
- Volatility3 Docs → https://volatility3.readthedocs.io  
- SANS Memory Forensics Poster → https://digital-forensics.sans.org/media/Poster-2015-Memory-Forensics.pdf  
- https://eforensicsmag.com/finding-advanced-malware-using-volatility/

---
