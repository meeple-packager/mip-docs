# MIP Features Documentation

## Overview

MIP 2.1 introduces a powerful features system that allows you to control almost every aspect of the package manager's behavior. Each feature can be enabled or disabled individually through a configuration file or CLI commands.

## Quick Start

```bash
# Generate default configuration with all features
mip --genconfig

# View all available features
mip --list-features

# Enable a feature
mip config set features.install.parallel true

# Disable a feature
mip config set features.update.checkForUpdates false

# Disable ALL features at once
mip config set features.config.enabled false
```

## Feature Categories

### Global

| Feature | Default | Description |
|---------|---------|-------------|
| `config.enabled` | `true` | Master switch - disables ALL feature modifications when set to `false` |

---

### Install & Update

| Feature | Default | Description |
|---------|---------|-------------|
| `install.parallel` | `true` | Install multiple packages in parallel for faster execution |
| `install.forceReinstall` | `false` | Always reinstall packages even if already in global store |
| `install.saveExact` | `false` | Save exact versions to config (without `^` and `~`) |
| `install.skipIntegrityCheck` | `false` | Skip package integrity checks (faster but less secure) |
| `install.dryRun` | `false` | Simulation mode - shows what would be installed without making changes |
| `install.ignoreScripts` | `false` | Do not run install scripts from `package.json` |
| `install.ignoreOptional` | `false` | Ignore optionalDependencies |
| `install.ignoreEngines` | `false` | Ignore engines check in `package.json` |
| `install.ignorePlatform` | `false` | Ignore os/cpu checks in `package.json` |

| Feature | Default | Description |
|---------|---------|-------------|
| `update.checkForUpdates` | `true` | Automatically check for new MIP versions on startup |
| `update.autoUpdate` | `false` | Automatically update packages on startup (without confirmation) |
| `update.prerelease` | `false` | Allow installation of pre-release versions (alpha, beta, rc) |
| `update.major` | `false` | Allow major version updates (may break code) |
| `update.minor` | `true` | Allow minor version updates |
| `update.patch` | `true` | Allow patch updates |

---

### Security & Audit

| Feature | Default | Description |
|---------|---------|-------------|
| `audit.enabled` | `true` | Enable security audit for packages |
| `audit.fixAutomatically` | `false` | Automatically fix found vulnerabilities |
| `audit.ignoreDev` | `false` | Ignore devDependencies during audit |
| `audit.ignoreLow` | `true` | Ignore low severity vulnerabilities |
| `audit.ignoreModerate` | `false` | Ignore moderate severity vulnerabilities |
| `audit.ignoreHigh` | `false` | Ignore high severity vulnerabilities |
| `audit.ignoreCritical` | `false` | Ignore critical severity vulnerabilities (dangerous!) |
| `audit.parallelScan` | `true` | Scan packages in parallel for faster execution |
| `audit.timeout` | `5000` | Registry request timeout for audit (ms) |

---

### Caching

| Feature | Default | Description |
|---------|---------|-------------|
| `cache.enabled` | `true` | Use cache for downloaded packages |
| `cache.maxSize` | `500` | Maximum cache size in MB |
| `cache.ttl` | `86400` | Cache time-to-live in seconds (86400 = 1 day) |
| `cache.cleanOnExit` | `false` | Clear cache when MIP exits |
| `cache.path` | `~/.mip/cache` | Cache directory path |
| `cache.compress` | `false` | Compress cache to save disk space |

---

### Performance

| Feature | Default | Description |
|---------|---------|-------------|
| `performance.parallelDownloads` | `5` | Number of parallel package downloads |
| `performance.useHardlinks` | `false` | Use hard links instead of symlinks (saves disk space) |
| `performance.optimizeSymlinks` | `true` | Optimize symlink creation |
| `performance.useMemoryCache` | `true` | Cache package metadata in memory |
| `performance.throttle` | `false` | Limit download speed (useful for slow networks) |
| `performance.maxSockets` | `15` | Maximum number of sockets for downloads |

---

### Logging & Debug

| Feature | Default | Description |
|---------|---------|-------------|
| `logging.level` | `info` | Log level: `silent` \| `error` \| `warn` \| `info` \| `debug` \| `trace` |
| `logging.color` | `true` | Colored console output |
| `logging.timestamp` | `false` | Show timestamps in logs |
| `logging.showStack` | `false` | Show full error stack traces |
| `logging.file` | `null` | Log file path (null = stdout only) |
| `logging.maxSize` | `10MB` | Maximum log file size |

---

### Interactivity

