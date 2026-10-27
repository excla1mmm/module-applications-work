# 001: start LIC-611 с планом
Тип: jira-write
Ветка/коммит: —

## Что нужно
`jira.sh start LIC-611 --comment -` с текстом ниже (markdown):

```
Старт (облачная сессия-исполнитель, x86_64). План:
1. Этап 0: образ mikopbx/mikopbx (тег, digest amd64/arm64), запуск в Docker, ручной цикл upload → install → enable → disable → uninstall через REST API v3 с 127.0.0.1; команды — в docs/stage0.md.
2. Формат заявки: schema/application.schema.json (schema_version 1), templates/application.yaml, rules/ (обязательный контракт MikoPBX отдельно от рекомендаций, у каждого требования id; спорные — черновик для LIC-319).
3. tools/validate.py по образцу oss-scanner: офлайн формат + онлайн сверка module.json на полном SHA; сверка контакта с карточкой издателя и уникальность module_id на сервере — интерфейс с заглушкой до LIC-613.
4. Фикстуры good/bad + pytest, контрольный модуль на базе ModuleTemplate с заявкой и ожидаемыми результатами.
5. Документация RU/EN (README, CONTRIBUTING, черновики TERMS и соглашения издателя — текст утверждает MIKO), PR template, workflows terms/cla/validate по паттернам oss-scanner.
Работа ведётся в рабочем репозитории, ветка claude/...; ссылки на коммиты — в следующих комментариях.
```

## Зачем
Перевести LIC-611 в работу с планом (EXECUTOR_PROMPT.md, «Цикл в Jira»).

## Ожидаемый результат
Подтверждение: статус LIC-611 «В работе», комментарий опубликован (ссылка или id комментария).
