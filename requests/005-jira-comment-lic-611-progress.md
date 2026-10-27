# 005: комментарий в LIC-611 — прогресс этапа 1 и выбор module-template
Тип: jira-write
Ветка/коммит: claude/stoic-goldberg-n01ons @ bda6b66

## Что нужно
`jira.sh comment-add LIC-611 -` с текстом ниже (markdown):

```
Этап 1 — реализовано и проверено локально (x86_64), ветка claude/stoic-goldberg-n01ons, коммиты ecda607, 33697f7, bda6b66 (рабочий репозиторий, каталог module-applications/).

Формат заявки: schema/application.schema.json (schema_version 1) по черновику §4.3 — module_id (^Module[A-Za-z0-9]{1,60}$ — пересечение Core, REST v3 и спецификации маркетплейса), publisher_id, name, description, repo (публичный GitHub), commit (только полный 40-hex), license (SPDX), support_email, maintainers, homepage, compatibility {min/max_pbx_version, architectures}. Шаблон templates/application.yaml. Операторские ключи (trust_level, paid, images, rules, budget, approved…) PR задать не может — APP-002.
Правила: rules/rules.yaml, версия 0.1.0, 38 требований с id (APP/PKG/MJ/CODE/LIFE), mandatory отдельно от recommended; доказательства — ссылки на Core develop @e7e2109; слабые помечены draft для LIC-319.
tools/validate.py (по образцу oss-scanner, коды выхода 0/1, формат «<id>: [RULE] сообщение»): офлайн — схема, whitelist, имя каталога = module_id, уникальность module_id без учёта регистра, только application.yaml в каталоге; онлайн — коммит существует (git fetch точного SHA), module.json на SHA (moduleUniqueID, min_pbx_version согласован с compatibility, support_email, version, lic_*), Setup/PbxExtensionSetup.php и namespace, пути/симлинки/лимиты; --pr-paths — PR трогает только один applications/<id>/.
Сверка support_email с карточкой издателя и владельца module_id на сервере — интерфейс tools/publisher_registry.py, сейчас заглушка: статус not_checked, не считается пройденным; реальная проверка — после API LIC-613.
Фикстуры: 3 правильные заявки + 21 испорченная (пропущенные поля, короткий SHA, ветка, верхний регистр, несуществующий коммит и репозиторий, конфликт GUID ×2, несовместимый module.json, чужой moduleUniqueID, нарушенный контракт module.json, операторский ключ, лишний файл, файл вне каталога, две заявки в одном PR и др.), у каждой ожидаемое правило. pytest: 49 passed (онлайн-случаи — на локальных git-репозиториях с воспроизводимыми SHA, без сети).
Контрольный модуль ModuleAppCheck (на базе ModuleTemplate: воркер + REST check) с заявкой и ожидаемыми результатами (tests/modules/expected.yaml): проходит установку, обновление 1.0.0→1.1.0, включение, воркер, API, отключение, удаление.
Документация RU/EN: README, CONTRIBUTING (новая заявка, обновление, отзыв, что получает следующий этап, repo ≠ source_url). TERMS.md и PUBLISHER_AGREEMENT.md — черновики «текст утверждает MIKO», без юридического текста. PR template с чек-листом. Workflows terms/cla/validate по паттернам oss-scanner (pull_request_target, base checkout, тело PR только через env, triage в два job gather/comment).
Решение по шаблону модуля: каталог module-template/ внутри module-applications, а не доработка mikopbx/ModuleTemplate. Причины: шаблон версионируется вместе с правилами и схемой и проверяется тем же тестом (заявка-шаблон + module-template проходят validate --online); не меняем репозиторий MIKO вне задачи; ModuleTemplate остаётся полным примером с UI. Сборка ZIP из SHA — tools/package (воспроизводимый ZIP, %ModuleVersion% → 0.0.0-<коммит>).
Осталось для закрытия: прогон в песочнице GitHub (пробная заявка по закреплённому SHA) и arm64.
```

## Зачем
Комментарий о находках и решениях (EXECUTOR_PROMPT.md, «Цикл в Jira»; выбор module-template — «обоснуй в Jira»).

## Ожидаемый результат
Подтверждение и id комментария.
