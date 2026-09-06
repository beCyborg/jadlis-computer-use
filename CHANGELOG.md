# Changelog — computer-use

Формат: [Keep a Changelog](https://keepachangelog.com/ru/1.1.0/), версии — [SemVer](https://semver.org/lang/ru/).

## [1.1.2] — 2026-09-07 — уточнение по screenshot / screenshot note

### Для человека
- В таблице инструментов отмечено, что у `screenshot()` в текущей сборке нет прежнего параметра `save_to_disk`.

### For agents
- Changed: `skills/computer-use/SKILL.md` — строка «Скриншот» в таблице инструментов.
- Changed: `.claude-plugin/plugin.json` — `version` 1.1.1 → 1.1.2.
- Migration: не требуется.

## [1.1.1] — 2026-09-06 — двуязычный README и гейты передачи / bilingual README and handover gates

### Для человека
- README плагина переписан по пяти секциям (Зачем / Как выглядит / Как поставить / Как пользоваться / Границы и стоимость), рядом появился английский `README.en.md`.
- Основной путь установки теперь через хаб `jadlis`; свой маркетплейс `becyborg-desktop` остался как альтернатива.
- В CI добавлена проверка на утечку секретов, в репозитории — конвенции для агентов (`CLAUDE.md`).

### For agents
- Added: `plugins/computer-use/README.en.md`, `plugins/computer-use/CHANGELOG.md`; в корне репо — `README.en.md`, `CLAUDE.md`, `docs/img/hub-12.webp`.
- Changed: `plugins/computer-use/README.md` — 5 секций + «Обновление», Mermaid-схема иерархии поверхностей, установка через `claude plugin install computer-use@jadlis`.
- Changed: `.github/workflows/plugin-validate.yml` — job `gitleaks` (`gitleaks/gitleaks-action@v2`, `fetch-depth: 0`) рядом с job `validate`.
- Changed: `plugins/computer-use/.claude-plugin/plugin.json` — `version` 1.1.0 → 1.1.1.
- Migration: не требуется, скилл и рецепты приложений не менялись.

## [1.1.0] — 2026-09-01 — обновление скилла по итогам ресерча / research-driven skill update

### Для человека
- Скилл научился заменять скриншоты текстовыми каналами (буфер обмена, файлы) — самый крупный выигрыш по контексту.
- `computer_batch` теперь идёт с хвостовым кадром: он верифицирует итог и становится базой координат следующего вызова.
- Добавлены диагностика (TCC-гранты, лок «одна сессия», чёрный кадр, чек-лист «тулов не видно») и накопитель рецептов под приложения.

### For agents
- Added: `plugins/computer-use/skills/computer-use/references/app-recipes.md` — deep-links Системных настроек, каналы извлечения результата, ловушки чат-приложений.
- Added: таблица валидных сигнатур инструментов и правила надёжности `type`/кликов (потеря фокуса, кириллица через буфер, невидимые слои).
- Changed: `skills/computer-use/SKILL.md` — иерархия поверхностей API/MCP/CLI → браузер → пиксели, правила экономии кадров, семантика `computer_batch`.
- Changed: `plugins/computer-use/.claude-plugin/plugin.json` — `version` 1.0.0 → 1.1.0.

## [1.0.0] — 2026-09-01 — первый релиз плагина / first plugin release

### Для человека
- Скилл-дисциплина для встроенного в Claude Code computer-use MCP: когда кадр нужен, чем его заменить, как не сжечь контекст петлёй «скриншот → клик → скриншот».
- Репозиторий приведён к домашнему стандарту: явный semver, `$schema` в манифестах, LICENSE, CI-валидация плагинов.

### For agents
- Added: `plugins/computer-use/` — `skills/computer-use/SKILL.md`, `.claude-plugin/plugin.json`.
- Added: маркетплейс `becyborg-desktop` в `.claude-plugin/marketplace.json`; `.github/workflows/plugin-validate.yml`; `LICENSE` (MIT).
- Changed: README синхронизирован с выверенной инструкцией — ветка отказа computer-use, механика обновления, имя маркетплейса в командах.