| Feature | Default | Description |
|---------|---------|-------------|
| `interactive.promptOnInstall` | `false` | Ask for confirmation before installing |
| `interactive.promptOnDelete` | `true` | Ask for confirmation before deleting |
| `interactive.promptOnUpdate` | `true` | Ask for confirmation before updating |
| `interactive.promptOnAudit` | `false` | Ask for confirmation before running audit |
| `interactive.promptOnDedupe` | `true` | Ask for confirmation before deduplication |
| `interactive.autoConfirm` | `false` | Auto-confirm all prompts (for CI) |

---

### Registries

| Feature | Default | Description |
|---------|---------|-------------|
| `registry.default` | `npm` | Default package registry |
| `registry.fallbackToNpm` | `true` | Fall back to npm if custom registry is unavailable |
| `registry.strictSSL` | `true` | Validate registry SSL certificates |
| `registry.timeout` | `30000` | Registry request timeout (ms) |
| `registry.retryCount` | `3` | Number of retry attempts on error |
| `registry.userAgent` | `mip` | User-Agent for registry requests |
| `registry.cacheMetadata` | `true` | Cache package metadata from registry |

---

### Dependency Resolution

| Feature | Default | Description |
|---------|---------|-------------|
| `dependency.resolutionStrategy` | `semver` | Version resolution strategy: `semver` \| `exact` \| `latest` |
| `dependency.dedupeOnInstall` | `true` | Automatically deduplicate dependencies on install |
| `dependency.hoist` | `false` | Hoist dependencies to top level (like npm) |
| `dependency.workspaceProtocol` | `true` | Use `workspace:` protocol for local packages |
| `dependency.allowConflicts` | `false` | Allow conflicting versions (may cause errors) |
| `dependency.ignorePeerDependencies` | `false` | Ignore peerDependencies |
| `dependency.optionalDependencies` | `true` | Install optionalDependencies |

---

### Git & Integration

| Feature | Default | Description |
|---------|---------|-------------|
| `git.enabled` | `true` | Use Git for cloning repositories |
| `git.branch` | `main` | Default Git branch |
| `git.ssh` | `false` | Use SSH instead of HTTPS for Git |
| `git.depth` | `1` | Clone depth (1 = only latest commit) |
| `git.privateToken` | `null` | Token for private repository access |
| `git.cloneTimeout` | `60000` | Repository clone timeout (ms) |

---

### Monorepo (Workspaces)

| Feature | Default | Description |
|---------|---------|-------------|
| `monorepo.autoLink` | `true` | Automatically link local packages in workspace |
| `monorepo.hoistWorkspaces` | `false` | Hoist workspace dependencies to root level |
| `monorepo.ignoreWorkspaceRoot` | `false` | Ignore root package.json in workspace |
| `monorepo.runScriptsInOrder` | `true` | Run workspace scripts in order |
| `monorepo.parallelScripts` | `false` | Run workspace scripts in parallel |

---

### Doctor (Diagnostics)

| Feature | Default | Description |
|---------|---------|-------------|
| `doctor.checkNodeVersion` | `true` | Check Node.js version in `mip doctor` |
| `doctor.checkDiskSpace` | `true` | Check free disk space |
| `doctor.checkGitHubToken` | `true` | Check for `GITHUB_TOKEN` presence |
| `doctor.autoFix` | `false` | Automatically fix issues in `mip doctor` |
| `doctor.checkNetwork` | `true` | Check network availability |
| `doctor.checkPermissions` | `true` | Check file permissions |
| `doctor.checkSymlinks` | `true` | Check symlink integrity |
| `doctor.checkManifest` | `true` | Check manifest integrity |

---

### Server (`mip server`)

| Feature | Default | Description |
|---------|---------|-------------|
| `server.port` | `3000` | Port for web server (`mip server`) |
| `server.host` | `localhost` | Host for web server |
| `server.autoOpen` | `false` | Auto-open browser when server starts |
| `server.refreshInterval` | `3000` | Dashboard refresh interval (ms) |
| `server.darkTheme` | `true` | Use dark theme in dashboard |
| `server.authToken` | `null` | Authentication token for dashboard |
| `server.enableCors` | `true` | Enable CORS for dashboard |

---

### CLI Behavior

| Feature | Default | Description |
|---------|---------|-------------|
| `cli.suggestCommands` | `true` | Suggest similar commands on error |
| `cli.autoComplete` | `true` | Enable terminal autocompletion |
| `cli.pager` | `false` | Use pager for long output |
| `cli.confirmExit` | `false` | Ask for confirmation on exit |
| `cli.showTimings` | `false` | Show command execution time |
| `cli.noColor` | `false` | Disable colors in output |
| `cli.quiet` | `false` | Minimal output (errors only) |

---

### Plugins

