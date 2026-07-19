## Маршрутизация запросов

**Первый контакт — всегда через скилл `agent-router`** (`.claude/skills/agent-router/`,
вызывается через `Skill`, не через `Agent` — это не отдельный субагент, а моя
собственная процедура классификации). Классифицирую запрос (тип/сложность/
критичность) сама и решаю, кому его передать: простые/средние задачи —
напрямую исполнителю через `Agent`, сложные многошаговые — сначала через скилл
`task-coordinator` (`.claude/skills/task-coordinator/`) на декомпозицию в
атомарные task-карточки (кому, зачем, вход/выход, требования, ограничения,
критерии успеха, зависимости). Тривиальный однозначный запрос в один шаг можно
направить исполнителю напрямую, без формального прогона через скиллы.

Оба скилла — `disable-model-invocation: true`: это правило и есть их триггер,
а не автосрабатывание по описанию (не задваивает контекстную нагрузку).

Типовой конвейер: **research → build → verify**.

## Агенты (subagents в `.claude/agents/`)

- **web-search** — поиск актуальных/внешних фактов с цитированием (read-only).
- **crypto-trader** — анализ крипторынка и торговые сетапы (SMC / price action).
- **onchain-sentiment-analyst** — фундаментал крипты: on-chain + сентимент + токеномика (вердикт BULLISH/BEARISH/NEUTRAL, без прогноза цены).
- **code-craftsman** — производственный код высокого качества; сам не коммитит.
- **bug-diagnostician** — систематическая диагностика трудных багов и регрессий (петля → репродукция → гипотезы → фикс+регрессионный тест); сам не коммитит.
- **trading-strategy-developer** — торговая стратегия end-to-end (clarify→…→document).
- **data-engineer** — добывает/чистит/валидирует данные (OHLCV, funding, OI) в датасет для бэктеста; сам не коммитит.
- **quant-analyst** — валидатор бэктестов: OOS/walk-forward/Monte-Carlo, overfitting (вердикт APPROVED/CONDITIONAL/REJECTED).
- **risk-manager** — размер позиции, портфельная экспозиция/корреляция, лимиты просадки (вердикт APPROVED/REDUCE SIZE/REJECTED).
- **prompt-engineer** — проектирование/улучшение промптов, агентов, скиллов.
- **work-verifier** — независимый read-only аудит готовой работы (вердикт PASS/FAIL).
- **token-optimizer** — сжимает раздутый текст/ответ на 30–50% без потери сути (read-only рефакторинг).

### Экономия токенов — обязательна для всех агентов

Каждый агент в `.claude/agents/` держит раздел «Экономия токенов» / «Token economy»:
без вводных фраз, без повторов, без очевидного, таблицы вместо прозы, примеры по
требованию — но **обязательный формат вывода агента не режется**, экономится только
проза вокруг него. **Новый агент обязан включать этот раздел** (см. формулировку в
любом существующем агенте как образец). Для сжатия уже готового текста/ответа —
`token-optimizer`.

## Agent skills

### Issue tracker

Issues live in GitHub Issues on `LecksLecks/Slom`. See `docs/agents/issue-tracker.md`.

### Triage labels

Default canonical labels (`needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix`). See `docs/agents/triage-labels.md`.

### Domain docs

Single-context — `CONTEXT.md` and `docs/adr/` at the repo root. See `docs/agents/domain.md`.
