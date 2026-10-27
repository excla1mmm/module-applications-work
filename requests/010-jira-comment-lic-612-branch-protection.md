# 010 (редакция 2): комментарий в LIC-612 — проверка защиты ветки (M03) без bypass
Тип: jira-write
Ветка/коммит: claude/stoic-goldberg-n01ons @ 76d7a24 (docs/branch-protection.md, разделы «The image update pull request» и «In the sandbox»)

Заменяет первую редакцию 010 (не опубликована, см. 010-…answer.md).

## Что нужно
`jira.sh comment-add LIC-612 -` с текстом ниже (markdown):

```
Защита ветки в песочнице проверена повторно, уже без bypass (ruleset «main: applications gate», id 25947319).

Находка. Ruleset сначала создавался с вариантом A (bypass «Repository admin, только PR») — для PR обновления образа. Через ~13 минут PR #6 был смержен в main через этот bypass вручную, хотя у его head (90adbc2) не было статуса «AI review», а agreement был красным. Агенты #6 не мержили. Вывод: вариант A позволяет админу — и агенту, действующему через GitHub App от его имени, — смержить любой PR в обход gate; соглашение «только image-update» ничем не обеспечено. Исправлено: bypass убран (bypass_actors = []), #6 откачен PR #10 (смержен при кратковременно отключённом ruleset, оба переключения в аудите). Для боевого репозитория: bypass пустой; PR обновления образа — вариант B (GitHub App, обычные проверки) или ручное временное отключение ruleset с записью в аудите. https://github.com/excla1mmm/module-applications/pull/6 , https://github.com/excla1mmm/module-applications/pull/10

Конфигурация (API): enforcement active, bypass_actors [], current_user_can_bypass never; deletion и non_fast_forward запрещены; pull_request — 1 одобрение, dismiss_stale_reviews_on_push=true; required_status_checks strict: result, gather, terms-accepted, agreement, AI review. Отличия от боевой конфигурации: require_last_push_approval и code owner review выключены (один человек, нет CODEOWNERS) — в боевом включить.

Проверено без bypass:
- Прямой push в main отклонён: «GH013: Repository rule violations ... Changes must be made through a pull request. 5 of 5 required status checks are expected.», main не изменился (42d9f17).
- Merge через API от имени админа отклонён для PR с красными тестами (#8, ModuleAppBroken: result failure, статуса AI review нет): «405 Repository rule violations found. At least 1 approving review is required ... 5 of 5 required status checks are expected.» https://github.com/excla1mmm/module-applications/pull/8
- То же для PR с блокерами AI-ревью (#7, AI review = failure): 405. https://github.com/excla1mmm/module-applications/pull/7
- Push после AI-ревью: служебный пробный PR #11 — у head 2bdd585 «AI review» = success; после нового коммита 1a975f4 статусов нет, merge через API сразу после push отклонён: «405 ... 5 of 5 required status checks have not succeeded: 2 expected.» PR закрыт без merge. https://github.com/excla1mmm/module-applications/pull/11
- Force push не выполнялся намеренно (при отказе правила переписал бы историю main); non_fast_forward активно (API).

Не проверено одним аккаунтом: сброс человеческого одобрения при push. Функция включена (dismiss_stale_reviews_on_push), но автор не может одобрить свой PR. Проверка: второй аккаунт с правом записи одобряет PR → автор пушит коммит → одобрение dismissed, merge снова заблокирован.
```

## Зачем
M03 после ответа 010: проверка без bypass и находка про вариант A.

## Ожидаемый результат
Подтверждение и id комментария.
