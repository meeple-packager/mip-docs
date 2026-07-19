---
title: "mip features - настройка поведения"
description: "Система фич MIP позволяет тонко настроить каждый аспект работы пакетного менеджера через единый файл конфигурации."
---

# mip features

::: callout info "Что это?" icon:🎛️
MIP имеет встроенную систему фич, которая позволяет контролировать почти каждый аспект его поведения. От скорости установки до проверок безопасности - вы можете включать или отключать фичи одной командой.
:::

## Быстрый старт

```bash
# Сгенерировать конфигурацию по умолчанию
mip --genconfig

# Посмотреть все доступные фичи
mip --list-features

# Включить фичу
mip config set features.install.parallel true

# Выключить фичу
mip config set features.update.checkForUpdates false

# Выключить ВСЕ фичи разом
mip config set features.config.enabled false
```

## Как это работает

1. **Фичи хранятся** в `mip.config.yml` (на уровне проекта) или `~/.mip/config.yml` (глобально)
2. **У каждой фичи** есть значение по умолчанию - вам нужно переопределять только то, что хотите изменить
3. **Главный выключатель** - `config.enabled: false` отключает ВСЕ модификации фич
4. **Нулевое влияние на производительность** - фичи загружаются один раз при запуске
5. **Гранулярный контроль** - включайте/отключайте конкретные поведения без влияния на другие

## Список фич

### 🚀 Установка и обновление

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `install.parallel` | `true` | Параллельная установка пакетов для ускорения |
| `install.forceReinstall` | `false` | Всегда переустанавливать пакеты, даже если уже есть в глобальном хранилище |
| `install.saveExact` | `false` | Сохранять точные версии в конфиг (без ^ и ~) |
| `install.skipIntegrityCheck` | `false` | Пропускать проверку целостности пакетов (быстрее, но менее безопасно) |
| `install.dryRun` | `false` | Режим имитации - показывает что будет установлено, но ничего не меняет |
| `install.ignoreScripts` | `false` | Не запускать скрипты установки из package.json |
| `install.ignoreOptional` | `false` | Игнорировать optionalDependencies |
| `install.ignoreEngines` | `false` | Игнорировать проверку engines в package.json |
| `install.ignorePlatform` | `false` | Игнорировать проверку os/cpu в package.json |

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `update.checkForUpdates` | `true` | Автоматически проверять новые версии MIP при запуске |
| `update.autoUpdate` | `false` | Автоматически обновлять пакеты при запуске (без подтверждения) |
| `update.prerelease` | `false` | Разрешать установку pre-release версий (alpha, beta, rc) |
| `update.major` | `false` | Разрешать обновление на мажорные версии (может сломать код) |
| `update.minor` | `true` | Разрешать обновление на минорные версии |
| `update.patch` | `true` | Разрешать обновление на патчи |

### 🔒 Безопасность и аудит

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `audit.enabled` | `true` | Включить проверку безопасности пакетов |
| `audit.fixAutomatically` | `false` | Автоматически исправлять найденные уязвимости |
| `audit.ignoreDev` | `false` | Игнорировать devDependencies при аудите |
| `audit.ignoreLow` | `true` | Игнорировать уязвимости низкого уровня |
| `audit.ignoreModerate` | `false` | Игнорировать уязвимости среднего уровня |
| `audit.ignoreHigh` | `false` | Игнорировать уязвимости высокого уровня |
| `audit.ignoreCritical` | `false` | Игнорировать критические уязвимости (опасно!) |
| `audit.parallelScan` | `true` | Сканировать пакеты параллельно для ускорения |
| `audit.timeout` | `5000` | Таймаут запросов к реестру при аудите (мс) |

### 💾 Кеширование

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `cache.enabled` | `true` | Использовать кеш загруженных пакетов |
| `cache.maxSize` | `500` | Максимальный размер кеша в МБ |
| `cache.ttl` | `86400` | Время жизни кеша в секундах (86400 = 1 день) |
| `cache.cleanOnExit` | `false` | Очищать кеш при завершении работы MIP |
| `cache.path` | `~/.mip/cache` | Путь к папке кеша |
| `cache.compress` | `false` | Сжимать кеш для экономии места |

