# Skills — Agent Skills и `/find-skills`

> Проект базы знаний для хранения полезных Agent Skills, которые расширяют возможности coding-агентов готовыми инструкциями, workflow и специализированными знаниями.

## Зачем нужен проект

`Skills` — это слой переиспользуемых навыков для AI-агентов. Вместо того чтобы каждый раз вручную объяснять агенту процесс, можно подключить готовый skill с инструкциями для конкретной задачи: React, тестирование, code review, deployment, документация, дизайн, DevOps и т.д.

Главная идея: **агент получает специализированный workflow как подключаемый модуль**.

Полезно для нашей схемы agent-driven / Harness Engineering разработки с Claude Code, Codex и другими coding agents.

---

## `/find-skills` от Vercel Labs

Источник: https://github.com/vercel-labs/skills/tree/main/skills/find-skills

Каталог: https://skills.sh/vercel-labs/skills/find-skills

Документация Vercel Agent Skills: https://vercel.com/docs/agent-resources/skills

### Что это

`find-skills` — meta-skill от **Vercel Labs**, который учит AI-агента самостоятельно искать подходящие Agent Skills в открытой экосистеме `skills.sh` и предлагать их установку.

Skill особенно полезен, когда пользователь спрашивает:

- «как сделать X?»;
- «есть skill для X?»;
- «найди skill для React / FastAPI / тестирования / code review»;
- «можно расширить возможности агента для этой задачи?»;
- «найди готовый workflow или специализированный инструмент».

Идея проста:

```text
Задача пользователя
      ↓
/find-skills
      ↓
поиск подходящего Agent Skill
      ↓
проверка источника и популярности
      ↓
предложение skill
      ↓
установка
      ↓
агент использует новый workflow
```

---

## Установка `/find-skills`

Основной вариант:

```bash
npx skills add vercel-labs/skills@find-skills
```

Альтернативная команда из skills.sh:

```bash
npx skills add https://github.com/vercel-labs/skills --skill find-skills
```

Глобальная установка без интерактивного подтверждения:

```bash
npx skills add vercel-labs/skills@find-skills -g -y
```

---

## Основные команды Skills CLI

### Найти skill

```bash
npx skills find <query>
```

Примеры:

```bash
npx skills find react performance
npx skills find playwright testing
npx skills find pr review
npx skills find docker deployment
npx skills find fastapi
```

Можно ограничить поиск конкретным GitHub owner:

```bash
npx skills find react --owner vercel-labs
```

### Установить skill

```bash
npx skills add <package>
```

### Обновить установленные skills

```bash
npx skills update
```

> Старый интерфейс `npx add-skill` deprecated. Vercel рекомендует использовать `npx skills`.

---

## Как `/find-skills` выбирает хороший skill

Vercel рекомендует не устанавливать первый найденный результат автоматически. Перед рекомендацией нужно проверить:

1. **Количество установок** — предпочтительнее проверенные skills с большим числом установок.
2. **Репутацию автора** — например `vercel-labs`, `anthropics`, `microsoft` и другие известные источники.
3. **GitHub repository** — активность проекта, stars, обновления и содержимое `SKILL.md`.
4. **Соответствие задаче** — skill должен решать именно текущую инженерную задачу.

Это хорошо вписывается в Harness Engineering: skill рассматривается как внешняя зависимость и должен проходить минимальную проверку перед подключением.

---

## Как использовать в нашем workflow

Рекомендуемый сценарий:

```text
1. Формулируем задачу агенту
2. Если нужен специализированный workflow → используем /find-skills
3. Агент ищет кандидатов через Skills CLI / skills.sh
4. Проверяем автора, репозиторий и назначение
5. Устанавливаем выбранный skill
6. Агент выполняет задачу с учетом SKILL.md
7. Полезный skill фиксируем в этой базе знаний
```

Пример запроса агенту:

```text
/find-skills
Найди качественный skill для проверки архитектуры FastAPI-проекта и code review.
Сначала проверь источник и репозиторий, затем предложи 2–3 лучших варианта и команды установки.
```

---

## Где применять

Особенно полезно для:

- [[Claude Code]] / coding agents;
- Codex;
- React / Next.js / TypeScript;
- Python / FastAPI;
- тестирования и Playwright;
- Git / GitHub / code review;
- Docker / DevOps / deployment;
- документации;
- UI/UX и accessibility;
- автоматизации повторяемых инженерных процессов.

---

## Статус

- Источник: **Vercel Labs**
- Репозиторий: `vercel-labs/skills`
- Skill: `find-skills`
- CLI: `npx skills`
- Каталог: `skills.sh`
- Назначение: discovery + установка Agent Skills
- Добавлено в базу знаний: 2026-09-10

## Связанные заметки

- [[Полезные плагины]]
- [[Claude Code — структура AI-assisted проекта]]
- [[Harness Engineering — транскрипт видео]]
