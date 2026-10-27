# M03 (от Mac / пользователя → облаку): защита ветки включена — проверить и закрыть LIC-612

## Что сделано
В `excla1mmm/module-applications` создан ruleset **`main: applications gate`** (id 25947319), active, по
`docs/branch-protection.md`, вариант **A** (согласован с владельцем):
- deletion, non_fast_forward (force push) — запрещены;
- pull_request: 1 одобрение, **dismiss stale reviews on push — вкл**, code owners — выкл (CODEOWNERS пока нет),
  **require_last_push_approval — выкл** (в песочнице один человек; в боевом репозитории — включить, отметь в доке);
- required status checks (strict, up to date): `result`, `gather`, `terms-accepted`, `agreement`, `AI review`;
- bypass: Repository admin, **только для pull requests** (для PR image-update).
«Allow GitHub Actions to create and approve PRs» остаётся включённым (нужно для A).

## Ограничение песочницы
Все PR открыты от `excla1mmm` — он же единственный человек и админ. GitHub не даёт автору одобрить свой PR, а админ
может обойти правила только явным bypass. Поэтому:
- проверь блокировку без bypass: merge красного PR (#8) и PR, у которого после AI-ревью сделан push (новый head без
  статуса `AI review`), — должен быть отказ;
- проверь, что прямой push в main и force push отклоняются;
- сброс человеческого одобрения при push — встроенная функция GitHub (dismiss stale); одним аккаунтом не проверить —
  зафиксируй это и способ проверки (второй аккаунт / боевой репозиторий). Нужен второй аккаунт — запрос пользователю.
Не используй bypass для проверочных merge.

## Результат
1. Запрос jira-write: комментарий в LIC-612 с результатом проверки (ссылки).
2. Если все критерии LIC-612 выполнены — запрос jira-write на `done LIC-612` с полным итогом (как 009). Пользователь
   разрешил закрывать готовые задачи; закрою после сверки.

LIC-614 не начинать: API регистрации LIC-613 ещё не сделан, задача остаётся заблокированной.
