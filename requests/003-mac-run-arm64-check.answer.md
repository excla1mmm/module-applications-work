# 003: ответ — arm64 (Apple Silicon, Docker Desktop), коммит bda6b66

Выполнено 27.10.2026, 14:10–14:21 (+08). Полный лог и JSON — в `requests/003/`.

## pytest (быстрые)
`1 failed, 48 passed, 8 skipped in 2.89s`

**FAILED** `test_validate.py::test_bad_application_fails_with_the_expected_reason[guid-conflict-case]`.
Причина — файловая система macOS (APFS) **нечувствительна к регистру**: в git лежат `applications/ModuleAPPCheck` и
`applications/ModuleAppCheck`, на Mac после checkout остаётся один каталог (`ModuleAPPCheck/`), конфликта нет.
Это не только проблема теста: такой фикстурой репозиторий нельзя нормально склонировать на macOS/Windows (издатели на
Mac получат «грязное» дерево). Предлагаю создавать этот случай во время теста во временном каталоге (или проверять
конфликт регистра по `git ls-tree`/списку путей PR, а не по ФС) и убрать каталог-дубль из репозитория.

## Образ
`docker image inspect … --format '{{.Architecture}} {{.Os}}'` → **`arm64 linux`**, platform digest
`sha256:3af27a8ecc070108bdde4a2e3939407eeda0574896e55a547ee221973d0b6a6c`, MikoPBX 2026.6.34.

## pytest (Docker)
`RUN_DOCKER_TESTS=1 pytest -q tests/test_check.py` → **`3 passed in 330.01s (0:05:30)`**.

## tools/check
| Модуль | exit | Итог |
|---|---|---|
| ModuleAppCheck (`--previous-commit eb13e87…`) | **0** | boot 17 с; update 1.0.0→1.1.0, install, php_lint, enable, 1 воркер, module_api, disable, uninstall, logs, health — всё pass |
| ModuleAppProbe | **0** | boot 17 с; «the probe found no secret, no host file and no route out» |
| ModuleAppBroken | **1** (ожидалось 1) | boot 12 с; fail на `disable`: `Call to undefined function …module_app_broken_function_that_does_not_exist()` |

- На каждом прогоне ожидаемый `WARNING: could not add the iptables rule` (`host_firewalled: false`, Docker Desktop VM).
  При этом зонд на macOS **ничего не достиг** (секретов/файлов/монтирований нет, сеть недоступна).
- Скорость на arm64 сопоставима с x86_64 (старт 12–17 с), весь прогон ~11 мин.
