# 006: комментарий в LIC-612 — изолированные тесты (этап 2), прогоны в песочнице
Тип: jira-write
Ветка/коммит: claude/stoic-goldberg-n01ons @ 98afdad; песочница excla1mmm/module-applications @ 2878c28

## Что нужно
`jira.sh comment-add LIC-612 -` с текстом ниже (markdown):

```
Прогресс по тестовой части (без AI) — этап 2 закрыт, проверено в публичной песочнице excla1mmm/module-applications (временная).

tools/check <module_id> — один код локально и в CI: validate --online → git fetch точного SHA (без checkout) → воспроизводимый ZIP + sha256 → свежий контейнер MikoPBX (образ и digest из доверенного config/mikopbx.yaml) → установка через REST API → php -l → включение, воркеры, API модуля → отключение → удаление → логи → жив ли MikoPBX → JSON (module_id, SHA, sha256, правила, версия MikoPBX, архитектура, digest, шаги, итог). Обновление: ставится предыдущий одобренный SHA и обновляется до нового; для первой версии — «not_applicable: first version».
Изоляция: контейнер без томов/сокета/env раннера, docker-сеть --internal, iptables DROP с моста на хост (без него сервис хоста на 0.0.0.0 был доступен — найдено и закрыто), лимиты cpu/mem/pids.
test.yaml: pull_request_target, base checkout, из PR берётся только applications/<id>/application.yaml из git objects; матрица ubuntu-latest + ubuntu-24.04-arm; permissions contents:read, без секретов/кэшей; логи в ::stop-commands::; результат — артефакт на 7 дней. Модель угроз — в комментарии workflow и docs/security.md.

Прогоны:
- PR #1 контрольный ModuleAppCheck — Validate и Test зелёные на x86_64 и arm64, triage-комментарий опубликован: https://github.com/excla1mmm/module-applications/actions/runs/40470488921 , https://github.com/excla1mmm/module-applications/pull/1
- PR #2 сломанный ModuleAppBroken — Test красный на обеих архитектурах (fail на disable: undefined function): https://github.com/excla1mmm/module-applications/actions/runs/40470491512
- PR #3 зонд ModuleAppProbe — секретов в env нет, файлов/монтирований раннера нет, metadata/docker host/интернет/частные сети/мост хоста недоступны, host_firewalled=true (arm64 и x86_64): https://github.com/excla1mmm/module-applications/actions/runs/40470500044
- PR #4 заявка + правка .github/workflows/test.yaml (write-all, echo pwned) — отвергнут на приёме файлов (APP-006) до исполнения чего-либо, подменённый workflow не использован: https://github.com/excla1mmm/module-applications/actions/runs/40470507640
- Локально (x86_64 облако и arm64 Mac, запрос 003): те же три модуля — ожидаемые итоги.

M01 (автообновление образа): image-update.yaml раз в сутки смотрит mikopbx/mikopbx:latest через registry API (без pull), при новом digest прогоняет контрольные модули на x86_64+arm64 и открывает PR только с config/mikopbx.yaml. Прогон контрольных модулей на новом образе проверен: https://github.com/excla1mmm/module-applications/actions/runs/40472330102 (обе архитектуры зелёные). Смена образа делает прежние результаты и одобрения неактуальными (документировано). Открытие PR — проверяется повторным прогоном.
Осталось по LIC-612: AI-отчёт (этап 3) — калибровка идёт; одобрение конкретного SHA и защита ветки.
```

## Зачем
Комментарий о прогрессе в LIC-612 со ссылками на прогоны (EXECUTOR_PROMPT.md: «Ссылки на все run приложены в Jira»).

## Ожидаемый результат
Подтверждение и id комментария.
