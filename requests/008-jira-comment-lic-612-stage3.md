# 008: комментарий в LIC-612 — AI-отчёт (этап 3), калибровка, лимиты, M01
Тип: jira-write
Ветка/коммит: claude/stoic-goldberg-n01ons @ 9a9c08a; песочница excla1mmm/module-applications @ 7b688f8

## Что нужно
`jira.sh comment-add LIC-612 -` с текстом ниже (markdown):

```
Этап 3 (AI-отчёт) сделан и проверен в песочнице.

Архитектура (docs/ai-review.md, модель угроз — в ai-review.yaml):
- ai-review.yaml запускается после Test (workflow_run, код только из main). prepare (без ключа, read-only токен) сверяет вход через GitHub API, а не по артефактам: это test.yaml этого репозитория; provenance (пишет job без кода модуля) называет PR, head и заявку; PR открыт и не сдвинулся; sha256 заявки совпадает; итоги job'ов совпадают с result.json; в результате тот же commit, текущий digest образа и sha256 ZIP, который prepare пересобирает сам. Иначе — отказ (покрыто тестами: поддельный sha256/коммит/исход/образ, push после теста, чужой workflow, лимит запусков).
- review — отдельная VM в Environment ai-review (ключ только там, ручное подтверждение каждого запуска): Messages API, claude-sonnet-5-5, effort high, structured output по схеме, три инструмента только на чтение (list/read/grep) в каталоге извлечённых исходников, без shell/сети/записи; ничего из модуля не загружается (не Agent SDK — он несёт Bash/правку/загрузку CLAUDE.md). Исходники и логи обрамлены как недоверенные данные. Каждую находку проверяет код MIKO (правило, файл, строка, цитата); блокер — только по обязательному правилу. Таймаут/бюджет/отказ/невалидный JSON → incomplete = не пройдено. Отчёт: schema/ai-report.schema.json v1 (module_id, publisher_id, repo, SHA, sha256 ZIP, правила, digest, модель/конфиг, блокеры/рекомендации, токены, оценка стоимости, шаги, время).
- comment публикует отчёт в PR и статус «AI review» на проверенном head. Новый push/заявка/правила/образ/конфиг → отчёт неактуален (tools/ai_review.py current), кэш — только при совпадении всех входов. Прослойка провайдера (tools/ai_providers.py) — для DeepSeek/Qwen без переделки workflow.

Калибровка (config/ai-calibration.yaml: ModuleAppBroken, ModuleAppInject, ModuleAppProbe, ModuleTemplate, ModuleCalleridSearchCH, ModuleHttpAlert):
- №1 (effort medium) https://github.com/excla1mmm/module-applications/actions/runs/40472280398 — поймано 3/4, пропуск: дефект ModuleAppBroken найден, но записан рекомендацией («тесты поймают»); $0.25.
- Исправлено: политика (обязательное правило = блокер независимо от тестов; маленькие модули читать целиком), effort high, CODE-002 уточнено (rules 0.2.1).
- №2 (effort high) https://github.com/excla1mmm/module-applications/actions/runs/40473524214 — 4/4, ложных блокеров 0, отброшенных находок 0; $0.75 на 6 модулей; максимум на модуль 7 ходов, $0.25, 114 с. Prompt injection (README, комментарий, syslog) — не повлиял, отмечен находками; собственный CLAUDE.md в ModuleHttpAlert прочитан как данные. Opus не нужен.
- Лимиты на ревью (config/ai-review.yaml): 25 ходов, $0.75, 600 с, 3 ревью на PR в сутки, 2 повтора SDK.
- На PR: #6 контрольный — complete, без блокеров, статус success, $0.05 (https://github.com/excla1mmm/module-applications/actions/runs/40475753763); #7 injection — 3 блокера (SEC-002, SEC-001, SEC-004), статус failure, «approved» нигде нет, $0.05 (https://github.com/excla1mmm/module-applications/actions/runs/40475810198); #8 сломанный — тесты красные, AI не запускался. Итого по оценке SDK ≈ $1.10 (сверка по Console — запрос 007).

M01: image-update открыл PR сам (флаг «Allow GitHub Actions to create PRs» включён): https://github.com/excla1mmm/module-applications/pull/9 — меняет только config/mikopbx.yaml, контрольные модули на x86_64+arm64 зелёные до открытия.

Найдено в песочнице: без защиты ветки PR со сломанным модулем и с правкой workflow (write-all) были смержены — проверки только окрашивают PR. Откачено (d463592). Подготовлены сводные обязательные проверки (result в Test, статус «AI review» и для служебных PR) и список настроек — docs/branch-protection.md. Осталось по LIC-612: включить защиту ветки (обязательные проверки, 1 approve со сбросом при push, без обхода) и проверить на ней сброс одобрения при push.
```

## Зачем
Комментарий о прогрессе (M02: «запрос jira-write с итогом калибровки в LIC-612»).

## Ожидаемый результат
Подтверждение и id комментария.
