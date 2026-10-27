# 002: комментарий в LIC-611 — итоги этапа 0 (x86_64)
Тип: jira-write
Ветка/коммит: claude/stoic-goldberg-n01ons @ ecda607

## Что нужно
`jira.sh comment-add LIC-611 -` с текстом ниже (markdown):

```
Этап 0 (x86_64, Docker 29.9) — образ и цикл модуля проверены.

Образ: mikopbx/mikopbx:2026.6.34 (= latest), multi-arch index sha256:ff5cc0238a4e170bfbf8c0be9a59b58c8affb6fb24b36d1506f5fc0cb41f7691; amd64 sha256:50ed8982…f1d5d, arm64 sha256:3af27a8e…b6a6c. Зафиксирован в доверенном config/mikopbx.yaml, заявка его не задаёт.
Запуск: --privileged не нужен, тома не нужны, порты не публикуются. REST API отвечает через ~18 с. Все вызовы — docker exec … curl http://127.0.0.1/pbxcore/api/v3 (modules: LOCALHOST|BEARER_TOKEN, без пароля).
Сеть: с --network none MikoPBX не загружается (Asterisk без интерфейса, ~140 с). Выбрана docker-сеть --internal: старт 18 с, наружу/metadata пути нет. Найдено: --internal не закрывает адрес моста хоста — сервис хоста на 0.0.0.0 был доступен; закрыто правилом iptables INPUT -i br-<сеть> -j DROP на время прогона, проверено модулем-зондом.
Цикл (upload → getMetadataFromPackage → installFromPackage → enable → disable → uninstall) проходит; команды и вызовы — module-applications/docs/mikopbx-docker.md в ветке.
Отличия 2026.6.34 от Core develop: uninstall без keepSettings падает (Undefined array key), GET /modules (getList) — Unknown action, getInstallationStatus не работает с новым конвейером; операции отслеживаются через getOperations/getOperationStatus. Иногда «database is locked» сразу после старта — tools/check повторяет операцию (до 2 раз) и пишет это в отчёт.
tools/check на x86_64: контрольный ModuleAppCheck проходит полный цикл, включая обновление 1.0.0 → 1.1.0; намеренно сломанный ModuleAppBroken — красный (fatal в disable); ModuleAppProbe — секретов в env нет, файлов хоста нет, ни один сетевой адрес (metadata, docker host, интернет, мост) не доступен.
Осталось: прогон на arm64 (запрос на Mac) и на GitHub runner (нужна песочница).
```

## Зачем
Комментарий о находках этапа 0 (EXECUTOR_PROMPT.md, «Цикл в Jira»).

## Ожидаемый результат
Подтверждение и id комментария.
