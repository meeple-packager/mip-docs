---
title: "mip alias - командные сокращения"
description: "Команда alias позволяет создавать, просматривать и удалять пользовательские сокращения для часто используемых команд MIP."
---

# mip alias

::: callout info "Что это?" icon:⚡
`mip alias` позволяет создавать собственные сокращения для команд MIP. Хватит печатать длинные команды - используйте свои аббревиатуры и ускорьте работу.
:::

## Синтаксис

```bash
mip alias <подкоманда> [опции]
```

## Подкоманды

| Подкоманда | Описание |
|------------|----------|
| `set <имя> <команда>` | Создать или обновить алиас |
| `remove <имя>` | Удалить алиас |
| `list` | Показать все настроенные алиасы |
| `clear` | Удалить все алиасы |
| `help` | Показать эту справку |

## Как это работает

1. **Алиасы хранятся** в `~/.mip/aliases.yml` (глобально, не привязаны к проекту)
2. **Разрешение** происходит перед выполнением команды - MIP проверяет, совпадает ли команда с каким-либо алиасом
3. **Поддерживаются пробелы** в именах алиасов (используйте кавычки)
4. **Поддерживаются аргументы** - всё, что после алиаса, передаётся целевой команде
5. **Самый длинный алиас** - если несколько алиасов совпадают, используется самый длинный

## Примеры

::: tabs
== tab "Простой алиас"
```bash
# Создать сокращение для install
mip alias set i install

# Использовать
mip i lodash
```

**Вывод:**
```
✅ Алиас установлен: i → install
   Хранится в: ~/.mip/aliases.yml
```
:::

== tab "Алиас с пробелами"
```bash
# Создать алиас с пробелами в имени
mip alias set "mip pe calc" "mip run calc"

# Использовать
mip pe calc 2 plus 2
```

**Вывод:**
```
🧮 2 + 2 = 4
```
:::

== tab "Алиас с аргументами"
```bash
# Создать алиас для сборки с флагами
mip alias set "mip build fast" "mip run build --mode production"

# Использовать
mip build fast
# → mip run build --mode production
```
:::

== tab "Список всех алиасов"
```bash
mip alias list
```

**Вывод:**
```
📋 Алиасы MIP:

  i          →  install
  up         →  update
  "mip pe calc"  →  mip run calc

💡 Всего: 3 алиаса
📁 Файл: ~/.mip/aliases.yml
```
:::

== tab "Удаление алиаса"
```bash
# Удалить по имени
mip alias remove i
```

**Вывод:**
```
✅ Алиас удалён: i (был → install)
```

Удаление алиаса с пробелами:
```bash
mip alias remove "mip pe calc"
# или
mip alias remove mip pe calc
```
:::

== tab "Очистка всех алиасов"
```bash
mip alias clear
```

**Вывод:**
```
⚠️ Это удалит все 3 алиаса
   Вы уверены? (y/N) y
✅ Удалены все 3 алиаса
```
:::
:::

## Формат файла алиасов

Алиасы хранятся в `~/.mip/aliases.yml`:

```yaml
i: install
up: update
"mip pe calc": "mip run calc"
"mip build fast": "mip run build --mode production"
```

Вы можете редактировать этот файл напрямую - MIP подхватит изменения при следующем запуске.

## Частые сценарии использования

### Сокращение повседневных команд
```bash
mip alias set i install
mip alias set up update
mip alias set ls list
mip alias set rm uninstall
mip alias set s search
mip alias set st status
mip alias set ci "mip ci --frozen-lockfile"
```

### Сокращения для команд плагинов
```bash
mip alias set "build" "mip pe mip-webpack build"
mip alias set "dev" "mip pe mip-webpack dev"
```

### Сложные команды
```bash
# Деплой с несколькими шагами
mip alias set "deploy" "mip run build && mip run test && mip run deploy"

# Чистая установка
mip alias set "fresh" "mip cache clean && mip install"
```

## Частые ошибки

::: callout warning "❌ ERR_ALIAS_NOT_FOUND" icon:alert-triangle
**Причина:** Указанный алиас не существует.

**Решение:**
1. Проверьте точное имя: `mip alias list`
2. Алиасы с пробелами требуют кавычек: `mip alias remove "мой алиас"`
3. Убедитесь, что вы правильно ввели имя
:::

::: callout danger "❌ ERR_RESERVED_NAME" icon:skull
**Причина:** Имя алиаса конфликтует с зарезервированной командой MIP.

**Решение:**
1. Зарезервированные имена нельзя использовать для алиасов: `alias`, `config`, `registry`, `server`, `publish`, `help`
2. Выберите другое имя для алиаса
:::

::: callout warning "⚠️ ERR_ALIAS_OVERWRITE" icon:alert-circle
**Причина:** Алиас с таким именем уже существует.

**Решение:**
1. Вам будет предложено подтвердить перезапись
2. Введите `y` для перезаписи, `n` для отмены
3. Или сначала выполните `mip alias remove <имя>`, затем установите заново
:::

## Как работает разрешение алиасов

Когда вы запускаете команду, MIP проверяет:

1. **Точное совпадение** - если вся команда совпадает с именем алиаса
2. **Самый длинный алиас** - если несколько алиасов совпадают, побеждает самый длинный
3. **Передача аргументов** - оставшиеся аргументы передаются целевой команде

**Пример:**
```
Алиасы:
  "build" → "mip run build"
  "build fast" → "mip run build --production"

Ввод: mip build fast --force
Совпадение: "build fast" (самый длинный)
Результат: mip run build --production --force
```

## Связанные команды

- [mip config](/commands/config) - управление конфигурацией MIP
- [mip plugin](/commands/plugin) - управление плагинами (которые часто предоставляют дополнительные команды)
- [mip pe](/commands/pe) - выполнение команд плагинов (часто используется в алиасах)

::: collapsible "🔧 Алиасы MIP vs Алиасы оболочки"
**Алиасы MIP** работают внутри команд MIP и переносимы между любыми оболочками.

**Алиасы оболочки** (в `.bashrc` или `.zshrc`) работают на уровне оболочки:
```bash
# Алиас оболочки (в .zshrc)
alias mip='mip --super'

# Алиас MIP (хранится в ~/.mip/aliases.yml)
mip alias set i install
```

**Когда что использовать:**
- Алиасы оболочки - для глобальных флагов MIP (`--super`, `--debug`)
- Алиасы MIP - для подстановки команд (`i` → `install`)
:::

::: callout tip "💡 Совет: Делитесь алиасами" icon:lightbulb
Скопируйте файл `~/.mip/aliases.yml` на другие машины, чтобы сохранить свои сокращения во всех рабочих окружениях.
:::
