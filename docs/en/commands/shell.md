---
title: "mip shell - interactive package manager shell"
description: "The shell command starts an interactive REPL shell for MIP, allowing you to run multiple package manager commands without typing 'mip' each time, with autocomplete, command history, and built-in navigation."
---

# mip shell

::: callout info "What is this?" icon:terminal
`mip shell` launches an interactive terminal shell for MIP. Inside the shell, you can run any MIP command without the `mip` prefix, use built-in shell commands like `cd` and `pwd`, get tab autocompletion, navigate command history, and more. It's like a dedicated mini-shell for package management.
:::

## Syntax

```bash
mip shell
```

Alias:

```bash
mip sh
```

## Options

The `shell` command has no command-line options. All configuration is done through the interactive session.

## Built-in Shell Commands

Once inside the MIP shell, the following built-in commands are available:

| Command | Description |
|---------|-------------|
| `cd <dir>` | Change current working directory |
| `pwd` | Print current working directory path |
| `clear` | Clear the screen and redisplay the logo |
| `exit` / `quit` | Exit the interactive shell |
| `help` | Display the full help menu with all available commands |
| `version` | Display the current MIP version |
| `shell` | Inform that you are already in the shell |

## MIP Commands

All standard MIP commands work inside the shell without the `mip` prefix:

**Core:**
- `install` / `i <pkg>` — install packages
- `uninstall` / `rm <pkg>` — remove packages
- `list` / `ls` — show installed packages
- `update` / `up` — update packages
- `search <q>` — search the registry
- `info <pkg>` — package details
- `init` — initialize a new project
- `run <script>` — run a script
- `exec <cmd>` — execute a binary

**Info & Security:**
- `outdated` — show outdated packages
- `audit` — security audit
- `doctor` — system diagnostics
- `why <pkg>` — why is this package installed?
- `feel` — project vibe check
- `hello` — system information

**Management:**
- `cache` — cache commands
- `config` — manage configuration
- `alias` — manage aliases
- `registry` — manage registries
- `dedupe` — deduplicate dependencies
- `ci` — CI installation
- `genlock` — generate lockfile
- `workspaces` — workspace commands
- `bundle` — bundle packages
- `clone` — clone a project
- `publish` — publish a package
- `plugin` — manage plugins
- `exports` — export packages
- `language` — change interface language
- `global` — manage global packages

## Features

### Tab Autocompletion
Press `Tab` to autocomplete:
- **Command names** — type `in` + `Tab` → `install`
- **Package names** — when a command expects a package (e.g., `install`, `uninstall`, `info`), it autocompletes from the installed packages manifest

### Command History
- **Up arrow (↑)** — navigate to previous commands
- **Down arrow (↓)** — navigate to next commands
- History is persisted to `~/.mip/shell-history.json` (up to 500 entries)

### Keyboard Shortcuts
| Shortcut | Action |
|----------|--------|
| `↑` | Previous command |
| `↓` | Next command |
| `Tab` | Autocomplete |
| `Ctrl+C` | Exit shell |
| `Ctrl+L` | Clear screen (keeps prompt) |

### Smart `mip` Prefix Handling
If you type a command prefixed with `mip` (e.g., `mip install express`), the shell detects this common mistake, shows a friendly warning, and automatically executes the command without the prefix.

### Welcome Screen
When the shell starts, it displays:
- MIP ASCII logo
- Version number
- Current project name
- Number of installed packages
- Current language

### Prompt
The prompt shows:
```
mip project-name ❯
```

## History File

Command history is saved to `~/.mip/shell-history.json`. It stores up to 500 most recent commands and persists across shell sessions.

## Examples

::: tabs
== tab "Start the shell"
```bash
mip shell
```

**Output:**
```
  ╔══════════════════════════════╗
  ║  ███╗   ███╗██╗██████╗       ║
  ║  ████╗ ████║██║██╔══██╗      ║
  ║  ██╔████╔██║██║██████╔╝      ║
  ║  ██║╚██╔╝██║██║██╔═══╝       ║
  ║  ██║ ╚═╝ ██║██║██║           ║
  ║  ╚═╝     ╚═╝╚═╝╚═╝           ║
  ╚══════════════════════════════╝

  MIP Interactive Shell v1.0.0
  Type help for commands, exit to quit

  📁 my-project
  📦 17 packages
  🌐 en

  mip my-project ❯
```
:::

== tab "Working in the shell"
```bash
# Inside the shell, without 'mip' prefix:
mip my-project ❯ install express

mip my-project ❯ list
# Shows installed packages

mip my-project ❯ cd ../other-project

mip other-project ❯ pwd
# /home/user/other-project

mip other-project ❯ help
# Shows full help menu

mip other-project ❯ exit
# ⚓ Fair winds!
```
:::

== tab "Tab completion"
```bash
mip my-project ❯ in[Tab] → install
mip my-project ❯ install lo[Tab] → lodash
```
:::

== tab "Command history"
```bash
# Press ↑ to recall previous commands
mip my-project ❯ ↑
mip my-project ❯ install express
```
:::

== tab "mip prefix handling"
```bash
mip my-project ❯ mip install express
# ⚠️ You are in shell, there is no need to write mip <command>
#    Try: install express
# (automatically executes install express)
```
:::

== tab "Clear screen"
```bash
mip my-project ❯ clear
# Clears the screen and shows the logo again
```
:::

== tab "Exit the shell"
```bash
mip my-project ❯ exit
# ⚓ Fair winds!
```

Or press `Ctrl+C`:
```bash
mip my-project ❯ ^C
# Bye!
```
:::
:::

## Common Mistakes

::: callout warning "❌ You are in shell, not in CLI" icon:alert-triangle
**Problem:** Typing `mip <command>` inside the shell (e.g., `mip install lodash`).

**What happens:** The shell detects this common mistake, displays a warning, and automatically strips the `mip` prefix and executes the command.

**Tip:** Inside the shell, just type the command directly: `install lodash`.
:::

::: callout warning "❌ Already in shell" icon:alert-circle
**Problem:** Typing `mip shell` when already inside the shell.

**What happens:** The shell responds with "⚠️ Already in shell!" and remains in shell mode.
:::

::: callout warning "❌ Unknown command" icon:alert-circle
**Problem:** Typing a command that doesn't exist.

**What happens:** The shell shows "❌ Unknown command: <cmd>" and suggests using `help` to see available commands.
:::

::: callout warning "❌ cd to invalid directory" icon:alert-triangle
**Problem:** Trying to `cd` to a directory that doesn't exist or is inaccessible.

**What happens:** The shell shows "❌ cd: <error message>" and stays in the current directory.
:::

::: callout tip "💡 Tip" icon:lightbulb
- The shell is great for batch operations — you can run multiple commands in sequence without typing `mip` each time.
- Use `cd` to navigate between different MIP projects while staying in the same shell session.
- The command history persists across sessions, so your most-used commands are always available.
- If you have many packages installed, tab completion for package names saves a lot of typing.
:::

## Related commands

- [mip hello](/commands/hello) — system information TUI
- [mip list](/commands/list) — list installed packages
- [mip install](/commands/install) — install packages
- [mip exec](/commands/exec) — execute binaries
