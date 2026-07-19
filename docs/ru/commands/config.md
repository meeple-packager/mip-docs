---
title: "mip config - управление конфигурацией MIP"
description: "Команда config позволяет просматривать, изменять и управлять конфигурацией проекта MIP прямо из командной строки."
---

# mip config

::: callout info "Что это?" icon:gear
`mip config` предоставляет удобный CLI-интерфейс для управления конфигурацией вашего MIP-проекта. Вместо ручного редактирования `mip.yml` или `mip.json`, вы можете просматривать, устанавливать, удалять и сбрасывать значения конфигурации прямо из терминала. Идеально для быстрых правок, автоматизации в CI/CD и скриптов.
:::

## Синтаксис

```bash
mip config <подкоманда> [опции]
```

## Подкоманды

| Подкоманда | Описание |
|------------|----------|
| `list` | Показать всю конфигурацию в отформатированном виде |
| `get <ключ>` | Получить конкретное значение конфигурации |
| `set <ключ> <значение>` | Установить значение конфигурации |
| `delete <ключ>` | Удалить значение конфигурации |
| `reset` | Сбросить конфигурацию к значениям по умолчанию |
| `edit` | Открыть конфигурацию в редакторе |

## Опции

| Флаг | Описание |
|------|----------|
| `--json` | Вывод в формате JSON |
| `--yaml` | Вывод в формате YAML |

## Как это работает

1. **Обнаружение конфигурации** - MIP автоматически определяет ваш файл конфигурации (`mip.yml`, `mip.json` или `package.json`).

2. **Разрешение значений** - поддерживаются вложенные ключи с использованием точечной нотации (например, `registries.github.url`).

3. **Определение типа** - значения автоматически парсятся (строки, числа, JSON-объекты, массивы).

4. **Миграция файлов** - если вы используете устаревший формат конфига, MIP автоматически мигрирует на `mip.yml`.

5. **Интеграция с редактором** - использует переменные окружения `$EDITOR` или `$VISUAL` для подкоманды `edit`.

## Примеры

::: tabs
== tab "Просмотр всей конфигурации"
```bash
mip config list
```

**Вывод:**
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

== tab "Получить конкретное значение"
```bash
mip config get version
```
**Вывод:**
```
2.0.0
```

```bash
mip config get scripts.build
```
**Вывод:**
```
webpack --mode production
```

```bash
mip config get registries.github
```
**Вывод:**
```
{
  "url": "https://npm.pkg.github.com/",
  "token": "ghp_xxx"
}
```
:::

== tab "Установить значение"
```bash
mip config set version 2.1.0
```
**Вывод:**
```
✅ Config updated: version
   Old: "2.0.0"
   New: "2.1.0"
```

```bash
mip config set defaultRegistry github
```
**Вывод:**
```
✅ Config updated: defaultRegistry
   Old: "npm"
   New: "github"
```

```bash
mip config set registries.github.token ghp_new_token
```
**Вывод:**
```
✅ Config updated: registries.github.token
   Old: "ghp_xxx"
   New: "ghp_new_token"
```
:::

== tab "Удалить значение"
```bash
mip config delete registries.github.token
```
**Вывод:**
```
✅ Deleted: registries.github.token
   Old value: "ghp_xxx"
```
:::

== tab "Редактировать в редакторе"
```bash
mip config edit
```
**Вывод:**
```
📝 Opening mip.yml in vim...
✅ Config edited successfully
```
:::

== tab "Сброс к значениям по умолчанию"
```bash
mip config reset
```
**Вывод:**
```
⚠️ This will reset all configuration to default values
   All custom settings will be lost!
   Are you sure? (yes/no) yes
✅ Config reset to defaults
```
:::
:::

## Частые сценарии использования

### Быстрое изменение версии
```bash
# Перед публикацией
mip config set version 2.1.0
mip config get version  # Проверить
```

