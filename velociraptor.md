# ✅ Velociraptor Cheat Sheet
**Author**: Tanishq Nama  
**Tool**: [Velociraptor](https://www.velociraptor.app/)  
**Purpose**: Endpoint visibility, live forensics, threat hunting

---

## 📌 Introduction
Velociraptor is an advanced open-source DFIR and monitoring tool used to query, collect, and analyze data from multiple endpoints in real time. It uses its own query language called **VQL (Velociraptor Query Language)**.

---

## 🚀 Installation

```bash
# Download Velociraptor
wget https://github.com/Velocidex/velociraptor/releases/latest/download/velociraptor-vx.x.x-linux-amd64
chmod +x velociraptor-vx.x.x-linux-amd64

# Create a config
./velociraptor-vx.x.x-linux-amd64 config generate > server.config.yaml

# Start server
./velociraptor-vx.x.x-linux-amd64 -config server.config.yaml frontend

# Start GUI (default: https://localhost:8889)
```

---

## 🧠 Core Components

| Component     | Description                                   |
|---------------|-----------------------------------------------|
| **Frontend**  | Main interface users connect to               |
| **Client**    | Agent installed on endpoints                  |
| **VQL**       | Velociraptor Query Language                   |
| **Artifact**  | Predefined queries for forensic data          |
| **Collector** | Collects data like logs, artifacts, binaries  |

---

## 📂 Common Artifact Categories

- `Windows.*` – For Windows systems (e.g., `Windows.System.Prefetch`)
- `Linux.*` – For Linux endpoints
- `MacOS.*` – For Mac endpoints
- `Generic.*` – Cross-platform artifacts
- `PE.*` – PE file analysis (hashing, strings, imports)

---

## 🧾 Common VQL Queries

```sql
# List running processes (Windows)
SELECT * FROM pslist()

# List network connections
SELECT * FROM netstat()

# Get prefetch info
SELECT * FROM prefetch()

# List autoruns
SELECT * FROM autoruns()

# Search files for a keyword
SELECT * FROM glob(globs='C:\\Users\\*\\Documents\\*.txt') WHERE content =~ 'password'
```

---

## 🔍 Artifact Use Examples

```bash
# Run prefetch artifact
Windows.System.Prefetch

# Run YARA scan
Generic.Yara.ProcessMemory

# Collect MFT
Windows.NTFS.MFT

# Get browser history
Windows.Browser.ChromeHistory

# Run across multiple clients
Server -> Hunt Manager -> New Hunt -> Select Artifact -> Target Hosts
```

---

## 🔐 Authentication & Access

- Web UI: `https://localhost:8889`
- Default credentials: set during setup
- Use `velociraptor user` commands to manage users

```bash
# Add a user
./velociraptor user add admin --role administrator
```

---

## 📦 File Collection

```bash
# Collect files from endpoints
SELECT * FROM glob(globs='C:\\Users\\*\\Desktop\\*')

# Download from Web UI or via CLI
```

---

## 💡 Useful Built-In Artifacts

| Artifact                          | Purpose                            |
|----------------------------------|------------------------------------|
| `Windows.System.Prefetch`        | Prefetch file parsing              |
| `Windows.Detection.AnomalousPs`  | Detect suspicious processes        |
| `Windows.EventLogs.EvtxHunter`   | Custom event log hunting           |
| `Generic.Utils.FetchBinary`      | Download file from endpoint        |
| `Generic.Hunting.YaraScan`       | YARA scan on disk or memory        |
| `Linux.Sys.BashHistory`          | Collect `.bash_history`            |

---

## 🛠️ Hunt Creation (Step-by-Step)

1. Go to **Hunt Manager** → **New Hunt**
2. Select target artifact(s)
3. Choose target endpoints (based on label or all)
4. Configure parameters (if needed)
5. Launch hunt and monitor results

---

## 📊 Logs & Output

- Collected artifacts and outputs are downloadable as:
  - `.csv`
  - `.json`
  - Raw data
- Stored in server directory under `output/`

---

## 📚 Resources

- [Official Docs](https://docs.velociraptor.app/)
- [GitHub Repository](https://github.com/Velocidex/velociraptor)
- [Community Wiki](https://docs.velociraptor.app/docs/learn/tutorials/)
- [Training](https://academy.velociraptor.app/)