### ⚡ Производительность

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `performance.parallelDownloads` | `5` | Количество параллельных загрузок пакетов |
| `performance.useHardlinks` | `false` | Использовать жёсткие ссылки вместо симлинков (экономит место) |
| `performance.optimizeSymlinks` | `true` | Оптимизировать создание симлинков |
| `performance.useMemoryCache` | `true` | Кешировать метаданные пакетов в памяти |
| `performance.throttle` | `false` | Ограничивать скорость загрузки (полезно для слабых сетей) |
| `performance.maxSockets` | `15` | Максимальное количество сокетов для загрузки |

### 📝 Логирование и отладка

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `logging.level` | `info` | Уровень логирования: silent \| error \| warn \| info \| debug \| trace |
| `logging.color` | `true` | Цветной вывод в консоль |
| `logging.timestamp` | `false` | Показывать временные метки в логах |
| `logging.showStack` | `false` | Показывать полный стек ошибок |
| `logging.file` | `null` | Путь к файлу лога (null = только в stdout) |
| `logging.maxSize` | `10MB` | Максимальный размер файла лога |

### 🎮 Интерактивность

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `interactive.promptOnInstall` | `false` | Запрашивать подтверждение перед установкой |
| `interactive.promptOnDelete` | `true` | Запрашивать подтверждение перед удалением |
| `interactive.promptOnUpdate` | `true` | Запрашивать подтверждение перед обновлением |
| `interactive.promptOnAudit` | `false` | Запрашивать подтверждение перед аудитом |
| `interactive.promptOnDedupe` | `true` | Запрашивать подтверждение перед дедупликацией |
| `interactive.autoConfirm` | `false` | Автоматически подтверждать все запросы (для CI) |

### 🔗 Реестры

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `registry.default` | `npm` | Реестр пакетов по умолчанию |
| `registry.fallbackToNpm` | `true` | Если кастомный реестр недоступен, использовать npm |
| `registry.strictSSL` | `true` | Проверять SSL сертификаты реестров |
| `registry.timeout` | `30000` | Таймаут запросов к реестру (мс) |
| `registry.retryCount` | `3` | Количество повторных попыток при ошибке |
| `registry.userAgent` | `mip` | User-Agent для запросов к реестру |
| `registry.cacheMetadata` | `true` | Кешировать метаданные пакетов с реестра |

### 📦 Разрешение зависимостей

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `dependency.resolutionStrategy` | `semver` | Стратегия разрешения версий: semver \| exact \| latest |
| `dependency.dedupeOnInstall` | `true` | Автоматически дедуплицировать зависимости при установке |
| `dependency.hoist` | `false` | Поднимать зависимости на верхний уровень (как npm) |
| `dependency.workspaceProtocol` | `true` | Использовать workspace: для локальных пакетов |
| `dependency.allowConflicts` | `false` | Разрешать конфликтующие версии (может вызвать ошибки) |
| `dependency.ignorePeerDependencies` | `false` | Игнорировать peerDependencies |
| `dependency.optionalDependencies` | `true` | Устанавливать optionalDependencies |

### 🔧 Git и интеграция

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `git.enabled` | `true` | Использовать Git для клонирования репозиториев |
| `git.branch` | `main` | Ветка Git по умолчанию |
| `git.ssh` | `false` | Использовать SSH вместо HTTPS для Git |
| `git.depth` | `1` | Глубина клонирования (1 = только последний коммит) |
| `git.privateToken` | `null` | Токен для доступа к приватным репозиториям |
| `git.cloneTimeout` | `60000` | Таймаут клонирования репозитория (мс) |

