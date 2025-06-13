
# 🛡️ Windows Forensics Cheat Sheet 1 (Registry)

This cheat sheet provides key registry paths and usage for forensic investigation on a Windows system.

---

## 🖥️ System Info & Accounts

- **OS Version**  
  `SOFTWARE\Microsoft\Windows NT\CurrentVersion`

- **Current Control Set**  
  - `HKLM\SYSTEM\CurrentControlSet`  
  - `SYSTEM\Select\Current`  
  - `SYSTEM\Select\LastKnownGood`

- **Computer Name**  
  `SYSTEM\CurrentControlSet\Control\ComputerName\ComputerName`

- **Time Zone Information**  
  `SYSTEM\CurrentControlSet\Control\TimeZoneInformation`

- **Network Interfaces & Past Networks**  
  `SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces`

- **SAM Hive (User Info)**  
  `SAM\Domains\Account\Users`

---

## 🚀 Autostart Programs (Autoruns)
Tracks programs configured to run at startup.

- `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Run`  
- `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\RunOnce`  
- `SOFTWARE\Microsoft\Windows\CurrentVersion\Run`  
- `SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce`  
- `SOFTWARE\Microsoft\Windows\CurrentVersion\policies\Explorer\Run`

---

## 📁 Recent Files (User Activity)
**Registry Hive**: `NTUSER.DAT`  
- `Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs`  
  - Stores MRU (Most Recently Used) files.
  - Contains subkeys by file extension like `.pdf`, `.jpg`, `.docx`.

---

## 📝 Office Recent Files  
**Registry Hive**: `NTUSER.DAT`  
- Office (generic):  
  `Software\Microsoft\Office\<VERSION>\UserMRU\LiveID_####\FileMRU`  
- Office 2013 example:  
  `Software\Microsoft\Office\15.0\Word`  
  - Stores full file paths of recent Office documents.

---

## 🧳 ShellBags (Folder View Forensics)
**Registry Hives**: `NTUSER.DAT`, `USRCLASS.DAT`  
- `NTUSER.DAT\Software\Microsoft\Windows\Shell\BagMRU`  
- `NTUSER.DAT\Software\Microsoft\Windows\Shell\Bags`  
- `USRCLASS.DAT\Local Settings\Software\Microsoft\Windows\Shell\BagMRU`  
- `USRCLASS.DAT\Local Settings\Software\Microsoft\Windows\Shell\Bags`  

> 📦 Use **ShellBag Explorer** (Eric Zimmerman) to parse.

---

## 📂 Open/Save Dialog MRUs  
**Registry Hive**: `NTUSER.DAT`  
- `Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\OpenSavePIDlMRU`  
- `Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\LastVisitedPidlMRU`

---

## 🔍 Windows Explorer: Address/Search Bar
**Registry Hive**: `NTUSER.DAT`  
- Typed Paths:  
  `Software\Microsoft\Windows\CurrentVersion\Explorer\TypedPaths`  
- Search Queries:  
  `Software\Microsoft\Windows\CurrentVersion\Explorer\WordWheelQuery`

---

## ⚙️ UserAssist (GUI Launched Programs)
**Registry Hive**: `NTUSER.DAT`  
- `Software\Microsoft\Windows\Currentversion\Explorer\UserAssist\{GUID}\Count`  
  - Tracks GUI-launched programs with execution time and launch count.
  - Encoded using ROT13.

---

## 🧬 ShimCache / AppCompatCache
**Registry Hive**: `SYSTEM`  
- `ControlSet001\Control\Session Manager\AppCompatCache`  
  - Stores executed program artifacts, file path, size, and timestamps.

> 🛠 Tool: `AppCompatCacheParser.exe`
```bash
AppCompatCacheParser.exe --csv <output_path> -f <SYSTEM_hive> -c <control_set>
```

---

## 🧾 AmCache (Detailed Execution Info)
**File**: `C:\Windows\appcompat\Programs\Amcache.hve`  
- `Root\File\{Volume GUID}\`  
  - Tracks program install path, install time, SHA1 hash, execution and deletion times.

---

## 📊 BAM/DAM (Background App Monitoring)
**Registry Hive**: `SYSTEM`  
- `CurrentControlSet\Services\bam\UserSettings\{SID}`  
- `CurrentControlSet\Services\dam\UserSettings\{SID}`  
  - Records paths and last run time of background applications.

---

## 💽 USB Device Forensics

### 🔎 1. Device Identification
**Registry Hive**: `SYSTEM`  
- `CurrentControlSet\Enum\USBSTOR`  
- `CurrentControlSet\Enum\USB`  
  - Vendor ID, Product ID, Version, Plug-in Time

### 🕒 2. First/Last Connection Times
**Registry Hive**: `SYSTEM`  
- `CurrentControlSet\Enum\USBSTOR\Ven_Prod_Version\USBSerial#\Properties\{83da6326-97a6-4088-9453-a19231573b29}\####`  
  - `0064` – First connection  
  - `0066` – Last connection  
  - `0067` – Last removal  

### 🏷 3. USB Volume Name
**Registry Hive**: `SOFTWARE`  
- `Microsoft\Windows Portable Devices\Devices`  
  - Cross-reference with `USBSTOR` entries for full device name mapping.

---

## 🛠️ Recommended Tools

| Tool | Purpose |
|------|---------|
| **Registry Explorer** | Browse registry hives |
| **AppCompatCache Parser** | Parse AppCompatCache |
| **ShellBag Explorer** | View ShellBag contents |
| **EZViewer** | View/convert encoded registry data |

---

## 📌 Notes
- Load hives from forensic disk image or extract manually.
- Decode `UserAssist` values using ROT13 or GUI tool.
- Correlate with other evidence sources (event logs, prefetch, etc.).
- BAM/DAM helpful for stealthy execution tracking.
- ShellBags helpful to show folder activity—even on deleted paths.
