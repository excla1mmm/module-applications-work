# Заявки модулей MikoPBX

[English version](README.md)

Чтобы разместить сторонний модуль в маркетплейсе MikoPBX, издатель открывает здесь pull request, который добавляет один
файл: `applications/<module_id>/application.yaml`. Исходники модуля остаются в собственном GitHub-репозитории издателя.
Заявка указывает этот репозиторий и **один полный SHA коммита**. Роботы проверяют заявку и тестируют ровно этот коммит
в свежих контейнерах MikoPBX на x86_64 и arm64. Затем MIKO проводит ревью и одобряет этот коммит. После merge модуль
регистрируется за подтверждённым издателем, а заявка остаётся здесь как запись о допуске модуля.

**Статус: пилот.** Условия размещения и соглашение издателя — черновики, пока MIKO не утвердит текст. Регистрация после
merge ещё не сделана.

```
ваш репозиторий ──► pull request ──► проверки ──► исправления ──► MIKO одобряет коммит ──► merge ──► регистрация ──► версии в кабинете
                    (одна заявка)     │
                                      ├─ Terms: чекбокс в описании
                                      ├─ Publisher agreement: подпись комментарием
                                      ├─ Validate: правила + triage-комментарий для ревьюера
                                      ├─ Test: установка / включение / отключение / удаление в MikoPBX, x86_64 и arm64
                                      └─ AI-ревью: блокеры с правилом и доказательством отдельно от советов (docs/ai-review.md)
```

## Как подать модуль

Подробно — в [CONTRIBUTING.ru.md](CONTRIBUTING.ru.md): новая заявка, обновление и отзыв. Коротко:

1. Модуль лежит в публичном репозитории на GitHub и устроен как [module-template/](module-template/README.ru.md)
   (обязательный контракт MikoPBX описан там и в [rules/rules.yaml](rules/rules.yaml)).
2. Скопируйте [templates/application.yaml](templates/application.yaml) в `applications/<module_id>/application.yaml`,
   заполните и проверьте локально: `tools/validate.py --online <module_id>` и `tools/check <module_id>`.
3. Откройте pull request, который меняет только этот файл, отметьте пункты чек-листа и подпишите соглашение издателя.

Одна заявка — один каталог — один идентификатор модуля (GUID). Для каждого нового модуля нужна своя заявка.

## Что в репозитории

| Путь | Что |
|---|---|
| `applications/<module_id>/application.yaml` | Допущенные модули (реестр), а в pull request — заявки |
| `templates/application.yaml` | Шаблон заявки |
| `schema/application.schema.json` | Формат заявки с версией |
| `rules/rules.yaml` | Требования с идентификаторами: обязательный контракт MikoPBX отдельно от рекомендаций |
| `module-template/` | Минимальный модуль, который проходит проверки, и как собрать его ZIP и запустить проверки |
| `tools/validate.py` | Правила: офлайн (формат) и онлайн (репозиторий модуля на коммите) |
| `tools/check` | Жизненный цикл модуля в свежем контейнере MikoPBX; `tools/package` только собирает ZIP |
| `config/mikopbx.yaml` | Доверенная тестовая конфигурация MIKO: стабильный образ MikoPBX по digest, лимиты, таймауты. `image-update.yaml` предлагает новый digest через PR после прогона контрольных модулей; новый образ делает прежние результаты неактуальными |
| `.github/workflows/` | `terms`, `cla` (соглашение издателя), `validate` (правила + triage), `test` (матрица MikoPBX), `ai-review` (этап 3), `image-update` |
| `docs/` | Как устроены проверки: [MikoPBX в Docker](docs/mikopbx-docker.md), [AI-ревью](docs/ai-review.md), [безопасность проверок](docs/security.md) |
| `TERMS.md`, `PUBLISHER_AGREEMENT.md` | Черновики: текст утверждает MIKO |

Локально и в CI работает один и тот же код. Workflows, инструменты, правила и конфигурация всегда берутся из базовой
ветки: pull request не может поменять то, по чему его проверяют. PR, который трогает что-то вне своего
`applications/<module_id>/`, отклоняется раньше, чем что-либо запустится.

## Лицензия

Инструменты — Apache-2.0. Часть кода адаптирована из [anthropics/oss-scanner](https://github.com/anthropics/oss-scanner)
(Apache-2.0), это указано в каждом таком файле. `module-template/` основан на
[mikopbx/ModuleTemplate](https://github.com/mikopbx/ModuleTemplate) (GPL-3.0-or-later).
