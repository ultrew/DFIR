# 🐧 Linux Forensics Cheatsheet

---

## 📘 System and OS Information

| Info Type               | Location                          | Notes                                      |
|------------------------|-----------------------------------|--------------------------------------------|
| OS Release Info        | `/etc/os-release`                 | Use `cat`, `vim`, etc.                     |
| User Accounts          | `/etc/passwd`                     | Lists user accounts                        |
| User Groups            | `/etc/group`                      | Lists group info                           |
| Sudoers List           | `/etc/sudoers`                    | Requires `sudo` or root to read            |
| Login Info             | `/var/log/wtmp`                   | View using `last` command                  |
| Authentication Logs    | `/var/log/auth.log*`              | Use `cat`, `vim`, `grep`                   |
| Cron Jobs              | `/etc/crontab`                    | Lists scheduled jobs                       |
| Services               | `/etc/init.d/`                    | Registered services                        |
| Bash Startup Scripts   | `~/.bashrc`, `/etc/bash.bashrc`, `/etc/profile` | Per-user and system-wide config    |

---

## 🛡️ Persistence Mechanism

| Artifact              | Location                           | Notes                                        |
|-----------------------|------------------------------------|----------------------------------------------|
| Auth Logs             | `/var/log/auth.log*`               | Use `grep -i COMMAND`                        |
| Bash History          | `/home/<user>/.bash_history`       | Shows user command history                   |
| Vim History           | `/home/<user>/.viminfo`            | Tracks opened files, search, etc.            |

---

## 🧪 Evidence of Execution

| Artifact              | Location                           | Notes                                        |
|-----------------------|------------------------------------|----------------------------------------------|
| Syslogs               | `/var/log/syslog`                  | Use `grep` for filtering                     |
| Auth Logs             | `/var/log/auth.log`                | Look for `sudo`, `ssh`, login attempts, etc. |
| Third-party Logs      | `/var/log/`                        | Check subdirectories for app-specific logs   |

---

## 📄 Log Files & System Details

| Info Type              | Location / Command                | Notes                                        |
|------------------------|----------------------------------|----------------------------------------------|
| Hostname               | `/etc/hostname`                  | System hostname                              |
| Timezone               | `/etc/timezone`                  | System timezone                              |
| Network Interfaces     | `/etc/network/interfaces`        | Network interface settings                   |
| IP Address             | `ip address show`                | Live system command                          |
| Open Network Conns     | `netstat -natp`                  | Live system command                          |
| Running Processes      | `ps aux`                         | Shows current processes                      |
| DNS Host Resolutions   | `/etc/hosts`                     | Maps hostnames to IPs                        |
| DNS Server Info        | `/etc/resolv.conf`               | Lists configured DNS servers                 |

---

🧠 **Tips for Analysts:**
- Use `grep`, `less`, or `awk` to filter and parse large logs.
- Check for rotated logs like `auth.log.1`, `auth.log.2.gz`, etc.
- Analyze `.bashrc` and `.profile` for malicious persistence.

---

📚 **Related Learning:**  
[TryHackMe - Linux Forensics Room](https://tryhackme.com/room/linuxforensics)

