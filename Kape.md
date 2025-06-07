
# 🧰 KAPE (Kroll Artifact Parser and Extractor) Cheat Sheet

> A complete reference based on the [TryHackMe KAPE Room](https://tryhackme.com/room/kape) and additional research.

---

## 📌 Overview

**KAPE** is a portable triage tool designed for digital forensics and incident response (DFIR). It rapidly collects and processes forensic artifacts from Windows systems.

- **Main Functions**:
  - **Collection**: Gathers specified forensic artifacts using *Targets*.
  - **Processing**: Analyzes collected data using *Modules*.

- **Execution**:
  - CLI: `kape.exe`
  - GUI: `gkape.exe`

---

## 🧩 Key Terminology

| Term             | Description                                                                 |
|------------------|-----------------------------------------------------------------------------|
| `Target`         | Defines artifacts to collect (e.g., registry hives, prefetch).              |
| `Module`         | Specifies how to process artifacts (e.g., using tools like PECmd).          |
| `Compound Target`| A group of multiple targets for broader collection.                         |
| `.tkape`         | Target file extension.                                                      |
| `.mkape`         | Module file extension.                                                      |
| `%d`             | Appends date/time to output path.                                           |
| `%m`             | Appends machine name to output path.                                        |

---

## ⚙️ Directory Structure

```
KAPE/
├── kape.exe              # CLI
├── gkape.exe             # GUI
├── Targets/              # .tkape files
├── Modules/              # .mkape files
├── Bin/                  # Dependencies/tools
├── Get-KAPEUpdate.ps1    # Update script
└── Documentation/
```

---

## 🖥️ GUI Usage (`gkape.exe`)

1. **Launch GUI**: Run `gkape.exe` as Administrator.
2. **Set Target Options**:
   - Source: Path to system or mounted image.
   - Destination: Folder to store collected artifacts.
   - Choose targets (e.g., `KapeTriage`).
3. **Set Module Options**:
   - Source: Usually same as target output.
   - Destination: Folder for parsed output.
   - Choose modules (e.g., `!EZParser`).
4. **Use `%d` and `%m`** for timestamp and hostname.
5. **Click Execute**.

---

## 💻 CLI Usage (`kape.exe`)

### Basic Syntax

```bash
kape.exe --tsource <source> --tdest <target_output> --target <targets>          --msource <module_input> --mdest <module_output> --module <modules>
```

### Common Flags

| Flag         | Purpose                                  |
|--------------|------------------------------------------|
| `--tsource`  | Source system (e.g., `C:\`)              |
| `--tdest`    | Directory to store collected files       |
| `--target`   | Comma-separated targets (e.g., `KapeTriage`) |
| `--msource`  | Path to artifacts for processing         |
| `--mdest`    | Output directory for processed results   |
| `--module`   | Modules to run                           |
| `--tflush`   | Clean target output dir before running   |
| `--mflush`   | Clean module output dir before running   |
| `--vhdx`     | Treat source as mounted VHDX             |
| `--cu`       | Clean up after processing                |
| `--debug`    | Enable debug output                      |

### Example Command

```bash
kape.exe --tsource C: --tdest C:\KAPE\Collected --target KapeTriage          --msource C:\KAPE\Collected --mdest C:\KAPE\Parsed --module !EZParser --vhdx
```

---

## 📁 Targets (.tkape)

Define what data to collect.

### Example: `Prefetch.tkape`

```xml
<Target>
  <TargetName>Prefetch</TargetName>
  <Description>Collects Windows Prefetch files</Description>
  <Paths>
    <Path>C:\Windows\Prefetch\*.pf</Path>
  </Paths>
</Target>
```

---

## 🧪 Modules (.mkape)

Define how to process data.

### Example: `PrefetchParser.mkape`

```xml
<Module>
  <ModuleName>PrefetchParser</ModuleName>
  <Description>Parses Prefetch files using PECmd</Description>
  <Program>PECmd.exe</Program>
  <Arguments>-d %source% -o %destination%</Arguments>
</Module>
```

---

## ✅ Best Practices

- Use `KapeTriage` for quick, wide artifact collection.
- Always run as Administrator.
- Combine targets and modules for full automation.
- Update often using:
  ```powershell
  .\Get-KAPEUpdate.ps1
  ```
- Clean before re-runs using `--tflush` and `--mflush`.
- Use `--debug` to troubleshoot issues.

---

## 🔗 References

- [TryHackMe: KAPE Room](https://tryhackme.com/room/kape)
- [Eric Zimmerman's Tools](https://ericzimmerman.github.io/)
- [KAPE GitHub Repo](https://github.com/EricZimmerman/KapeFiles)
- [KAPE Official Guide](https://www.kroll.com/en/services/cyber-risk/kape)

---
