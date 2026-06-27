---
title: "mip registry — manage package registries"
description: "The registry command allows you to add, remove, list, and manage custom package registries for private packages and alternative package sources."
---

# mip registry

::: callout info "What is this?" icon:package
`mip registry` manages custom package registries (GitHub Packages, GitLab Registry, Verdaccio, JFrog Artifactory, and others). This allows you to install private packages or use alternative package sources without losing compatibility with the default npm registry.
:::

## Syntax

```bash
mip registry <subcommand> [options]
```

## Subcommands

| Subcommand | Description |
|------------|-------------|
| `add <name> <url> [--token TOKEN]` | Add a new registry with optional authentication token |
| `remove <name>` | Remove an existing registry |
| `list` | List all configured registries |
| `set-default <name>` | Set the default registry for package installation |

## Options

| Flag | Description |
|------|-------------|
| `--token TOKEN` | Authentication token for private registries (GitHub, GitLab, etc.) |

## How it works

1. **Configuration storage** — registries are stored in the project's `mip.yml` or `mip.json` file in the `registries` section.

2. **Default registry** — if not specified, `npm` is always the default registry. You can change this with `set-default`.

3. **Registry resolution** — when installing packages, MIP checks:
   - Is a registry explicitly specified in the package name? (`package@registry:version`)
   - If not, use the default registry (`npm` by default)
   - The npm registry always works as a fallback

4. **Authentication** — tokens are stored securely in the config and used for authentication with private registries.

## Examples

::: tabs
== tab "Add GitHub Packages"
```bash
mip registry add github https://npm.pkg.github.com/ --token ghp_xxx
```
This adds GitHub Packages registry with authentication.

**Output:**
```
✅ Registry "github" added successfully
   📦 URL: https://npm.pkg.github.com/
   🔑 Token: ghp_xxx... (set)
   📁 Config: mip.yml
```
:::

== tab "Add GitLab Registry"
```bash
mip registry add gitlab https://gitlab.com/api/v4/packages/npm/
```
**Output:**
```
✅ Registry "gitlab" added successfully
   📦 URL: https://gitlab.com/api/v4/packages/npm/
   🔑 Token: not set
   📁 Config: mip.yml
```
:::

== tab "List all registries"
```bash
mip registry list
```
**Output:**
```
📦 Configured registries:

  ✓ npm (built-in)
    URL: https://registry.npmjs.org/
    ★ DEFAULT

  ★ github
    URL: https://npm.pkg.github.com/
    Token: ✓ ghp_xxx…
    Added: 6/27/2026
    ★ DEFAULT REGISTRY

📌 Default registry: github

💡 To use a registry for a specific package:
   mip install <package>@<registry>:<version>
   Example: mip install lodash@github:4.17.21
```
:::

== tab "Set default registry"
```bash
mip registry set-default github
```
**Output:**
```
✅ Default registry set to "github"
   📦 URL: https://npm.pkg.github.com/
```
:::

== tab "Remove a registry"
```bash
mip registry remove github
```
**Output:**
```
🗑️ Removing registry "github"...
   URL: https://npm.pkg.github.com/
✅ Registry "github" removed successfully
```
:::
:::

## Using private registries

After adding a registry, you can install packages from it:

### Default registry
If you set a registry as default, all packages are installed from it:

```bash
# Uses the default registry (github)
mip install lodash

# Explicitly use a specific registry
mip install lodash@npm:4.17.21
```

### Specific registry for a package
```bash
# Install from GitHub Packages
mip install @myorg/secret-package@github:1.0.0

# Install from GitLab Registry
mip install @mygroup/private-package@gitlab:2.1.0
```

### Registry format
```
<package>@<registry>:<version>
```
- `<package>` — package name
- `<registry>` — registry name (as added)
- `<version>` — package version (optional)

## Common errors

::: callout warning "❌ ERR_REGISTRY_NOT_FOUND" icon:alert-triangle
**Cause:** The specified registry doesn't exist in the configuration.

**Resolution:**
1. Check the registry name: `mip registry list`
2. Add the registry first: `mip registry add <name> <url>`
3. Try again with the correct registry name.
:::

::: callout danger "❌ ERR_INVALID_URL" icon:skull
**Cause:** The provided URL is not a valid HTTP/HTTPS URL.

**Resolution:**
1. Make sure the URL starts with `http://` or `https://`.
2. Check for typos in the URL.
3. Example of a valid URL: `https://npm.pkg.github.com/`
:::

::: callout warning "⚠️ ERR_TOKEN_REQUIRED" icon:alert-circle
**Cause:** The registry requires authentication but no token was provided.

**Resolution:**
1. Add the registry with `--token` flag:
   ```bash
   mip registry add github https://npm.pkg.github.com/ --token ghp_xxx
   ```
2. If the registry is already added, remove and re-add with token.
:::

::: callout info "🔒 Reserved registry names" icon:info
The following names are reserved and cannot be used for custom registries:
- `npm` — built-in npm registry
- `default` — reserved for internal use
- `registry` — reserved for internal use
:::

## Configuration format

Registries are stored in `mip.yml` or `mip.json`:

```yaml
# mip.yml
registries:
  github:
    url: https://npm.pkg.github.com/
    token: ghp_xxx
    added: 2026-06-27T10:00:00.000Z
  gitlab:
    url: https://gitlab.com/api/v4/packages/npm/
    token: glpat-xxx
    added: 2026-06-27T10:30:00.000Z
defaultRegistry: github
```

### Configuration fields

| Field | Type | Description |
|-------|------|-------------|
| `url` | string | Base URL of the registry |
| `token` | string\|null | Authentication token (stored as plaintext) |
| `added` | string | ISO timestamp of when the registry was added |
| `defaultRegistry` | string | Name of the default registry |

::: callout warning "Token security" icon:shield
Tokens are stored in plaintext in the config file. Make sure to:
- **Never** commit the config file to public repositories.
- Use environment variables for sensitive tokens (recommended).
- Consider using `.gitignore` to exclude config files with tokens.
:::

## Supported registries

| Registry | Example URL | Authentication |
|----------|-------------|----------------|
| **npm** | `https://registry.npmjs.org/` | Not required |
| **GitHub Packages** | `https://npm.pkg.github.com/` | GitHub token (`ghp_xxx`) |
| **GitLab Registry** | `https://gitlab.com/api/v4/packages/npm/` | GitLab token (`glpat-xxx`) |
| **Verdaccio** | `http://localhost:4873/` | Optional |
| **JFrog Artifactory** | `https://<company>.jfrog.io/artifactory/api/npm/npm-local/` | JFrog token |
| **Azure Artifacts** | `https://pkgs.dev.azure.com/<org>/_packaging/<feed>/npm/registry/` | Personal Access Token |

## Related commands

- [mip install](/commands/install) — install packages from configured registries
- [mip doctor](/commands/doctor) — checks project health (including registry configuration)
- [mip config](/commands/config) — view and edit configuration

::: collapsible "🔧 How registry resolution works"
When you run `mip install <package>`:

1. **Parse package spec** — check if package contains a registry prefix (`package@registry:version`).
2. **Resolve registry** — if a registry is explicitly specified, use it.
3. **Default fallback** — if no registry is specified, use the default registry from config.
4. **npm fallback** — if the default registry fails, fall back to npm registry.
5. **Authentication** — add the token from config as `Authorization` header.
6. **Fetch package** — request package metadata from the registry.
7. **Install** — download and install the package.

This ensures maximum compatibility and flexibility when working with multiple package sources.
:::
