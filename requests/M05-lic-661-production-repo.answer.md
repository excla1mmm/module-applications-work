# M05: ведёт Mac-сессия (27.10.2026)

Перенос делает сессия на Mac (у неё права admin на mikopbx/module-applications), новая облачная сессия не нужна.
- Импорт: https://github.com/mikopbx/module-applications/pull/1 (источник 36d79b9) — смержен пользователем.
- Ветки: publisher-agreement-signatures, test-modules/{ModuleAppBadManifest,Broken,Check,Inject,Paid,Probe} (SHA = фикстуры).
- Actions: workflow permissions read, create PRs — вкл (пока нет App).
- Environments: ai-review (обязательный ревьюер excla1mmm, только main; prevent self-review — выкл, пока ревьюер один),
  image-update (только main). Секреты кладёт пользователь.
- Дальше: ключ ANTHROPIC_API_KEY, логины ревьюеров → CODEOWNERS PR → ruleset → отрицательные проверки → пробные заявки.
Облаку по M05 ничего делать не нужно.
