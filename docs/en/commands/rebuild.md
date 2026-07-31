---
title: "mip rebuild - rebuild installed packages"
description: "The rebuild command re-runs build scripts for packages installed in the global store, useful after platform changes, Node.js upgrades, or when native modules need recompilation."
---

# mip rebuild

::: callout info "What is this?" icon:wrench
`mip rebuild` re-executes build scripts for packages that are already installed in the global store. This is useful when you upgrade Node.js, switch platforms, or need to recompile native (C++) addons — without re-downloading the entire package.
:::

## Syntax

```bash
mip rebuild [packages...] [options]
```

## Options

| Flag | Description |
|------|-------------|
| `--force`, `-f` | Force rebuild even if the package has no build scripts (uses `rebuild` as the default script) |
| `--dry-run` | Simulate the rebuild process without actually running any scripts |
| `--quiet` | Suppress all output except errors and the final summary |
| `--script <name>` | Run a specific script instead of auto-detecting one (e.g., `--script build`) |

## How it works

1. **Read the lockfile** — the command reads `mip-lock.yml` (or `mip-lock.json` as a fallback). If neither exists, execution stops.

2. **Determine target packages** — if no package names are provided, **all** packages from the lockfile are rebuilt. You can filter specific packages by passing their names:

   ```bash
   mip rebuild lodash axios
   ```

3. **Validate store presence** — for each package, the command checks whether it actually exists in the global store (`~/.mip/store/<name>/<version>/package`). Missing packages are skipped.

4. **Detect the build script** — the command reads `package.json` from the store and looks for scripts in this priority order:

   | Priority | Script name | Purpose |
   |----------|-------------|---------|
   | 1 | `install` | Post-install script (most common for native modules) |
   | 2 | `rebuild` | Explicit rebuild script |
   | 3 | `preinstall` | Pre-install hook |
   | 4 | `build` | General build script |

   If none of these are found and `--force` is not set, the package is skipped.

5. **Run the script** — the script is executed via `npm run <script>` in the package directory inside the store. The following environment variables are injected:

   | Variable | Value |
   |----------|-------|
   | `MIP_REBUILD` | `true` |
   | `MIP_PACKAGE_NAME` | package name |
   | `MIP_PACKAGE_VERSION` | package version |

   A local `.npm-cache` directory is used to avoid polluting the global npm cache.

6. **Summary** — after processing all packages, the command prints:

   ```
   ✅ Done. Rebuilt: 5, Skipped: 2, Errors: 0.
   ```

## Examples

::: tabs
== tab "Rebuild all packages"
```bash
mip rebuild
```

Rebuilds every package listed in `mip-lock.yml` that has a build script.

**Output:**
```
🔨 Rebuilding 8 package(s)...

🔧 Rebuilding "lodash@4.17.21" (install)...
✅ Rebuilt: lodash@4.17.21

🔧 Rebuilding "node-gyp@9.3.0" (install)...
✅ Rebuilt: node-gyp@9.3.0

⚠️ Package "express@4.18.2" has no build scripts. Skipping.

✅ Done. Rebuilt: 2, Skipped: 1, Errors: 0.
```
:::

== tab "Rebuild specific packages"
```bash
mip rebuild node-gyp bcrypt
```

Only rebuilds the packages you specify. Unknown or missing packages are reported but ignored.

**Output:**
```
🔨 Rebuilding 2 package(s)...

🔧 Rebuilding "node-gyp@9.3.0" (install)...
✅ Rebuilt: node-gyp@9.3.0

🔧 Rebuilding "bcrypt@5.1.0" (install)...
✅ Rebuilt: bcrypt@5.1.0

✅ Done. Rebuilt: 2, Skipped: 0, Errors: 0.
```
:::

== tab "Force rebuild"
```bash
mip rebuild --force
```

Rebuilds every package, even those without build scripts. Packages without scripts will use the `rebuild` npm script (which may be empty, but the attempt is still made).

**Output:**
```
🔨 Rebuilding 8 package(s)...

🔧 Rebuilding "lodash@4.17.21" (install)...
✅ Rebuilt: lodash@4.17.21

🔧 Rebuilding "express@4.18.2" (rebuild)...
❌ Error: express@4.18.2: command exited with code 1

✅ Done. Rebuilt: 1, Skipped: 0, Errors: 1.
```
:::

== tab "Custom script"
```bash
mip rebuild --script build
```

Runs the `build` script for every package that has one, instead of auto-detecting.

**Output:**
```
🔨 Rebuilding 8 package(s)...

🔧 Rebuilding "lodash@4.17.21" (build)...
✅ Rebuilt: lodash@4.17.21

⚠️ Package "node-gyp@9.3.0" has no script "build"
   (use --force to run anyway)

✅ Done. Rebuilt: 1, Skipped: 1, Errors: 0.
```
:::

== tab "Dry run"
```bash
mip rebuild --dry-run
```

Shows what would be rebuilt without actually running anything.

**Output:**
```
🔨 Rebuilding 8 package(s)...

🔧 Would rebuild "lodash@4.17.21" (install)
🔧 Would rebuild "node-gyp@9.3.0" (install)

🔍 Dry-run complete. Would rebuild: 2, Skipped: 6.
```
:::

== tab "Quiet mode"
```bash
mip rebuild --quiet
```

Rebuilds with no per-package output. Only the final summary is shown.

**Output:**
```
✅ Done. Rebuilt: 2, Skipped: 1, Errors: 0.
```
:::
:::

## When to use rebuild

| Scenario | Why `mip rebuild`? |
|----------|--------------------|
| **Node.js upgrade** | Native addons compiled for the old Node.js ABI may fail. Rebuild recompiles them. |
| **Platform switch** | Moving from macOS to Linux (or vice versa) — binaries are platform-specific. |
| **Corrupted store** | If a package in the global store is incomplete or broken, rebuild fixes it without re-downloading. |
| **CI/CD cache** | After restoring the store from cache, rebuild ensures native modules are compiled for the CI runner's environment. |
| **After `mip dedupe`** | Deduplication may change which physical package is used — rebuild ensures consistency. |

## Common errors

::: callout warning "❌ Package not found in global store" icon:alert-triangle
**Problem:** The package exists in `mip-lock.yml` but its directory is missing from `~/.mip/store/`.

**What happens:** The package is skipped with a warning.

**Fix:** Run `mip install` to re-fetch the package.
:::

::: callout danger "❌ mip-lock.yml not found" icon:skull
**Problem:** The current directory does not contain `mip-lock.yml` or `mip-lock.json`.

**What happens:** The command exits immediately.

**Fix:** Run `mip install` or `mip genlock` to generate the lockfile, then try again.
:::

::: callout warning "⚠️ Script execution failed" icon:alert-circle
**Problem:** The build script exited with a non-zero exit code.

**What happens:** The error is reported in the summary, but the command continues processing remaining packages.

**Fix:** Run `mip rebuild <pkg>` (without `--quiet`) to see the full error output.
:::

::: callout info "💡 Build script priority order" icon:info
The command checks for scripts in this order: `install` → `rebuild` → `preinstall` → `build`. It picks the **first** one found. If you need to run a specific script, use `--script <name>`.
:::

## Related commands

- [mip install](/commands/install) — install packages and run their build scripts
- [mip dedupe](/commands/dedupe) — deduplicate dependencies (may require rebuild after)
- [mip ci](/commands/ci) — clean CI installation (rebuild is often needed in CI)
- [mip cache](/commands/cache) — manage the local cache (rebuild uses a local `.npm-cache`)

