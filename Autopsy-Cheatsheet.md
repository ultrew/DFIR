# 🕵️ Autopsy Forensics Tool Cheat Sheet

> A practical quick-reference for using Autopsy – a powerful digital forensics platform built on SleuthKit.

---

## 📦 Data Source Types Supported
- Raw Disk Images (`.dd`, `.img`)
- EnCase Images (`.E01`)
- Virtual Machines (`.vmdk`)
- Logical Files/Folders
- RAM Dumps

---

## ⚙️ Ingest Modules

Autopsy plug-ins that extract data/artifacts from evidence. Can be configured:
- 🔁 While **adding data source**
- 🔧 **After data source is added**: Right-click → "Run Ingest Modules"

📌 **Defaults**: All files, directories, and unallocated space

🧠 Examples:
- **Keyword Search** (no per-run config)
- **Interesting Files Finder** (has per-run config ⚠️ yellow triangle)

📍 Output appears in:  
`Tree Viewer → Results` node

---

## 🧭 User Interface Overview

### 🌲 Tree Viewer (Left Pane)
- **Data Sources** – File Explorer-like view
- **Views** – Organize by Extension, MIME, Deleted Files, File Size
- **Results** – Outputs from Ingest Modules
- **Tags** – Analyst or module-applied tags
- **Reports** – Analyst or module-generated reports

### 📋 Result Viewer (Center Pane)
Shows file info when an item is selected from Tree Viewer.

Tabs:
- `Table` – Structured info
- `Thumbnail` – Visual files
- `Summary` – Key metadata

Right-click any file → `Extract File(s)`

### 🧩 Contents Viewer (Lower Center)
Shows metadata or preview of selected files.

Columns:
- `S (Score)` – Red ❗ = Notable | Yellow ⚠️ = Suspicious
- `C (Comment)` – Shows analyst comments
- `O (Occurrence)` – Seen in previous cases (requires Central Repo)

### 🔎 Keyword Search (Top-Right)
Search terms on-the-fly. Options:
- Keyword Lists
- Manual Search

### 📊 Status Area (Bottom-Right)
Shows progress of Ingest Modules  
Click bar = More info  
Click ❌ = Cancel module execution

---

## 🧮 Timeline Tool

🗓 Visualize events over time from artifacts.

### View Modes:
- `Counts` – Bar chart
- `Details` – Clustered timeline
- `List` – Tabular view

### Controls:
- ➕ Expand Cluster
- ➖ Collapse Cluster
- 📍 Pin Events
- 🙈 Hide / Unhide groups

---

## 🧾 Data Source Summary

Summarized high-level artifact stats:
- OS info
- User activity
- Communication artifacts
- File metadata

📌 Access by double-clicking the image name under `Data Sources`.

---

## 📤 Generate Report

Create summary reports in:
- HTML
- Excel
- Text
- Other formats

Helps continue investigation **offline** (lighter on system resources).

---

## 🔧 Advanced Settings to Explore

- 🔍 **Global Keyword Search Settings**
- 🧩 **Global Hash Lookup Settings**
- 🧷 **File Extension Mismatch Identification**
- 🌟 **Interesting Items Rules**
- 🐍 **YARA Analyzer**
- 🧩 **3rd-Party Modules**:  
  [SleuthKit GitHub Modules](https://github.com/sleuthkit/autopsy)

---

## 📁 Practice Dataset

📂 CFReDS by NIST (Forensics disk images):  
🔗 https://www.cfreds.nist.gov/

---

## 🧠 Tips

- Watch for file extension mismatch in `Views → MIME Type`
- Tag key files for future reporting
- Use reports if UI lags on low-resource systems
- Timeline is very useful for narrowing down event windows

---

## 🛠️ Resources

- Autopsy Docs: https://sleuthkit.org/autopsy/docs/user-docs/
- Timeline: https://sleuthkit.org/autopsy/docs/user-docs/4.12.0/timeline_page.html
- Communications: https://sleuthkit.org/autopsy/docs/user-docs/4.12.0/communications_page.html
- Image Gallery: https://sleuthkit.org/autopsy/docs/user-docs/4.12.0/image_gallery_page.html

---

**Author:** Tanishq Nama  
**Last Updated:** June 2, 2025

