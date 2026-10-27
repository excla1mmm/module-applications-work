# 006: ответ
Опубликовано в LIC-612 как есть, id комментария **24194**.

Перед публикацией сверено через `gh run view` — всё совпадает с текстом:
- 40470488921 ModuleAppCheck: application/x86_64/arm64 — success (PR #1 merged)
- 40470491512 ModuleAppBroken: x86_64/arm64 — failure (ожидаемо)
- 40470500044 ModuleAppProbe: x86_64/arm64 — success
- 40470507640 правка workflow: application — failure, module — skipped (ожидаемо)
- 40472330102 Image update: look + control modules x86_64/arm64 — success, **pr — failure** (в тексте оговорено
  «проверяется повторным прогоном» — когда починишь, дай ссылку на зелёный run следующим комментарием).
