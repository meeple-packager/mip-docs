# Документация по фичам MIP

## Обзор

MIP 2.1 представляет мощную систему фич, которая позволяет контролировать почти каждый аспект поведения пакетного менеджера. Каждая фича может быть включена или отключена через файл конфигурации или команды CLI.

## Быстрый старт

```bash
# Сгенерировать конфигурацию со всеми фичами
mip --genconfig

# Просмотреть все доступные фичи
mip --list-features

# Включить фичу
mip config set features.install.parallel true

# Выключить фичу
mip config set features.update.checkForUpdates false

# Выключить ВСЕ фичи разом
mip config set features.config.enabled false
```

## Категории фич

### Глобальные

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `config.enabled` | `true` | Главный выключатель - отключает ВСЕ модификации фич при значении `false` |

---

### Установка и обновление

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `install.parallel` | `true` | Параллельная установка пакетов для ускорения |
| `install.forceReinstall` | `false` | Всегда переустанавливать пакеты, даже если уже есть в глобальном хранилище |
| `install.saveExact` | `false` | Сохранять точные версии в конфиг (без `^` и `~`) |
| `install.skipIntegrityCheck` | `false` | Пропускать проверку целостности пакетов (быстрее, но менее безопасно) |
| `install.dryRun` | `false` | Режим имитации - показывает что будет установлено, но ничего не меняет |
| `install.ignoreScripts` | `false` | Не запускать скрипты установки из `package.json` |
| `install.ignoreOptional` | `false` | Игнорировать optionalDependencies |
| `install.ignoreEngines` | `false` | Игнорировать проверку engines в `package.json` |
| `install.ignorePlatform` | `false` | Игнорировать проверку os/cpu в `package.json` |

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `update.checkForUpdates` | `true` | Автоматически проверять новые версии MIP при запуске |
| `update.autoUpdate` | `false` | Автоматически обновлять пакеты при запуске (без подтверждения) |
| `update.prerelease` | `false` | Разрешать установку pre-release версий (alpha, beta, rc) |
| `update.major` | `false` | Разрешать обновление на мажорные версии (может сломать код) |
| `update.minor` | `true` | Разрешать обновление на минорные версии |
| `update.patch` | `true` | Разрешать обновление на патчи |

---

### Безопасность и аудит

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

---

### Кеширование

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `cache.enabled` | `true` | Использовать кеш загруженных пакетов |
| `cache.maxSize` | `500` | Максимальный размер кеша в МБ |
| `cache.ttl` | `86400` | Время жизни кеша в секундах (86400 = 1 день) |
| `cache.cleanOnExit` | `false` | Очищать кеш при завершении работы MIP |
| `cache.path` | `~/.mip/cache` | Путь к папке кеша |
| `cache.compress` | `false` | Сжимать кеш для экономии места |

---

### Производительность

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `performance.parallelDownloads` | `5` | Количество параллельных загрузок пакетов |
| `performance.useHardlinks` | `false` | Использовать жёсткие ссылки вместо симлинков (экономит место) |
| `performance.optimizeSymlinks` | `true` | Оптимизировать создание симлинков |
| `performance.useMemoryCache` | `true` | Кешировать метаданные пакетов в памяти |
| `performance.throttle` | `false` | Ограничивать скорость загрузки (полезно для слабых сетей) |
| `performance.maxSockets` | `15` | Максимальное количество сокетов для загрузки |

---

### Логирование и отладка

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `logging.level` | `info` | Уровень логирования: `silent` \| `error` \| `warn` \| `info` \| `debug` \| `trace` |
| `logging.color` | `true` | Цветной вывод в консоль |
| `logging.timestamp` | `false` | Показывать временные метки в логах |
| `logging.showStack` | `false` | Показывать полный стек ошибок |
| `logging.file` | `null` | Путь к файлу лога (null = только в stdout) |
| `logging.maxSize` | `10MB` | Максимальный размер файла лога |

---

### Интерактивность

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `interactive.promptOnInstall` | `false` | Запрашивать подтверждение перед установкой |
| `interactive.promptOnDelete` | `true` | Запрашивать подтверждение перед удалением |
| `interactive.promptOnUpdate` | `true` | Запрашивать подтверждение перед обновлением |
| `interactive.promptOnAudit` | `false` | Запрашивать подтверждение перед аудитом |
| `interactive.promptOnDedupe` | `true` | Запрашивать подтверждение перед дедупликацией |
| `interactive.autoConfirm` | `false` | Автоматически подтверждать все запросы (для CI) |

---