### 📁 Монорепозитории (Workspaces)

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `monorepo.autoLink` | `true` | Автоматически линковать локальные пакеты в workspace |
| `monorepo.hoistWorkspaces` | `false` | Поднимать зависимости workspace на корневой уровень |
| `monorepo.ignoreWorkspaceRoot` | `false` | Игнорировать корневой package.json в workspace |
| `monorepo.runScriptsInOrder` | `true` | Запускать скрипты в workspace по порядку |
| `monorepo.parallelScripts` | `false` | Запускать скрипты в workspace параллельно |

### 🏥 Диагностика (doctor)

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `doctor.checkNodeVersion` | `true` | Проверять версию Node.js в mip doctor |
| `doctor.checkDiskSpace` | `true` | Проверять свободное место на диске |
| `doctor.checkGitHubToken` | `true` | Проверять наличие GITHUB_TOKEN |
| `doctor.autoFix` | `false` | Автоматически исправлять проблемы в mip doctor |
| `doctor.checkNetwork` | `true` | Проверять доступность сети |
| `doctor.checkPermissions` | `true` | Проверять права доступа |
| `doctor.checkSymlinks` | `true` | Проверять целостность симлинков |
| `doctor.checkManifest` | `true` | Проверять целостность манифеста |

### 🌐 Сервер (mip server)

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `server.port` | `3000` | Порт для веб-сервера (mip server) |
| `server.host` | `localhost` | Хост для веб-сервера |
| `server.autoOpen` | `false` | Автоматически открывать браузер при запуске сервера |
| `server.refreshInterval` | `3000` | Интервал обновления дашборда (мс) |
| `server.darkTheme` | `true` | Использовать тёмную тему в дашборде |
| `server.authToken` | `null` | Токен для аутентификации на дашборде |
| `server.enableCors` | `true` | Включить CORS для дашборда |

### 💻 Поведение CLI

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `cli.suggestCommands` | `true` | Предлагать похожие команды при ошибке |
| `cli.autoComplete` | `true` | Включить автодополнение в терминале |
| `cli.pager` | `false` | Использовать пейджер для длинного вывода |
| `cli.confirmExit` | `false` | Запрашивать подтверждение при выходе |
| `cli.showTimings` | `false` | Показывать время выполнения команд |
| `cli.noColor` | `false` | Отключить цвета в выводе |
| `cli.quiet` | `false` | Минимальный вывод (только ошибки) |

### 🔌 Плагины

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `plugins.autoLoad` | `true` | Автоматически загружать плагины из папки |
| `plugins.allowUnsecure` | `false` | Разрешать установку непроверенных плагинов |
| `plugins.path` | `./plugins` | Путь к папке с плагинами |
| `plugins.allowGlobal` | `false` | Разрешать установку глобальных плагинов |
| `plugins.verbose` | `false` | Показывать подробный вывод плагинов |

### 🌍 Интернационализация

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `i18n.language` | `en` | Язык интерфейса (en, ru, и т.д.) |
| `i18n.fallback` | `en` | Язык интерфейса по умолчанию (если выбранный недоступен) |
| `i18n.loadCustom` | `true` | Загружать пользовательские языковые пакеты |

### 🧪 Экспериментальные функции

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `experimental.enabled` | `false` | Включить экспериментальные функции (осторожно!) |
| `experimental.useNewResolver` | `false` | Использовать новый резолвер зависимостей |
| `experimental.fastGlob` | `false` | Использовать быстрый glob для поиска файлов |
| `experimental.parallelExtract` | `false` | Распаковывать архивы параллельно |
| `experimental.symlinkCache` | `false` | Кешировать информацию о симлинках |

## Пример файла конфигурации

`mip.config.yml`:

