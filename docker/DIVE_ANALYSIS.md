# Dive Analysis Report: ghcr.io/agent-infra/sandbox:latest

**Image version:** 1.9.3  
**Base image:** Ubuntu 22.04  
**Architecture:** linux/amd64  
**Browser:** Chromium 146.0.7680.31  
**Analysis date:** 2026-06-14  
**Tool:** [dive](https://github.com/wagoodman/dive) v0.12.0

---

## Image Efficiency

| Metric | Value |
|--------|-------|
| Efficiency | 99.68% |
| Wasted bytes | 42 MB |
| Wasted percent | 0.50% |

## Layer Breakdown (top-down, largest first)

| # | Size | Description |
|---|------|-------------|
| 1 | **1.26 GB** | `pip install` runtime-requirements.txt (numpy, matplotlib, pandas, etc.) |
| 2 | **890 MB** | Heavy fonts (Noto CJK, emoji, Thai, Khmer, Indic, etc.) |
| 3 | **760 MB** | Desktop/VNC packages (ffmpeg, imagemagick, tigervnc, openbox, build-essential, cmake) |
| 4 | **585 MB** | `npm install -g yarn@1.22.22 bun@1.3.3` |
| 5 | **558 MB** | Node.js via fnm (v20, v22, v24) |
| 6 | **454 MB** | code-server v4.104.0 (VS Code in browser) |
| 7 | **372 MB** | Chromium browser kernel install |
| 8 | **344 MB** | Core system packages (git, python3, nginx, curl, vim, etc.) |
| 9 | **318 MB** | Python ipykernel + matplotlib for 3.10/3.11/3.12 |
| 10 | **297 MB** | opencode CLI |
| 11 | **286 MB** | CJK IME (fcitx5, dbus) |
| 12 | **280 MB** | agent-browser + MCP browser tools (npm global) |
| 13 | **222 MB** | Go 1.22.8 |
| 14 | **204 MB** | Python 3.11 + 3.12 standalone builds |
| 15 | **203 MB** | JupyterLab 4.4.5 |
| 16 | **157 MB** | Node.js runtime node_modules (from build stage) |
| 17 | **136 MB** | Browser dependency apt packages |
| 18 | **94.7 MB** | Node.js REPL server npm install |
| 19 | **78.1 MB** | Ubuntu 22.04 base layer |
| 20 | **60.2 MB** | AIO CLI build |
| 21 | **44.4 MB** | uv (Python package manager) |
| 22 | **27.9 MB** | gost proxy v3.0.0-rc10 |
| 23 | **10.1 MB** | locale-gen + fc-cache |
| 24 | **8.81 MB** | Volcengine CLI (linux_amd64_ve) |
| 25 | **7.33 MB** | pip install meson |
| 26 | **7.25 MB** | websocat v1.13.0 |
| 27 | **3.12 MB** | Python server pip requirements |
| 28 | **3.08 MB** | yt-dlp |
| 29 | **3.05 MB** | Supervisor (from git) |
| 30 | **2.55 MB** | Static sandbox assets |
| 31 | **2.33 MB** | noVNC v1.4.0 |
| — | < 1 MB | Config files, scripts, patches, symlinks (many small layers) |

## Top Wasted Files

| Count | Wasted | File |
|-------|--------|------|
| 2 | 6.2 MB | `/usr/local/bin/yt-dlp` |
| 2 | 6.0 MB | `/var/cache/fontconfig/...cache-7` |
| 6 | 3.0 MB | `/var/lib/dpkg/status` |
| 4 | 2.9 MB | `/var/cache/debconf/templates.dat` |
| 5 | 2.9 MB | `/var/lib/dpkg/status-old` |

## Multi-Stage Build Stages

The image uses at least 3 external build stages:

1. **uv-stage** — provides `/uv` (44.4 MB) and `/uvx` (393 KB) from `ghcr.io/astral-sh/uv`
2. **python-server-builder** — provides pre-built `.whl` files (~379 KB)
3. **python-server-requirements** — provides `requirements.txt` (~12 KB)
4. **node-runtime-builder** — provides `node_modules` (~157 MB)

## Software Inventory

### Languages & Runtimes
- **Python:** 3.10 (system), 3.11, 3.12 (standalone builds from astral-sh)
- **Node.js:** 20, 22 (default), 24 via fnm v1.38.1
- **Go:** 1.22.8
- **Bun:** 1.3.3

### Package Managers
- pip, pip3.11, pip3.12
- npm, npx, yarn 1.22.22
- uv (astral-sh)

### Development Tools
- code-server 4.104.0 (VS Code in browser)
- JupyterLab 4.4.5 (with Python 3.10/3.11/3.12 kernels)
- opencode CLI
- git, gh (GitHub CLI)
- gcc, g++, make, cmake, ninja-build
- ripgrep, jq, tmux, vim, nano

### Browser & GUI
- Chromium 146.0.7680.31
- agent-browser 0.22.3
- @agent-infra/mcp-server-browser 1.2.29
- chrome-devtools-mcp 0.9.0
- TigerVNC + noVNC 1.4.0
- openbox (window manager)
- fcitx5 (CJK IME)

### Networking & Proxy
- nginx (reverse proxy / gateway)
- gost v3.0.0-rc10 (proxy)
- websocat v1.13.0 (WebSocket CLI)
- nmap, netcat, telnet, lsof

### Media
- ffmpeg
- imagemagick
- yt-dlp

### Other
- Supervisor (process manager, from git commit)
- AIO CLI (custom sandbox management CLI)
- Volcengine CLI tools

## Key Paths

| Path | Purpose |
|------|---------|
| `/opt/gem/` | Main config/scripts directory |
| `/opt/gem/run.sh` | Container entrypoint |
| `/opt/gem/supervisord.conf` | Process manager config |
| `/opt/browser/chrome` | Chromium binary |
| `/opt/novnc/` | noVNC web client |
| `/opt/python3.11/`, `/opt/python3.12/` | Standalone Python installs |
| `/opt/nodejs/` | Node.js versions (symlinks to fnm) |
| `/opt/fnm/` | fnm node version manager data |
| `/opt/jupyter/` | JupyterLab runtime/data |
| `/opt/repl-servers/nodejs/` | Node.js REPL server |
| `/opt/runtime/nodejs/` | Node.js runtime modules |
| `/opt/skills/` | AIO CLI skills |
| `/var/www/app/static/sandbox/` | Static web assets |
| `/usr/local/code-server-4.104.0/` | code-server installation |

## Ports

| Port | Service |
|------|---------|
| 8080 | Public gateway (nginx) |
| 8081 | Auth backend |
| 8091 | Sandbox server (python-server) |
| 8092 | Node.js REPL (v22) |
| 8100 | MCP browser server |
| 8118 | Tinyproxy |
| 8192 | Node.js REPL (v20) |
| 8200 | code-server |
| 8392 | Node.js REPL (v24) |
| 8888 | JupyterLab |
| 4096 | opencode |
| 5900 | VNC server |
| 6080 | WebSocket proxy (noVNC) |
| 9222 | Chrome DevTools Protocol |