### Реестры

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `registry.default` | `npm` | Реестр пакетов по умолчанию |
| `registry.fallbackToNpm` | `true` | Если кастомный реестр недоступен, использовать npm |
| `registry.strictSSL` | `true` | Проверять SSL сертификаты реестров |
| `registry.timeout` | `30000` | Таймаут запросов к реестру (мс) |
| `registry.retryCount` | `3` | Количество повторных попыток при ошибке |
| `registry.userAgent` | `mip` | User-Agent для запросов к реестру |
| `registry.cacheMetadata` | `true` | Кешировать метаданные пакетов с реестра |

---

### Разрешение зависимостей

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `dependency.resolutionStrategy` | `semver` | Стратегия разрешения версий: `semver` \| `exact` \| `latest` |
| `dependency.dedupeOnInstall` | `true` | Автоматически дедуплицировать зависимости при установке |
| `dependency.hoist` | `false` | Поднимать зависимости на верхний уровень (как npm) |
| `dependency.workspaceProtocol` | `true` | Использовать `workspace:` для локальных пакетов |
| `dependency.allowConflicts` | `false` | Разрешать конфликтующие версии (может вызвать ошибки) |
| `dependency.ignorePeerDependencies` | `false` | Игнорировать peerDependencies |
| `dependency.optionalDependencies` | `true` | Устанавливать optionalDependencies |

---

### Git и интеграция

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `git.enabled` | `true` | Использовать Git для клонирования репозиториев |
| `git.branch` | `main` | Ветка Git по умолчанию |
| `git.ssh` | `false` | Использовать SSH вместо HTTPS для Git |
| `git.depth` | `1` | Глубина клонирования (1 = только последний коммит) |
| `git.privateToken` | `null` | Токен для доступа к приватным репозиториям |
| `git.cloneTimeout` | `60000` | Таймаут клонирования репозитория (мс) |

---

### Монорепозитории (Workspaces)

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `monorepo.autoLink` | `true` | Автоматически линковать локальные пакеты в workspace |
| `monorepo.hoistWorkspaces` | `false` | Поднимать зависимости workspace на корневой уровень |
| `monorepo.ignoreWorkspaceRoot` | `false` | Игнорировать корневой package.json в workspace |
| `monorepo.runScriptsInOrder` | `true` | Запускать скрипты в workspace по порядку |
| `monorepo.parallelScripts` | `false` | Запускать скрипты в workspace параллельно |

---

### Диагностика (doctor)

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `doctor.checkNodeVersion` | `true` | Проверять версию Node.js в `mip doctor` |
| `doctor.checkDiskSpace` | `true` | Проверять свободное место на диске |
| `doctor.checkGitHubToken` | `true` | Проверять наличие `GITHUB_TOKEN` |
| `doctor.autoFix` | `false` | Автоматически исправлять проблемы в `mip doctor` |
| `doctor.checkNetwork` | `true` | Проверять доступность сети |
| `doctor.checkPermissions` | `true` | Проверять права доступа |
| `doctor.checkSymlinks` | `true` | Проверять целостность симлинков |
| `doctor.checkManifest` | `true` | Проверять целостность манифеста |

---

### Сервер (`mip server`)

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `server.port` | `3000` | Порт для веб-сервера (`mip server`) |
| `server.host` | `localhost` | Хост для веб-сервера |
| `server.autoOpen` | `false` | Автоматически открывать браузер при запуске сервера |
| `server.refreshInterval` | `3000` | Интервал обновления дашборда (мс) |
| `server.darkTheme` | `true` | Использовать тёмную тему в дашборде |
| `server.authToken` | `null` | Токен для аутентификации на дашборде |
| `server.enableCors` | `true` | Включить CORS для дашборда |

---

### Поведение CLI

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `cli.suggestCommands` | `true` | Предлагать похожие команды при ошибке |
| `cli.autoComplete` | `true` | Включить автодополнение в терминале |
| `cli.pager` | `false` | Использовать пейджер для длинного вывода |
| `cli.confirmExit` | `false` | Запрашивать подтверждение при выходе |
| `cli.showTimings` | `false` | Показывать время выполнения команд |
| `cli.noColor` | `false` | Отключить цвета в выводе |
| `cli.quiet` | `false` | Минимальный вывод (только ошибки) |

---

### Плагины

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `plugins.autoLoad` | `true` | Автоматически загружать плагины из папки |
| `plugins.allowUnsecure` | `false` | Разрешать установку непроверенных плагинов |
| `plugins.path` | `./plugins` | Путь к папке с плагинами |
| `plugins.allowGlobal` | `false` | Разрешать установку глобальных плагинов |
| `plugins.verbose` | `false` | Показывать подробный вывод плагинов |

---

