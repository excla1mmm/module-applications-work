# 009: закрыть LIC-611 (done) с итогом — ТОЛЬКО после подтверждения пользователя
Тип: jira-write
Ветка/коммит: claude/stoic-goldberg-n01ons @ 9a9c08a

## Что нужно
После подтверждения пользователя: `jira.sh done LIC-611 --comment -` с текстом ниже (markdown):

```
Итог LIC-611 (module-template и PR-манифест в module-applications).

Что сделано (рабочий репозиторий excla1mmm/module-applications-work, ветка claude/stoic-goldberg-n01ons, каталог module-applications/; песочница excla1mmm/module-applications):
- Схема заявки schema/application.schema.json (schema_version 1) и templates/application.yaml: module_id, publisher_id, описание, совместимость (min/max MikoPBX, архитектуры → тестовая матрица), лицензия SPDX, публичный support_email, maintainers, repo (только публичный GitHub, отделён от source_url каталога), commit — только полный 40-hex SHA. Операторские ключи PR задать не может.
- Правила rules/rules.yaml (версия 0.2.2): требования с id, обязательный контракт MikoPBX (PKG/MJ/CODE/LIFE/SEC) отдельно от рекомендаций, доказательства — ссылки на Core develop; спорные помечены draft для LIC-319.
- tools/validate.py (детерминированный, по образцу oss-scanner): офлайн — схема, whitelist, каталог = module_id, уникальность module_id без учёта регистра; онлайн — коммит существует, module.json на SHA (moduleUniqueID, min_pbx_version согласован с compatibility, support_email, version, lic_*), Setup и namespace, пути/симлинки/лимиты; PR трогает только один applications/<id>/.
- Сверка support_email с карточкой издателя и владельца module_id на сервере — явная заглушка tools/publisher_registry.py (статус not_checked, не считается пройденным) до API LIC-613; решение владельца — подключить позже.
- Фикстуры: правильные заявки + 20 испорченных (пропущенные поля, невалидный/короткий SHA, ветка, отсутствующий коммит и репозиторий, конфликт GUID, несовместимый module.json, операторский ключ, файл вне каталога, лишний файл и др.); конфликт GUID по регистру собирается в тесте (на macOS ФС регистронезависима). pytest: 102 passed.
- module-template/ (решение: каталог внутри module-applications, а не доработка mikopbx/ModuleTemplate — версионируется вместе с правилами и схемой, проверяется тестом) + инструкции RU/EN: сборка ZIP из SHA (tools/package, воспроизводимый ZIP) и локальный прогон тех же проверок (tools/validate.py, tools/check).
- Контрольный модуль ModuleAppCheck с заявкой и ожидаемыми результатами (tests/modules/expected.yaml): установка, обновление 1.0.0→1.1.0, воркер, API, отключение, удаление — на x86_64 (облако, GitHub runner) и arm64 (Mac, GitHub runner).
- Документация RU/EN: README, CONTRIBUTING (новая заявка, обновление, отзыв, что получает следующий этап, repo ≠ source_url, непубличные исходники не принимаются, старые модули не затрагиваются), TERMS и соглашение издателя — черновики «текст утверждает MIKO, отложено владельцем», PR template, workflows terms/cla/validate (triage в два job).

Чем проверено: pytest; tools/validate.py проходит на правильных и падает на каждой испорченной фикстуре с нужным правилом; пробные заявки по закреплённому SHA в песочнице (PR #1–#8, ссылки в LIC-612).
Осталось вне этой задачи: API издателя и уникальности (LIC-613), текст условий/соглашения, CODEOWNERS (ревьюеры MIKO), реальный репозиторий mikopbx/module-applications.
```

## Зачем
EXECUTOR_PROMPT.md: LIC-611 закрывается при выполнении всех критериев; серверные критерии — интерфейс с заглушкой, явно указано.

## Ожидаемый результат
LIC-611 в статусе «Готово», id комментария. Без подтверждения пользователя не выполнять.
