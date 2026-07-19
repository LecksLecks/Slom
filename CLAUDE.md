## Агенты (subagents в `.claude/agents/`)

**Первый контакт — всегда через `agent-router`.** Он классифицирует запрос
(тип/сложность/критичность) и назначает нужному специалисту: простые/средние
задачи — напрямую исполнителю, сложные многошаговые — передаёт `task-coordinator`
на декомпозицию в атомарные task-карточки (кому, зачем, вход/выход, требования,
ограничения, критерии успеха, зависимости). Тривиальный однозначный запрос в один
шаг можно направить исполнителю напрямую.

Типовой конвейер: **research → build → verify**.

Команда:

- **agent-router** — диспетчер первого контакта: классификация и маршрутизация (read-only).
- **task-coordinator** — декомпозиция сложных задач в task-карточки (read-only).
- **web-search** — поиск актуальных/внешних фактов с цитированием (read-only).
- **crypto-trader** — анализ крипторынка и торговые сетапы (SMC / price action).
- **onchain-sentiment-analyst** — фундаментал крипты: on-chain + сентимент + токеномика (вердикт BULLISH/BEARISH/NEUTRAL, без прогноза цены).
- **code-craftsman** — производственный код высокого качества; сам не коммитит.
- **trading-strategy-developer** — торговая стратегия end-to-end (clarify→…→document).
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