### Интернационализация

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `i18n.language` | `en` | Язык интерфейса (en, ru, и т.д.) |
| `i18n.fallback` | `en` | Язык интерфейса по умолчанию (если выбранный недоступен) |
| `i18n.loadCustom` | `true` | Загружать пользовательские языковые пакеты |

---

### Экспериментальные функции

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `experimental.enabled` | `false` | Включить экспериментальные функции (осторожно!) |
| `experimental.useNewResolver` | `false` | Использовать новый резолвер зависимостей |
| `experimental.fastGlob` | `false` | Использовать быстрый glob для поиска файлов |
| `experimental.parallelExtract` | `false` | Распаковывать архивы параллельно |
| `experimental.symlinkCache` | `false` | Кешировать информацию о симлинках |

---

### Сообщение дня (MOTD)

| Фича | По умолчанию | Описание |
|------|--------------|----------|
| `motd.enabled` | `true` | Показывать сообщение дня с цитатами и советами при каждом запуске |

---

## Пример файла конфигурации

```yaml
# mip.config.yml

# === ГЛОБАЛЬНЫЕ ===
config.enabled: true

# === УСТАНОВКА ===
install.parallel: true
install.forceReinstall: false
install.saveExact: false
install.dryRun: false

# === ОБНОВЛЕНИЕ ===
update.checkForUpdates: true
update.autoUpdate: false
update.prerelease: false
update.major: false
update.minor: true
update.patch: true

# === АУДИТ ===
audit.enabled: true
audit.ignoreLow: true
audit.parallelScan: true
audit.timeout: 5000

# === КЕШ ===
cache.enabled: true
cache.maxSize: 500
cache.path: ~/.mip/cache

# === ПРОИЗВОДИТЕЛЬНОСТЬ ===
performance.parallelDownloads: 5
performance.useMemoryCache: true

# === ЛОГИРОВАНИЕ ===
logging.level: info
logging.color: true

# === ИНТЕРАКТИВНОСТЬ ===
interactive.promptOnDelete: true
interactive.promptOnUpdate: true

# === РЕЕСТРЫ ===
registry.default: npm
registry.fallbackToNpm: true

# === ЗАВИСИМОСТИ ===
dependency.resolutionStrategy: semver
dependency.dedupeOnInstall: true

# === ДИАГНОСТИКА ===
doctor.checkNodeVersion: true
doctor.checkDiskSpace: true

# === СЕРВЕР ===
server.port: 3000
server.darkTheme: true

# === CLI ===
cli.suggestCommands: true

# === ПЛАГИНЫ ===
plugins.autoLoad: true

# === I18N ===
i18n.language: en

# === ЭКСПЕРИМЕНТАЛЬНЫЕ ===
experimental.enabled: false

# === MOTD ===
motd.enabled: true
```

## Частые сценарии использования

### Оптимизация для CI/CD

Для CI/CD окружений отключите тяжёлые фичи для максимальной скорости:

```bash
mip config set features.audit.enabled false
mip config set features.update.checkForUpdates false
mip config set features.interactive.promptOnInstall false
mip config set features.interactive.autoConfirm true
mip config set features.motd.enabled false
```

### Среда разработки

Включите полезные фичи для разработки:

```bash
mip config set features.install.parallel true
mip config set features.logging.level debug
mip config set features.motd.enabled true
```

### Продакшн сборки

Для продакшн сборок обеспечьте воспроизводимость:

```bash
mip config set features.install.saveExact true
mip config set features.update.major false
mip config set features.dependency.resolutionStrategy exact
```

## Устранение неполадок

### Фича не найдена

```
❌ Feature "install.xxx" not found
```

**Решение:**
1. Проверьте доступные фичи: `mip --list-features`
2. Убедитесь, что правильно написали имя фичи
3. Фичи используют точечную нотацию: `install.parallel`, `update.checkForUpdates`

### Фичи отключены

```
⚠️ Features are currently DISABLED
```

**Решение:**
1. Включите фичи: `mip config set features.config.enabled true`
2. Или отредактируйте `mip.config.yml` и установите `config.enabled: true`

### Неверное значение

```
⚠️ Invalid value for feature "install.parallel"
```

**Решение:**
- Булевы: `true` или `false`
- Числа: `5`, `3000`, `86400`
- Строки: `"info"`, `"localhost"`, `"npm"`
- Проверьте описание фичи для ожидаемого типа

## Связанные команды

- [mip config](/commands/config) - управление конфигурацией
- [mip --genconfig](/commands/genconfig) - генерация конфигурации фич
- [mip --list-features](/commands/list-features) - список всех фич
- [mip doctor](/commands/doctor) - диагностика системы
- [mip registry](/commands/registry) - управление реестрами