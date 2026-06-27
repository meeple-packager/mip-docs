---
title: "mip server — web dashboard for MIP"
description: "The server command starts a local web dashboard that displays real-time information about your project, packages, global store, registries, scripts, and system health."
---

# mip server

::: callout info "What is this?" icon:🌐
`mip server` launches a beautiful, real-time web dashboard for your MIP project. It provides a visual overview of your project's dependencies, global storage, configured registries, available scripts, and system information — all in one place with auto-refresh.
:::

## Syntax

```bash
mip server <subcommand> [options]
```

## Subcommands

| Subcommand | Description |
|------------|-------------|
| `start [port]` | Start the web server (default port: 3000) |
| `stop` | Stop the running server |
| `status` | Show server status (running/stopped) |

## Options

| Flag | Description |
|------|-------------|
| `--open`, `-o` | Automatically open the dashboard in your browser |
| `--host` | Listen on all network interfaces (0.0.0.0) instead of just localhost |

## Dashboard Features

### 📊 Overview Tab
- **Package statistics** — total installed packages
- **Store size** — global storage usage
- **Dependency breakdown** — production vs development
- **Script count** — available npm scripts
- **Registry count** — configured registries
- **Workspace count** — monorepo workspaces
- **Top 5 packages** — largest packages by size
- **Registry quick view** — all configured registries with status

### 📦 Packages Tab
- Complete list of all installed packages
- Package name, version, size, and path
- Status indicator (exists/missing in global store)
- Sortable by size (largest first)

### 💾 Store Tab
- Global store total size
- Number of packages in the store
- Store path location
- Top 50 packages by size in the global cache

### 🔗 Registries Tab
- All configured registries with URLs
- Token status (set/missing)
- Default registry indicator
- Built-in registry (npm) always shown

### 📜 Scripts Tab
- All npm scripts defined in `mip.yml` or `mip.json`
- Script names and commands
- Quick reference for available automation

### ⚙️ System Tab
- Node.js version
- MIP version
- Platform and architecture
- Memory usage (free/total)
- CPU cores count
- System uptime
- Load average
- Current working directory
- Home directory

## How it works

1. **Server process** — MIP starts a lightweight HTTP server using Node.js `http` module.

2. **API endpoints** — the server exposes RESTful endpoints:
   - `/api/project` — project configuration
   - `/api/packages` — installed packages
   - `/api/store` — global storage info
   - `/api/registries` — registry configuration
   - `/api/system` — system information
   - `/api/health` — health check

3. **Dashboard UI** — a single-page application with tabs that fetches data from the API every 3 seconds.

4. **Process management** — server PID is stored in `~/.mip/server.pid` for process control.

5. **Auto-refresh** — the dashboard automatically updates every 3 seconds, keeping information current.

## Examples

::: tabs
== tab "Start server"
```bash
mip server start
```

**Output:**
```
🌐 MIP Server running at http://localhost:3000
📊 Dashboard: http://localhost:3000/
📁 Project: /home/user/my-project
🔧 Press Ctrl+C to stop
```

Open your browser and visit `http://localhost:3000` to see the dashboard.
:::

== tab "Start on custom port"
```bash
mip server start 8080
```
Starts the server on port 8080 instead of the default 3000.
:::

== tab "Start with browser"
```bash
mip server start --open
```
Starts the server and automatically opens the dashboard in your default browser.
:::

== tab "Start on all interfaces"
```bash
mip server start --host
```
Allows access from other devices on the network (useful for sharing the dashboard with team members).
:::

== tab "Stop server"
```bash
mip server stop
```

**Output:**
```
✅ Server stopped (PID: 12345)
```
:::

== tab "Check status"
```bash
mip server status
```

**Output:**
```
✅ Server is running (PID: 12345)
   To stop: mip server stop
```

Or if not running:
```
ℹ️ Server is not running
```
:::
:::

## Dashboard Preview

```
📊 MIP Dashboard

📦 Packages: 17
💾 Store Size: 273 MB
📋 Dependencies: 17 (6 prod · 11 dev)
📜 Scripts: 13
🔗 Registries: 2
📁 Workspaces: 0

┌─────────────────────────────────────────────────────────────┐
│  Overview  │  Packages  │  Store  │  Registries  │  Scripts │
├─────────────────────────────────────────────────────────────┤
│  Top 5 Packages:                    │  Registries:          │
│  ├─ lodash    4.17.21  2.3 MB      │  ├─ npm (built-in)   │
│  ├─ express   4.18.2   1.8 MB      │  └─ github (custom)  │
│  ├─ axios     1.6.0    1.2 MB      │                       │
│  ├─ react     18.2.0   850 KB      │                       │
│  └─ vue       3.3.0    720 KB      │                       │
└─────────────────────────────────────────────────────────────┘
```

## Common errors

::: callout warning "❌ ERR_PORT_IN_USE" icon:alert-triangle
**Cause:** Port 3000 (or the specified port) is already in use.

**Resolution:**
1. Use a different port: `mip server start 8080`
2. Stop the existing server: `mip server stop`
3. Or kill the process manually using the PID shown in the error message.
:::

::: callout danger "❌ ERR_SERVER_ALREADY_RUNNING" icon:skull
**Cause:** The server is already running.

**Resolution:**
1. Check the status: `mip server status`
2. Stop it first: `mip server stop`
3. Then start again: `mip server start`
:::

::: callout warning "⚠️ ERR_NO_CONFIG" icon:alert-circle
**Cause:** No `mip.yml` or `mip.json` found in the current directory.

**Resolution:**
1. Make sure you're in a MIP project directory.
2. Run `mip init` to create a new project.
3. Or navigate to a project directory with a config file.
:::

## Related commands

- [mip doctor](/commands/doctor) — system diagnostics
- [mip list](/commands/list) — list installed packages (CLI)
- [mip config](/commands/config) — manage configuration
- [mip registry](/commands/registry) — manage registries

::: collapsible "🔧 How the server works under the hood"
The server is a single-file implementation using Node.js's built-in `http` module (no external dependencies).

**Process flow:**
1. When `mip server start` is called, MIP:
   - Checks if the server is already running (using `~/.mip/server.pid`)
   - Creates an HTTP server
   - Listens on the specified port
   - Writes the PID to `~/.mip/server.pid`
   - Optional: opens the browser

2. **API responses** are generated on-demand by reading:
   - Project configuration from `mip.yml` or `mip.json`
   - Package manifest from `.mip/manifest.json`
   - Global store from `~/.mip/store/`
   - System information from Node.js `os` module

3. **Dashboard** is served as a single HTML file with embedded CSS and JavaScript. The JavaScript:
   - Fetches data from API endpoints every 3 seconds
   - Renders the data in tabbed interface
   - Provides keyboard shortcuts (Ctrl+R to refresh)

4. **Graceful shutdown** — the server handles SIGINT and SIGTERM signals to clean up the PID file.
:::

::: callout tip "💡 Pro tip: Share with your team" icon:lightbulb
Use `mip server start --host` to allow other devices on your network to access the dashboard. The server will listen on all interfaces, and you can share your local IP address with team members:

```bash
# Start on all interfaces
mip server start --host

# Your team can access at:
# http://192.168.1.100:3000  (replace with your IP)
```
:::
