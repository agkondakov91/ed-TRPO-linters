# 🐍 Python Linters — учебный курс

Материалы курса по настройке линтеров в Python-проекте. Курс охватывает полный цикл: от понимания того, зачем нужен линтер, до автоматической проверки кода на сервере через GitHub Actions.

---

## 📚 Содержание курса

| # | Лекция            | Тема                                           |
|---|-------------------|------------------------------------------------|
| 1 | Что такое линтер? | PEP 8, виды линтеров, линтер vs форматтер      |
| 2 | Ruff              | uv, ruff check, чтение вывода, --fix           |
| 3 | Настройка Ruff    | Категории правил, pyproject.toml, ignore, noqa |
| 4 | Pre-commit        | git hooks, .pre-commit-config.yaml             |
| 5 | GitHub Actions    | workflow, jobs, steps, push/PR проверки        |

---

## 🛠️ Стек

- **Python** 3.14+
- **[uv](https://docs.astral.sh/uv/)** — управление окружением и зависимостями
- **[Ruff](https://docs.astral.sh/ruff/)** — линтер и форматтер
- **[pre-commit](https://pre-commit.com/)** — автоматические проверки перед коммитом
- **[GitHub Actions](https://github.com/features/actions)** — CI-проверки на сервере

---

## 🚀 Быстрый старт

### 1. Клонировать репозиторий

```bash
git clone git@github.com:agkondakov91/ed-TRPO-linters.git
cd ed-TRPO-linters
```

### 2. Установить зависимости
Для управления зависимостями используется пакетный менеджер `uv`. Если `uv` ещё не установлен, ставим его командой:

```bash
# macOS и Linux
curl -Lsf https://astral.sh/uv/install.sh | sh

# Windows (PowerShell)
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
```

Проверяем, что всё установилось:
```bash
uv --version
# uv 0.4.x (или новее)
```

После этого можно установить зависимости:
```bash
uv sync --dev
```

### 3. Подключить git-хуки

```bash
uv run pre-commit install
```

После этого Ruff будет запускаться автоматически перед каждым `git commit`.

---

## 🔧 Полезные команды

### Линтер

```bash
# Проверить весь проект
uv run ruff check .

# Проверить и автоматически исправить
uv run ruff check --fix .

# Узнать подробности о конкретном правиле
uv run ruff rule F401
```

### Форматтер

```bash
# Отформатировать весь проект
uv run ruff format .

# Проверить форматирование без изменений
uv run ruff format --check .
```

### pre-commit

```bash
# Запустить проверку на всех файлах вручную
uv run pre-commit run --all-files

# Обновить версии хуков до актуальных
uv run pre-commit autoupdate
```

---

## ⚙️ Конфигурация

### pyproject.toml

Настройки Ruff хранятся в `pyproject.toml`:

```toml
[tool.ruff]
line-length = 88

[tool.ruff.lint]
select = ["E", "W", "F", "N", "I"]
ignore = ["E501"]

[tool.ruff.lint.pep8-naming]
ignore-names = ["setUp", "tearDown"]
```

**Ruff** поддерживает специальное значение `ALL`. Оно включает абсолютно все правила:

```toml
[tool.ruff.lint]
select = ["ALL"]
```

| Категория | Что проверяет |
|-----------|--------------|
| `E`       | Стилевые ошибки (пробелы, отступы) |
| `W`       | Стилевые предупреждения |
| `F`       | Логические проблемы (неиспользуемые импорты и переменные) |
| `N`       | Именование (`snake_case`, `PascalCase`) |
| `I`       | Порядок импортов |

### .pre-commit-config.yaml

```yaml
repos:
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.15.13
    hooks:
      - id: ruff
        args: [--fix]
      - id: ruff-format
```

### GitHub Actions

Workflow запускается при `push` и `pull_request` в ветки `main` / `master`:

```
.github/workflows/lint.yml
```

---

## 📂 Структура проекта

```
.
├── .github/
│   └── workflows/
│       └── lint.yml              # CI-пайплайн
├── .pre-commit-config.yaml       # хуки для git
├── pyproject.toml                # настройки проекта и Ruff
├── main.py                       # учебный файл с примерами
└── README.md
```

---

## 💡 Как работает защита кода

В проекте настроено два уровня проверки:

```
Локально                          На сервере
─────────────────────────         ─────────────────────────────
git commit                        git push / pull request
↓                                 ↓
pre-commit запускает Ruff         GitHub Actions запускает Ruff
↓                                 ↓
Ошибки → коммит отменён           Ошибки → красный крест ✗
Всё чисто → коммит прошёл         Всё чисто → зелёная галочка ✓
```

---

## 📖 Полезные ссылки

- [Документация Ruff](https://docs.astral.sh/ruff/)
- [Все правила Ruff](https://docs.astral.sh/ruff/rules/)
- [Документация uv](https://docs.astral.sh/uv/)
- [Документация pre-commit](https://pre-commit.com/)
- [PEP 8 — руководство по стилю Python](https://peps.python.org/pep-0008/)

---

## 👨‍🏫 Об авторе

Курс подготовлен для студентов, изучающих Python-разработку.  
Преподаватель: **Александр** / **[agkondakov91](https://github.com/agkondakov91)**