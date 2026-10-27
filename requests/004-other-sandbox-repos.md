# 004: песочница на GitHub для этапа 2 (нужно решение пользователя)
Тип: other
Ветка/коммит: claude/stoic-goldberg-n01ons @ bda6b66 (каталог module-applications/)

## Что нужно
Решение пользователя и, если «да», создание двух **публичных** репозиториев (arm64 runners бесплатны только в
публичных). Предлагаемые имена (можно другие — скажите какие):

1. `excla1mmm/module-applications-sandbox` — копия каталога `module-applications/` из ветки в корень репозитория,
   ветка `main`. В `.github/PULL_REQUEST_TEMPLATE.md` заменить `OWNER/REPOSITORY` на имя этого репозитория.
2. `excla1mmm/module-applications-test-modules` — тестовые модули: по одной ветке на модуль с теми же коммитами,
   чтобы SHA совпали с фикстурами:
   ```sh
   cd module-applications && python3 tests/module_repos.py /tmp/repos
   for m in ModuleAppCheck ModuleAppBroken ModuleAppProbe; do
     git -C /tmp/repos/$m.git push https://github.com/excla1mmm/module-applications-test-modules main:$m
   done
   ```

Настройки обоих: Actions включены; Settings → Actions → General → «Fork pull request workflows from outside
collaborators: Require approval for first-time contributors» (по умолчанию), Workflow permissions: **Read repository
contents** (read-only по умолчанию). Никаких secrets, защиту веток пока не включать.

**Доступ для облачной сессии:** чтобы я сам пушил ветки и открывал тестовые PR и чинил CI, добавьте оба репозитория в
установку Claude GitHub App (как сделали для module-applications-work) и скажите мне — я подключу их в сессии. Если не
хотите давать доступ, PR откроет Mac-сессия по моему следующему запросу.

## Зачем
Этап 2: PR контрольного модуля — всё зелёное на x86_64 и arm64 + triage-комментарий; PR со сломанным модулем —
красный; PR, который трогает workflow, — отвергнут; PR зонда — утечек нет. Ссылки на run — в Jira.

## Ожидаемый результат
Решение («да» с именами / «нет» / другое место), URL созданных репозиториев, подтверждение доступа для облачной сессии.
