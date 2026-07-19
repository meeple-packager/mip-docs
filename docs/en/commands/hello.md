---
title: "mip hello — system info"
description: "The hello command starts an interactive TUI that displays system/project information and shows quick access to some MIP commands."
---

# mip hello

::: callout info "What is this?" icon:cpu
`mip hello` starts an interactive terminal UI (TUI) that shows:
- **System** info (Node version, OS, kernel, architecture)
- **Hardware** info (CPU model, cores, RAM, uptime)
- **Project** info (detected config file, number of deps, whether workspaces exist)
- **Quick Commands** (a small helper list; can be extended via features)
:::

## Syntax

```bash
mip hello
```

Alias:

```bash
mip h
```

## What you see

The TUI refreshes periodically (see features below) and can also include a list of quick/custom commands depending on feature flags.

## How to exit

Press:
- `Ctrl+C`
- `Esc`
- `q`

## Feature flags (configurable via mip-features)

This command is controlled by feature flags loaded from the current project context.

| Flag | Meaning |
|------|---------|
| `hello.enabled` | Disable the command (when `false`) |
| `hello.darkTheme` | Use dark theme (when not `false`) |
| `hello.showCommands` | Show the "Quick Commands" block (when not `false`) |
| `hello.customCommands` | Extra commands to display in the "Custom Commands" block (array of strings) |
| `hello.refreshInterval` | Refresh interval in ms (default: `5000`) |

## Examples

::: tabs
== tab "Run hello"
```bash
mip hello
```
:::
== tab "Run via alias"
```bash
mip h
```
:::
:::

## Related commands

- [mip --help](/commands/why) — full CLI help entry point
- [mip doctor](/commands/doctor) — system health checks
- [mip feel](/commands/feel) — project status overview

