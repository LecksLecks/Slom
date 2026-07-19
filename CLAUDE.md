## Агенты (subagents в `.claude/agents/`)

**Первый контакт — всегда через `task-coordinator`.** Любой нетривиальный,
многошаговый, неоднозначный или кросс-ролевой запрос сначала прогоняй через
`task-coordinator`: он декомпозирует задачу на атомарные task-карточки (кому,
зачем, вход/выход, требования, ограничения, критерии успеха, зависимости), а
затем оркестратор раздаёт карточки названным специалистам. Тривиальный
однозначный запрос в один шаг можно направить исполнителю напрямую.

Типовой конвейер: **research → build → verify**.

Команда:

- **task-coordinator** — точка входа: декомпозиция и маршрутизация задач (read-only).
- **web-search** — поиск актуальных/внешних фактов с цитированием (read-only).
- **crypto-trader** — анализ крипторынка и торговые сетапы (SMC / price action).
- **onchain-sentiment-analyst** — фундаментал крипты: on-chain + сентимент + токеномика (вердикт BULLISH/BEARISH/NEUTRAL, без прогноза цены).
- **code-craftsman** — производственный код высокого качества; сам не коммитит.
- **trading-strategy-developer** — торговая стратегия end-to-end (clarify→…→document).
- **quant-analyst** — валидатор бэктестов: OOS/walk-forward/Monte-Carlo, overfitting (вердикт APPROVED/CONDITIONAL/REJECTED).
- **prompt-engineer** — проектирование/улучшение промптов, агентов, скиллов.
- **work-verifier** — независимый read-only аудит готовой работы (вердикт PASS/FAIL).

## Agent skills

### Issue tracker

Issues live in GitHub Issues on `LecksLecks/Slom`. See `docs/agents/issue-tracker.md`.

### Triage labels

Default canonical labels (`needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix`). See `docs/agents/triage-labels.md`.

### Domain docs

Single-context — `CONTEXT.md` and `docs/adr/` at the repo root. See `docs/agents/domain.md`.
