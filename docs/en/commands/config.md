---
title: "mip config — manage MIP configuration"
description: "The config command allows you to view, modify, and manage MIP project configuration directly from the command line."
---

# mip config

::: callout info "What is this?" icon:gear
`mip config` provides a convenient CLI interface for managing your MIP project configuration. Instead of manually editing `mip.yml` or `mip.json`, you can view, set, delete, and reset configuration values directly from the terminal. Perfect for quick tweaks, CI/CD automation, and scripting.
:::

## Syntax

```bash
mip config <subcommand> [options]
```

## Subcommands

| Subcommand | Description |
|------------|-------------|
| `list` | Show all configuration in a formatted output |
| `get <key>` | Get a specific configuration value |
| `set <key> <value>` | Set a configuration value |
| `delete <key>` | Delete a configuration value |
| `reset` | Reset configuration to defaults |
| `edit` | Open configuration in your editor |

## Options

| Flag | Description |
|------|-------------|
| `--json` | Output in JSON format |
| `--yaml` | Output in YAML format |

## How it works

1. **Configuration detection** — MIP automatically detects your config file (`mip.yml`, `mip.json`, or `package.json`).

2. **Value resolution** — supports nested keys using dot notation (e.g., `registries.github.url`).

3. **Type detection** — values are automatically parsed (strings, numbers, JSON objects, arrays).

4. **File migration** — if you're using a legacy config format, MIP will automatically migrate to `mip.yml`.

5. **Editor integration** — uses `$EDITOR` or `$VISUAL` environment variables for the `edit` subcommand.

## Examples

::: tabs
== tab "View all configuration"
```bash
mip config list
```

**Output:**
```
📋 MIP Configuration

   File: mip.yml
   Path: /home/user/my-project/mip.yml

📦 Project:
   name: my-awesome-app
   version: 2.0.0

📦 Dependencies:
   dependencies: 17 package(s)
   devDependencies: 5 package(s)

📜 Scripts:
   build: webpack --mode production
   dev: webpack serve
   test: jest

📦 Registries:
   default: npm
   github:
     url: https://npm.pkg.github.com/
     token: ✓ ghp_xxx…

💡 Use "mip config get <key>" to view specific values
   "mip config set <key> <value>" to update values
```
:::

== tab "Get a specific value"
```bash
mip config get version
```
**Output:**
```
2.0.0
```

```bash
mip config get scripts.build
```
**Output:**
```
webpack --mode production
```

```bash
mip config get registries.github
```
**Output:**
```
{
  "url": "https://npm.pkg.github.com/",
  "token": "ghp_xxx"
}
```
:::

== tab "Set a value"
```bash
mip config set version 2.1.0
```
**Output:**
```
✅ Config updated: version
   Old: "2.0.0"
   New: "2.1.0"
```

```bash
mip config set defaultRegistry github
```
**Output:**
```
✅ Config updated: defaultRegistry
   Old: "npm"
   New: "github"
```

```bash
mip config set registries.github.token ghp_new_token
```
**Output:**
```
✅ Config updated: registries.github.token
   Old: "ghp_xxx"
   New: "ghp_new_token"
```
:::

== tab "Delete a value"
```bash
mip config delete registries.github.token
```
**Output:**
```
✅ Deleted: registries.github.token
   Old value: "ghp_xxx"
```
:::

== tab "Edit in editor"
```bash
mip config edit
```
**Output:**
```
📝 Opening mip.yml in vim...
✅ Config edited successfully
```
:::

== tab "Reset to defaults"
```bash
mip config reset
```
**Output:**
```
⚠️ This will reset all configuration to default values
   All custom settings will be lost!
   Are you sure? (yes/no) yes
✅ Config reset to defaults
```
:::
:::

## Common use cases

### Quick version bump
```bash
# Before publishing
mip config set version 2.1.0
mip config get version  # Verify
```

### Registry management
```bash
# Set default registry
mip config set defaultRegistry github

# Update registry token
mip config set registries.github.token ghp_new_token

# Add a new registry
mip config set registries.gitlab.url https://gitlab.com/api/v4/packages/npm/
```

### Script management
```bash
# Add a new script
mip config set scripts.deploy "npm run build && rsync -avz dist/ server:/app"

# Update an existing script
mip config set scripts.test "jest --coverage"
```

### Workspace management
```bash
# Set workspaces
mip config set workspaces '["packages/*", "apps/*"]'
```

## Supported keys

| Key | Type | Description |
|-----|------|-------------|
| `name` | string | Project name |
| `version` | string | Project version |
| `description` | string | Project description |
| `dependencies` | object | Production dependencies |
| `devDependencies` | object | Development dependencies |
| `scripts` | object | NPM scripts |
| `workspaces` | array | Monorepo workspaces |
| `registries` | object | Package registries |
| `defaultRegistry` | string | Default registry name |
| `language` | string | UI language (en/ru/etc) |

### Nested keys
You can access nested values using dot notation:

```bash
mip config get registries.github.url
mip config set registries.github.token ghp_xxx
mip config delete registries.github.token
```

## Output formats

### JSON format
```bash
mip config list --json
```
Useful for scripting and programmatic access.

### YAML format
```bash
mip config list --yaml
```
Shows the raw YAML configuration.

## Common errors

::: callout warning "❌ ERR_NO_CONFIG" icon:alert-triangle
**Cause:** No config file found in the current directory.

**Resolution:**
1. Make sure you're in a MIP project directory.
2. Run `mip init` to create a new project.
3. Or run `mip config` from the project root.
:::

::: callout danger "❌ ERR_KEY_NOT_FOUND" icon:skull
**Cause:** The specified key doesn't exist in the configuration.

**Resolution:**
1. Check the key name: `mip config list` to see all keys.
2. Use dot notation for nested keys: `registries.github.url`.
3. Make sure the key exists before accessing it.
:::

::: callout warning "⚠️ ERR_INVALID_VALUE" icon:alert-circle
**Cause:** The provided value has an invalid format.

**Resolution:**
1. For objects, use valid JSON: `'{"key":"value"}'`
2. For arrays, use valid JSON: `'["item1","item2"]'`
3. For strings, no quotes needed: `mip config set name my-project`
:::

## Related commands

- [mip registry](/commands/registry) — manage package registries
- [mip init](/commands/init) — create a new project
- [mip doctor](/commands/doctor) — check project health

::: collapsible "🔧 How configuration resolution works"
MIP follows a specific order when resolving configuration:

1. **`mip.yml`** — primary configuration file (recommended)
2. **`mip.json`** — legacy JSON format (will be migrated)
3. **`package.json`** — npm compatibility (will be migrated)

When you run `mip config set`, MIP automatically:
- Detects the active config file
- Updates the value
- Saves back to `mip.yml`

If your project uses `package.json` or `mip.json`, MIP will automatically migrate to `mip.yml` on the first write operation.
:::

::: callout tip "💡 Pro tip: Configuration as code" icon:lightbulb
Use `mip config` in your CI/CD pipelines for dynamic configuration:

```bash
# Set environment-specific config
mip config set apiUrl https://api.production.com
mip config set logLevel info

# Then run your build
mip run build
```
:::
