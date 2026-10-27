# 004: ответ — песочница создана (решение пользователя 27.10.2026)

## Репозитории (оба публичные, временные — удаляются после задачи)
1. **`excla1mmm/module-applications`** — песочница механизма заявок (вместо предложенного `-sandbox`; репозиторий
   уже существовал пустым). Ветка по умолчанию `main`, сейчас пусто — залей туда каталог `module-applications/`
   из своей ветки в корень (через PR или напрямую — на твоё усмотрение), `OWNER/REPOSITORY` в PR template →
   `excla1mmm/module-applications`.
2. **`excla1mmm/module-applications-test-modules`** — тестовые модули, ветка на модуль, SHA совпадают с фикстурами:
   | ветка | HEAD |
   |---|---|
   | ModuleAppCheck | 1a4dfa6326c83d1ebcb40838ed7c0aebc24bbcda (история содержит предыдущий eb13e87500ab…) |
   | ModuleAppBroken | ab4075616a69e18b59dbd205facfb98280ee62dc |
   | ModuleAppProbe | 471c34cdf88c10d695286d83a62cc1aeb4f074a6 |
   | ModuleAppBadManifest | 25b1d41c116e2d86b5d05b04a7a70dcbaebcc5b0 |
   Собрано `tests/module_repos.py` из `origin/claude/stoic-goldberg-n01ons` @ 4717585.

## Настройки (проверено через API)
- Actions включены в обоих; Workflow permissions: **read**; «Allow GitHub Actions to create and approve PRs»: **выкл**.
- Repository secrets: **нет**.
- Environment **`ai-review`** в `excla1mmm/module-applications`:
  - секрет **`ANTHROPIC_API_KEY`** (только здесь, больше нигде);
  - **Required reviewers: excla1mmm** — каждый job с `environment: ai-review` ждёт ручного подтверждения;
  - Deployment branches: только `main`.
  Ключ — отдельный workspace Anthropic с лимитом трат. Значение ключа никто, кроме пользователя, не видел.
- Защита веток не включена.

## Доступ облачной сессии
Пользователь добавил репозитории в установку Claude GitHub App (шаг 4). Подключи их в сессии; если доступа нет —
скажи пользователю.

Если workflow, которому для `GITHUB_TOKEN` нужно больше `read` (triage-комментарий, image-update PR), — права
задаются в самом workflow (`permissions:`), настройки репозитория не трогаем.
