# 🛡️ Windows Forensics Cheat Sheet 1 (Registry)
This cheat sheet provides key registry paths and usage for forensic investigation on a Windows system.

---

## 📁 Recent Files (User Activity)
**Registry Hive**: `NTUSER.DAT`  
- `Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs`  
  - Stores MRU (Most Recently Used) files.
  - Contains subkeys by file extension like `.pdf`, `.jpg`, `.docx`.

---

## 📝 Office Recent Files  
**Registry Hive**: `NTUSER.DAT`  
- Office 2013 example:  
  `Software\Microsoft\Office\15.0\Word`  
- Office 365+:  
  `Software\Microsoft\Office\<VERSION>\UserMRU\LiveID_####\FileMRU`  
  - Stores complete paths of recently used files.

---

## 🧳 ShellBags (Folder View Forensics)
**Registry Hives**: `NTUSER.DAT`, `USRCLASS.DAT`  
- `NTUSER.DAT\Software\Microsoft\Windows\Shell\BagMRU`  
- `NTUSER.DAT\Software\Microsoft\Windows\Shell\Bags`  
- `USRCLASS.DAT\Local Settings\Software\Microsoft\Windows\Shell\BagMRU`  
- `USRCLASS.DAT\Local Settings\Software\Microsoft\Windows\Shell\Bags`  

Use **ShellBag Explorer** (Eric Zimmerman) to parse this.

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
- `Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist\{GUID}\Count`  
  - Tracks programs launched via GUI with timestamp and launch count.
  - Encoded in ROT13.

---

## 🧬 ShimCache / AppCompatCache
**Registry Hive**: `SYSTEM`  
- `ControlSet001\Control\Session Manager\AppCompatCache`  
  - Stores program execution artifacts, file size, and last modified time.

**Tool**: Use `AppCompatCacheParser.exe`  
```bash
AppCompatCacheParser.exe --csv <output_path> -f <SYSTEM_hive> -c <control_set>
```

---

## 🧾 AmCache (Detailed Execution Info)
**File**: `C:\Windows\appcompat\Programs\Amcache.hve`  
- Key: `Root\File\{Volume GUID}\`  
  - Stores path, install time, SHA1 hash, execution and deletion times.

---

## 📊 BAM/DAM (Background Execution Monitoring)
**Registry Hive**: `SYSTEM`  
- `CurrentControlSet\Services\bam\UserSettings\{SID}`  
- `CurrentControlSet\Services\dam\UserSettings\{SID}`  
  - Tracks last run time and full paths of background apps.

---

## 💽 USB Device Forensics

### 1. **Device Identification**
**Registry Hive**: `SYSTEM`  
- `CurrentControlSet\Enum\USBSTOR`  
- `CurrentControlSet\Enum\USB`  
  - Shows Vendor ID, Product ID, Version, Plug-in Time.

### 2. **First/Last Connection Times**
**Registry Hive**: `SYSTEM`  
- `CurrentControlSet\Enum\USBSTOR\Ven_Prod_Version\USBSerial#\Properties\{83da6326-97a6-4088-9453-a19231573b29}\####`  
  - `0064` – First connection  
  - `0066` – Last connection  
  - `0067` – Last removal  

### 3. **USB Volume Name**
**Registry Hive**: `SOFTWARE`  
- `Microsoft\Windows Portable Devices\Devices`  
  - Cross-reference GUIDs with USBSTOR for device name mapping.

---

## 🛠️ Recommended Tools
- **Registry Explorer** (Eric Zimmerman)  
- **AppCompatCache Parser**  
- **ShellBag Explorer**  
- **EZViewer**  

---

## 📌 Notes
- Always load hives from the disk image or extracted folders.
- Decode `UserAssist` values with ROT13 or use a GUI parser.
- Correlate timestamps with other artifacts (event logs, prefetch, etc.).