| Feature | Default | Description |
|---------|---------|-------------|
| `plugins.autoLoad` | `true` | Auto-load plugins from plugins directory |
| `plugins.allowUnsecure` | `false` | Allow installing untrusted plugins |
| `plugins.path` | `./plugins` | Path to plugins directory |
| `plugins.allowGlobal` | `false` | Allow global plugins installation |
| `plugins.verbose` | `false` | Show verbose plugin output |

---

### Internationalization

| Feature | Default | Description |
|---------|---------|-------------|
| `i18n.language` | `en` | Interface language (en, ru, etc.) |
| `i18n.fallback` | `en` | Fallback language if selected is unavailable |
| `i18n.loadCustom` | `true` | Load custom language packs |

---

### Experimental Features

| Feature | Default | Description |
|---------|---------|-------------|
| `experimental.enabled` | `false` | Enable experimental features (caution!) |
| `experimental.useNewResolver` | `false` | Use new dependency resolver |
| `experimental.fastGlob` | `false` | Use fast glob for file searching |
| `experimental.parallelExtract` | `false` | Extract archives in parallel |
| `experimental.symlinkCache` | `false` | Cache symlink information |

---

### Message of the Day (MOTD)

| Feature | Default | Description |
|---------|---------|-------------|
| `motd.enabled` | `true` | Show Message of the Day with quotes and tips on each startup |

---

## Configuration File Example

```yaml
# mip.config.yml

# === GLOBAL ===
config.enabled: true

# === INSTALL ===
install.parallel: true
install.forceReinstall: false
install.saveExact: false
install.dryRun: false

# === UPDATE ===
update.checkForUpdates: true
update.autoUpdate: false
update.prerelease: false
update.major: false
update.minor: true
update.patch: true

# === AUDIT ===
audit.enabled: true
audit.ignoreLow: true
audit.parallelScan: true
audit.timeout: 5000

# === CACHE ===
cache.enabled: true
cache.maxSize: 500
cache.path: ~/.mip/cache

# === PERFORMANCE ===
performance.parallelDownloads: 5
performance.useMemoryCache: true

# === LOGGING ===
logging.level: info
logging.color: true

# === INTERACTIVE ===
interactive.promptOnDelete: true
interactive.promptOnUpdate: true

# === REGISTRY ===
registry.default: npm
registry.fallbackToNpm: true

# === DEPENDENCY ===
dependency.resolutionStrategy: semver
dependency.dedupeOnInstall: true

# === DOCTOR ===
doctor.checkNodeVersion: true
doctor.checkDiskSpace: true

# === SERVER ===
server.port: 3000
server.darkTheme: true

# === CLI ===
cli.suggestCommands: true

# === PLUGINS ===
plugins.autoLoad: true

# === I18N ===
i18n.language: en

# === EXPERIMENTAL ===
experimental.enabled: false

# === MOTD ===
motd.enabled: true
```

## Common Use Cases

### CI/CD Optimizations

For CI/CD environments, disable heavy features for maximum speed:

```bash
mip config set features.audit.enabled false
mip config set features.update.checkForUpdates false
mip config set features.interactive.promptOnInstall false
mip config set features.interactive.autoConfirm true
mip config set features.motd.enabled false
```

### Development Environment

Enable helpful features for development:

```bash
mip config set features.install.parallel true
mip config set features.logging.level debug
mip config set features.motd.enabled true
```

### Production Builds

For production builds, ensure reproducibility:

```bash
mip config set features.install.saveExact true
mip config set features.update.major false
mip config set features.dependency.resolutionStrategy exact
```

## Troubleshooting

### Feature Not Found

```
❌ Feature "install.xxx" not found
```

**Resolution:**
1. Check available features: `mip --list-features`
2. Make sure you typed the feature name correctly
3. Features use dot notation: `install.parallel`, `update.checkForUpdates`

### Features Disabled

```
⚠️ Features are currently DISABLED
```

**Resolution:**
1. Enable features: `mip config set features.config.enabled true`
2. Or edit `mip.config.yml` and set `config.enabled: true`

### Invalid Value

```
⚠️ Invalid value for feature "install.parallel"
```

**Resolution:**
- Booleans: `true` or `false`
- Numbers: `5`, `3000`, `86400`
- Strings: `"info"`, `"localhost"`, `"npm"`
- Check the feature description for the expected type

## Related Commands

- [mip config](/commands/config) - manage configuration
- [mip --genconfig](/commands/genconfig) - generate feature configuration
- [mip --list-features](/commands/list-features) - list all features
- [mip doctor](/commands/doctor) - system diagnostics
- [mip registry](/commands/registry) - manage registries