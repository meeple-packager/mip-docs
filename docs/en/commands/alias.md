---
title: "mip alias - command shortcuts"
description: "The alias command allows you to create, list, and remove custom shortcuts for frequently used MIP commands."
---

# mip alias

::: callout info "What is this?" icon:⚡
`mip alias` allows you to create custom shortcuts for MIP commands. Stop typing long commands - use your own abbreviations and speed up your workflow.
:::

## Syntax

```bash
mip alias <subcommand> [options]
```

## Subcommands

| Subcommand | Description |
|------------|-------------|
| `set <name> <command>` | Create or update an alias |
| `remove <name>` | Remove an alias |
| `list` | Show all configured aliases |
| `clear` | Remove all aliases |
| `help` | Show this help |

## How it works

1. **Aliases are stored** in `~/.mip/aliases.yml` (global, not project-specific)
2. **Resolution** happens before command execution - MIP checks if the command matches any alias
3. **Supports spaces** in alias names (use quotes)
4. **Supports arguments** - everything after the alias is passed to the target command
5. **Longest match** - if multiple aliases match, the longest one is used

## Examples

::: tabs
== tab "Basic alias"
```bash
# Create shortcut for install
mip alias set i install

# Use it
mip i lodash
```

**Output:**
```
✅ Alias set: i → install
   Stored in: ~/.mip/aliases.yml
```
:::

== tab "Alias with spaces"
```bash
# Create alias with spaces in name
mip alias set "mip pe calc" "mip run calc"

# Use it
mip pe calc 2 plus 2
```

**Output:**
```
🧮 2 + 2 = 4
```
:::

== tab "Alias with arguments"
```bash
# Create alias for build with flags
mip alias set "mip build fast" "mip run build --mode production"

# Use it
mip build fast
# → mip run build --mode production
```
:::

== tab "List all aliases"
```bash
mip alias list
```

**Output:**
```
📋 MIP Aliases:

  i          →  install
  up         →  update
  "mip pe calc"  →  mip run calc

💡 Total: 3 aliases
📁 File: ~/.mip/aliases.yml
```
:::

== tab "Remove alias"
```bash
# Remove by name
mip alias remove i
```

**Output:**
```
✅ Alias removed: i (was → install)
```

Remove alias with spaces:
```bash
mip alias remove "mip pe calc"
# or
mip alias remove mip pe calc
```
:::

== tab "Clear all aliases"
```bash
mip alias clear
```

**Output:**
```
⚠️ This will remove all 3 aliases
   Are you sure? (y/N) y
✅ Cleared all 3 aliases
```
:::
:::

## Aliases file format

Aliases are stored in `~/.mip/aliases.yml`:

```yaml
i: install
up: update
"mip pe calc": "mip run calc"
"mip build fast": "mip run build --mode production"
```

You can edit this file directly - MIP will load changes on the next run.

## Common use cases

### Shorten everyday commands
```bash
mip alias set i install
mip alias set up update
mip alias set ls list
mip alias set rm uninstall
mip alias set s search
mip alias set st status
mip alias set ci "mip ci --frozen-lockfile"
```

### Shortcuts for plugin commands
```bash
mip alias set "build" "mip pe mip-webpack build"
mip alias set "dev" "mip pe mip-webpack dev"
```

### Complex commands
```bash
# Deploy with multiple steps
mip alias set "deploy" "mip run build && mip run test && mip run deploy"

# Clean install
mip alias set "fresh" "mip cache clean && mip install"
```

## Common errors

::: callout warning "❌ ERR_ALIAS_NOT_FOUND" icon:alert-triangle
**Cause:** The specified alias doesn't exist.

**Resolution:**
1. Check the exact name: `mip alias list`
2. Aliases with spaces need quotes: `mip alias remove "my alias"`
3. Make sure you typed the name correctly
:::

::: callout danger "❌ ERR_RESERVED_NAME" icon:skull
**Cause:** The alias name conflicts with a reserved MIP command.

**Resolution:**
1. Reserved names cannot be aliased: `alias`, `config`, `registry`, `server`, `publish`, `help`
2. Choose a different name for your alias
:::

::: callout warning "⚠️ ERR_ALIAS_OVERWRITE" icon:alert-circle
**Cause:** An alias with the same name already exists.

**Resolution:**
1. You'll be prompted to confirm overwrite
2. Type `y` to overwrite, `n` to cancel
3. Or use `mip alias remove <name>` first, then set it again
:::

## How alias resolution works

When you run a command, MIP checks:

1. **Exact match** - if the entire command matches an alias name
2. **Longest match** - if multiple aliases match, the longest one wins
3. **Argument passing** - remaining arguments are passed to the target command

**Example:**
```
Aliases:
  "build" → "mip run build"
  "build fast" → "mip run build --production"

Input: mip build fast --force
Match: "build fast" (longest match)
Result: mip run build --production --force
```

## Related commands

- [mip config](/commands/config) - manage MIP configuration
- [mip plugin](/commands/plugin) - manage plugins (which often provide additional commands)
- [mip pe](/commands/pe) - execute plugin commands (often used in aliases)

::: collapsible "🔧 Aliases vs Shell Aliases"
**MIP aliases** work inside MIP commands and are portable across any shell.

**Shell aliases** (in `.bashrc` or `.zshrc`) work at the shell level:
```bash
# Shell alias (in .zshrc)
alias mip='mip --super'

# MIP alias (stored in ~/.mip/aliases.yml)
mip alias set i install
```

**When to use each:**
- Shell aliases - for global MIP flags (`--super`, `--debug`)
- MIP aliases - for command substitutions (`i` → `install`)
:::

::: callout tip "💡 Pro tip: Share aliases" icon:lightbulb
Copy your `~/.mip/aliases.yml` file to other machines to keep your shortcuts consistent across all your development environments.
:::