```yaml
# ────────────────────────────────────────────────
# MIP Features Configuration
# Generated: 2026-07-05T10:00:00.000Z
# ────────────────────────────────────────────────
# Documentation: https://mipdocs.fwh.is/features
#
# Master switch - set to false to disable ALL feature modifications
# config.enabled: true
#
# To enable/disable a feature:
#   mip config set features.install.parallel true
# Or edit this file manually
# ────────────────────────────────────────────────

# === INSTALL ===
install.parallel: true  # Install multiple packages in parallel
install.forceReinstall: false  # Always reinstall packages
install.saveExact: false  # Save exact versions

# === UPDATE ===
update.checkForUpdates: true  # Check for new MIP versions
update.autoUpdate: false  # Auto-update packages

# === AUDIT ===
audit.enabled: true  # Enable security audit
audit.ignoreLow: true  # Ignore low severity vulnerabilities

# === CACHE ===
cache.enabled: true  # Use download cache
cache.maxSize: 500  # Maximum cache size in MB

# === PERFORMANCE ===
performance.parallelDownloads: 5  # Parallel downloads

# === LOGGING ===
logging.level: info  # Log level

# === INTERACTIVE ===
interactive.promptOnDelete: true  # Confirm before deleting

# === REGISTRY ===
registry.default: npm  # Default registry
registry.fallbackToNpm: true  # Fallback to npm

# === DEPENDENCY ===
dependency.resolutionStrategy: semver  # Version strategy

# === GIT ===
git.enabled: true  # Use Git

# === MONOREPO ===
monorepo.autoLink: true  # Auto-link workspaces

# === DOCTOR ===
doctor.checkNodeVersion: true  # Check Node.js version
doctor.checkDiskSpace: true  # Check disk space

# === SERVER ===
server.port: 3000  # Web server port
server.darkTheme: true  # Dark theme

# === CLI ===
cli.suggestCommands: true  # Suggest similar commands

# === PLUGINS ===
plugins.autoLoad: true  # Auto-load plugins

# === I18N ===
i18n.language: en  # Interface language

# === EXPERIMENTAL ===
experimental.enabled: false  # Experimental features
```

## Частые ошибки

::: callout warning "❌ ERR_FEATURE_NOT_FOUND" icon:alert-triangle
**Причина:** Указанная фича не существует.

**Решение:**
1. Проверьте доступные фичи: `mip --list-features`
2. Убедитесь, что правильно написали имя фичи
3. Фичи используют точечную нотацию: `install.parallel`, `update.checkForUpdates`
:::

::: callout danger "❌ ERR_FEATURES_DISABLED" icon:skull
**Причина:** Все фичи отключены глобально (`config.enabled: false`).

**Решение:**
1. Включите фичи: `mip config set features.config.enabled true`
2. Или отредактируйте `mip.config.yml` и установите `config.enabled: true`
:::

::: callout warning "⚠️ ERR_INVALID_VALUE" icon:alert-circle
**Причина:** Предоставленное значение имеет неверный тип.

**Решение:**
1. Булевы: `true` или `false`
2. Числа: `5`, `3000`, `86400`
3. Строки: `"info"`, `"localhost"`, `"npm"`
4. Проверьте описание фичи для ожидаемого типа
:::

## Связанные команды

- [mip config](/commands/config) - управление конфигурацией
- [mip doctor](/commands/doctor) - диагностика системы
- [mip registry](/commands/registry) - управление реестрами

::: collapsible "🔧 Как загружаются фичи"
MIP загружает фичи в следующем порядке:

1. **Значения по умолчанию** - встроенные значения
2. **Глобальный конфиг** - `~/.mip/config.yml` (на уровне пользователя)
3. **Проектный конфиг** - `mip.config.yml` (на уровне проекта)
4. **mip.yml** - секция `features` в `mip.yml`

Более поздние значения переопределяют предыдущие. Это позволяет задать глобальные настройки и переопределять их для конкретного проекта.
:::

::: callout tip "💡 Совет: Оптимизация производительности" icon:lightbulb
Для максимальной скорости в CI/CD окружениях отключите тяжёлые фичи:
```bash
mip config set features.audit.enabled false
mip config set features.update.checkForUpdates false
mip config set features.interactive.promptOnInstall false
mip config set features.interactive.autoConfirm true
```
:::
