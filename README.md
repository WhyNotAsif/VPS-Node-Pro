# VPS Node Pro

> A self-hosted, browser-based Linux server management interface.  
> SSH-equivalent terminal · GNU Screen manager · Live process control · WinSCP-style file manager  
> Built with Python (FastAPI) + pure HTML/CSS/JS · Zero external services · Mobile-ready

---

## Why This Exists

Managing a Linux VPS normally means:
- An SSH client installed on every device (PuTTY, MobaXterm, Terminal)
- A separate SFTP tool (WinSCP, FileZilla) for file transfers
- A desktop — mobile SSH is nearly unusable
- Or paying for a bloated panel (cPanel, Plesk) you use 10% of

**VPS Node Pro eliminates all of that.**  
Deploy two files. Open a browser. You have full server control from any device.

---

## Features

### ⌨ Terminal
- Run any shell command with real working-directory context
- Multi-line input (Shift+Enter), Enter to execute
- 15 quick-launch shortcuts: `ls`, `df`, `free`, `ss -tlnp`, `crontab`, `journalctl`, and more
- 60-second hard timeout with process kill
- Color-coded output: stdout / stderr / system messages

### 🖥 Screen Manager
- Create named, detached GNU screen sessions with an optional startup command
- View live scrollback buffer (last 120 lines) via `screen hardcopy`
- **Auto-refresh** every 3 seconds — watch your app logs without attaching
- Send commands into any running session (`screen -X stuff`)
- Kill sessions instantly
- Visual Detached / Attached status badge per session

### ⚙ Process Manager
- Full process table sorted by CPU% (PID, name, user, CPU%, RAM%, status, command)
- Live search — filter by name or command string
- Click any row → detailed modal: exe, cwd, threads, connections, open files, RSS + VMS memory
- Send **SIGTERM, SIGKILL, SIGHUP, SIGINT** from table or modal
- Graceful permission-denied handling

### 📁 File Manager (WinSCP-style)
- Click to navigate directories
- Upload files to any server path
- Download any file directly to your browser
- Delete files or entire directory trees (with confirmation)
- Create folders (recursive `mkdir -p`)
- Shows size, octal permissions, last modified

### 📊 Stats Bar
- CPU%, RAM%, Disk% — auto-refreshed every 15s
- Network I/O (bytes sent / received)
- Core count

---

## Tech Stack

| Layer | Technology |
|---|---|
| Backend | Python 3.10+, FastAPI, psutil, asyncio |
| Server | Uvicorn (ASGI) |
| Frontend | HTML5, CSS3, Vanilla JS (zero frameworks) |
| Fonts | JetBrains Mono, Rajdhani |
| Screen | GNU screen (must be installed on server) |

No database. No login system. No external APIs. No node_modules in production.

---

## Quick Start

```bash
# 1. Install
pip install -r requirements.txt

# 2. Run
python main.py

# 3. Open
http://YOUR_VPS_IP:9000
```

**Requirements:**
```
fastapi==0.115.5
uvicorn[standard]==0.32.1
psutil==6.1.0
python-multipart==0.0.18
aiofiles==24.1.0
```

GNU screen must be installed (`apt install screen`).

---

## Security

> ⚠ This tool executes real shell commands. It has **no authentication by default**.

Recommended setup:
- Bind to `localhost` only and access via SSH tunnel: `ssh -L 9000:localhost:9000 user@yourserver`
- Or run behind Nginx with HTTP Basic Auth
- Or firewall port 9000 to your IP only

---

## API Reference

| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/stats` | CPU, RAM, disk, network I/O |
| POST | `/api/exec` | Run shell command (`command`, `pwd`) |
| POST | `/api/list` | List directory contents |
| POST | `/api/upload` | Upload file to server |
| GET | `/api/download` | Download file (`?path=`) |
| POST | `/api/delete` | Delete file or directory |
| POST | `/api/mkdir` | Create folder |
| POST | `/api/rename` | Rename / move file |
| GET | `/api/screens` | List screen sessions |
| POST | `/api/screen/create` | Create detached screen |
| POST | `/api/screen/send` | Send command to screen |
| GET | `/api/screen/log` | Capture screen buffer |
| POST | `/api/screen/kill` | Kill screen session |
| GET | `/api/processes` | List processes (search, limit) |
| POST | `/api/process/kill` | Signal a process |
| GET | `/api/process/info` | Detailed process info |

---

## Project Structure

```
vps-node-pro/
├── main.py          # FastAPI backend — all 16 API endpoints
├── index.html       # Single-file frontend — 4-tab UI
└── requirements.txt
```

---

## For Recruiters

This project demonstrates:

- **Full-Stack Development** — Python backend + entire frontend UI built without any framework
- **Systems Programming** — Linux process signals, GNU screen IPC, filesystem operations, psutil integration
- **Async Python** — `asyncio.create_subprocess_shell` with timeout control, FastAPI ASGI
- **API Design** — 16 clean REST endpoints with proper error handling and HTTP status codes
- **UI Engineering** — Responsive 4-tab dark-theme interface, mobile-first, custom CSS design system
- **Problem Solving** — Identified WebSocket `finally` block as root cause of `RuntimeError`; replaced with stateless HTTP model that is more reliable and proxy-friendly

---

## Author

**Asif Ali** — Software Engineer & Public Health Technologist  
Portfolio: [asif-jamali-portfolio.vercel.app](https://asif-jamali-portfolio.vercel.app/)

