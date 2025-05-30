
# Windows Evidence of Execution & USB Artifacts - Forensic Cheatsheet

---

## 1. Windows Prefetch Files

**Purpose:**  
Stores info about executed programs to speed up future launches. Provides last run times, run count, files & device handles used.

**Location:**  
```
C:\Windows\Prefetch
```

**File Extension:** `.pf`

**Key Info:**  
- Last run times of apps  
- Number of times app was run  
- Files & device handles accessed

**Tool:** `PECmd.exe` by Eric Zimmerman  
- GitHub: https://github.com/EricZimmerman/PECmd  
- Run in elevated Command Prompt.

**Basic Usage:**  
```bash
PECmd.exe -f <prefetch-file-path> --csv <csv-output-path>
PECmd.exe -d <prefetch-directory> --csv <csv-output-path>
```

**Options:**
- `-d` : Directory to recursively process  
- `-f` : Single file to process  
- `-k` : Keywords to highlight (default: temp,tmp)  
- `--json`, `--csv`, `--html` : Save output in different formats  
- `-q` : Suppress detailed output for speed  
- `-vss` : Include Volume Shadow Copies  
- `-debug` / `-trace` : Debug or trace info

---

## 2. Windows 10 Timeline

**Purpose:**  
Stores recently used applications and files in an SQLite DB, including focus time.

**Location:**  
```
C:\Users\<username>\AppData\Local\ConnectedDevicesPlatform\{randomfolder}\ActivitiesCache.db
```

**Tool:** `WxTCmd.exe` by Eric Zimmerman  
- GitHub: https://github.com/EricZimmerman/WxTCmd

**Usage:**  
```bash
WxTCmd.exe -f <path-to-ActivitiesCache.db> --csv <csv-output-path>
```

---

## 3. Windows Jump Lists

**Purpose:**  
Stores recent files opened per application, including first and last execution times.

**Location:**  
```
C:\Users\<username>\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations
```

**File Types:** `.automaticDestinations-ms` and `.customDestinations-ms`

**Tool:** `JLECmd.exe` by Eric Zimmerman  
- GitHub: https://github.com/EricZimmerman/JLECmd

**Usage:**  
```bash
JLECmd.exe -f <jumplist-file> --csv <csv-output-path>
JLECmd.exe -d <jumplist-directory> --csv <csv-output-path>
```

---

## 4. Shortcut Files (.lnk)

**Purpose:**  
Shortcuts created when files are opened locally or remotely; contain file path, first and last open times.

**Locations:**  
```
C:\Users\<username>\AppData\Roaming\Microsoft\Windows\Recent\
C:\Users\<username>\AppData\Roaming\Microsoft\Office\Recent\
```

**Tool:** `LECmd.exe` (Lnk Explorer) by Eric Zimmerman  
- GitHub: https://github.com/EricZimmerman/LECmd

**Usage:**  
```bash
LECmd.exe -f <shortcut-file.lnk> --csv <csv-output-path>
LECmd.exe -d <shortcut-directory> --csv <csv-output-path>
```

---

## 5. IE/Edge Browsing History

**Purpose:**  
Includes local files accessed with prefix `file:///*`, capturing files opened via browser or other means.

**Location:**  
```
C:\Users\<username>\AppData\Local\Microsoft\Windows\WebCache\WebCacheV*.dat
```

**Tool:**  
Use **Autopsy**  
- Add Logical Files data source pointing to `triage` folder  
- Enable `Recent Activity` ingest module  
- View "Local Files Accessed" in Web History section

---

## 6. USB Artifacts

**Purpose:**  
Evidence of USB devices connected, including device IDs, serial numbers, timestamps, and drive letters.

**Important Locations:**

- **SetupAPI Logs:**  
  ```
  C:\Windows\inf\setupapi.dev.log
  ```
  Tracks device installation events.

- **Registry Keys:**  
  ```
  HKLM\SYSTEM\CurrentControlSet\Enum\USBSTOR\
  HKLM\SYSTEM\CurrentControlSet\Enum\USB\
  HKLM\SYSTEM\CurrentControlSet\Services\USBSTOR
  ```
  Contains info on USB devices ever connected.

- **MountedDevices:**  
  ```
  HKLM\SYSTEM\MountedDevices
  ```
  Shows drive letter assignments for USB drives.

- **USB Device History:**  
  ```
  C:\Users\<username>\AppData\Local\Microsoft\Windows\DeviceMetadataCache
  ```
  May contain device metadata and timestamps.

- **Windows Setup Logs:**  
  ```
  C:\Windows\inf\setupapi.dev.log
  ```
  Records device installation and connection times.

**Timestamps to Look For:**  
- First connection time (device install time)  
- Last connection time  
- Volume serial numbers  
- Drive letters assigned

**Tools for USB Forensics:**  
- **USBDeview** (NirSoft) — Lists USB devices connected with details  
- **Registry Viewer** tools to parse USB registry keys  
- Custom scripts for parsing SetupAPI logs and registry hives

---

# Summary Table of Artifacts & Evidence

| Artifact           | Location / File                                  | Tool / Analysis Method       | Evidence Provided                             |
|--------------------|------------------------------------------------|------------------------------|-----------------------------------------------|
| Prefetch           | `C:\Windows\Prefetch\*.pf`                      | `PECmd.exe`                  | Program execution times, run count, files used|
| Windows 10 Timeline| `C:\Users\<user>\AppData\Local\ConnectedDevicesPlatform\*\ActivitiesCache.db` | `WxTCmd.exe`                 | Recent apps run, focus times                    |
| Jump Lists         | `C:\Users\<user>\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations\` | `JLECmd.exe`                 | Recent files opened per app, execution times   |
| Shortcut Files     | `C:\Users\<user>\AppData\Roaming\Microsoft\Windows\Recent\` & Office Recent | `LECmd.exe`                  | File open times, file paths                     |
| IE/Edge History    | `C:\Users\<user>\AppData\Local\Microsoft\Windows\WebCache\WebCacheV*.dat` | Autopsy                      | Files accessed via browser or file:// links    |
| USB Devices        | Registry: USBSTOR keys, SetupAPI logs, MountedDevices registry | USBDeview, Registry tools    | USB device connect/disconnect timestamps, IDs |

---

*This cheatsheet summarizes key Windows forensic artifacts useful for investigating program execution, file access, and USB device usage.*
