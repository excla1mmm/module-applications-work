# 010: комментарий в LIC-612 — проверка защиты ветки (M03)
Тип: jira-write
Ветка/коммит: claude/stoic-goldberg-n01ons @ 6ef1783 (docs/branch-protection.md, раздел «In the sandbox»)

## Что нужно
`jira.sh comment-add LIC-612 -` с текстом ниже (markdown):

```
Защита ветки в песочнице проверена (ruleset «main: applications gate», id 25947319, вариант A). Bypass для проверок не использовался.

Конфигурация (прочитана через API): deletion и non_fast_forward запрещены; pull_request — 1 одобрение, dismiss_stale_reviews_on_push=true; required_status_checks strict: result, gather, terms-accepted, agreement, AI review; bypass — Repository admin только для PR. Отличия от боевой конфигурации (docs/branch-protection.md): require_last_push_approval и code owner review выключены — в песочнице один человек и нет CODEOWNERS; в боевом репозитории включить.

Проверено:
- Прямой push в main отклонён: «GH013: Repository rule violations ... Changes must be made through a pull request. 5 of 5 required status checks are expected.», main не изменился (7b688f8).
- PR с красными тестами не мержится: #8 (ModuleAppBroken) — result failure, статуса «AI review» нет (AI не запускался), состояние blocked/behind. https://github.com/excla1mmm/module-applications/pull/8
- Push после AI-ревью сбрасывает его актуальность: у #6 проверенный head имел «AI review» = success; после нового коммита (90adbc2) result и gather зелёные, но статуса «AI review» на новом head нет, mergeable_state = blocked, новое AI-ревью ждёт подтверждения: https://github.com/excla1mmm/module-applications/pull/6 , https://github.com/excla1mmm/module-applications/actions/runs/40484617919
- Force push не выполнялся намеренно: если бы правило не сработало, он переписал бы историю main; правило non_fast_forward активно (API).

Не проверено одним аккаунтом: сброс человеческого одобрения при push. Функция GitHub включена (dismiss_stale_reviews_on_push), но автор не может одобрить свой PR. Проверка: второй аккаунт с правом записи одобряет PR → автор пушит коммит → одобрение помечено dismissed, merge снова заблокирован. Нужен второй аккаунт или боевой репозиторий.
```

## Зачем
M03: комментарий в LIC-612 с результатом проверки и ссылками.

## Ожидаемый результат
Подтверждение и id комментария.