### Управление реестрами
```bash
# Установить реестр по умолчанию
mip config set defaultRegistry github

# Обновить токен реестра
mip config set registries.github.token ghp_new_token

# Добавить новый реестр
mip config set registries.gitlab.url https://gitlab.com/api/v4/packages/npm/
```

### Управление скриптами
```bash
# Добавить новый скрипт
mip config set scripts.deploy "npm run build && rsync -avz dist/ server:/app"

# Обновить существующий скрипт
mip config set scripts.test "jest --coverage"
```

### Управление рабочими пространствами
```bash
# Установить workspaces
mip config set workspaces '["packages/*", "apps/*"]'
```

## Поддерживаемые ключи

| Ключ | Тип | Описание |
|------|-----|----------|
| `name` | string | Имя проекта |
| `version` | string | Версия проекта |
| `description` | string | Описание проекта |
| `dependencies` | object | Производственные зависимости |
| `devDependencies` | object | Зависимости для разработки |
| `scripts` | object | NPM-скрипты |
| `workspaces` | array | Рабочие пространства монорепозитория |
| `registries` | object | Реестры пакетов |
| `defaultRegistry` | string | Имя реестра по умолчанию |
| `language` | string | Язык интерфейса (en/ru/etc) |

### Вложенные ключи
Вы можете обращаться к вложенным значениям, используя точечную нотацию:

```bash
mip config get registries.github.url
mip config set registries.github.token ghp_xxx
mip config delete registries.github.token
```

## Форматы вывода

### Формат JSON
```bash
mip config list --json
```
Полезно для скриптов и программного доступа.

### Формат YAML
```bash
mip config list --yaml
```
Показывает сырую YAML-конфигурацию.

## Частые ошибки

::: callout warning "❌ ERR_NO_CONFIG" icon:alert-triangle
**Причина:** Файл конфигурации не найден в текущей директории.

**Решение:**
1. Убедитесь, что вы находитесь в директории MIP-проекта.
2. Выполните `mip init` для создания нового проекта.
3. Или запустите `mip config` из корня проекта.
:::

::: callout danger "❌ ERR_KEY_NOT_FOUND" icon:skull
**Причина:** Указанный ключ не существует в конфигурации.

**Решение:**
1. Проверьте имя ключа: `mip config list` для просмотра всех ключей.
2. Используйте точечную нотацию для вложенных ключей: `registries.github.url`.
3. Убедитесь, что ключ существует перед обращением к нему.
:::

::: callout warning "⚠️ ERR_INVALID_VALUE" icon:alert-circle
**Причина:** Предоставленное значение имеет неверный формат.

**Решение:**
1. Для объектов используйте валидный JSON: `'{"key":"value"}'`
2. Для массивов используйте валидный JSON: `'["item1","item2"]'`
3. Для строк кавычки не требуются: `mip config set name my-project`
:::

## Связанные команды

- [mip registry](/commands/registry) - управление реестрами пакетов
- [mip init](/commands/init) - создание нового проекта
- [mip doctor](/commands/doctor) - проверка здоровья проекта

::: collapsible "🔧 Как работает разрешение конфигурации"
MIP следует определенному порядку при разрешении конфигурации:

1. **`mip.yml`** - основной файл конфигурации (рекомендуется)
2. **`mip.json`** - устаревший JSON-формат (будет мигрирован)
3. **`package.json`** - совместимость с npm (будет мигрирован)

Когда вы выполняете `mip config set`, MIP автоматически:
- Определяет активный файл конфигурации
- Обновляет значение
- Сохраняет обратно в `mip.yml`

Если ваш проект использует `package.json` или `mip.json`, MIP автоматически выполнит миграцию на `mip.yml` при первой операции записи.
:::

::: callout tip "💡 Совет: Конфигурация как код" icon:lightbulb
Используйте `mip config` в ваших CI/CD пайплайнах для динамической конфигурации:

```bash
# Установить конфигурацию для окружения
mip config set apiUrl https://api.production.com
mip config set logLevel info

# Затем запустить сборку
mip run build
```
:::
