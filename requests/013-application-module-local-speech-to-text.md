# 013: открыть PR с заявкой ModuleLocalSpeechToText в песочнице
Тип: other
Ветка/коммит: песочница excla1mmm/module-applications, main d28c980 (merge #12, M04: платные модули)

## Что нужно
Открыть в https://github.com/excla1mmm/module-applications PR от имени издателя (аккаунт @excla1mmm), который
добавляет ровно один файл `applications/ModuleLocalSpeechToText/application.yaml` с содержимым ниже. Ветку создать
от main (например `application/ModuleLocalSpeechToText`). Описание PR — по шаблону `.github/PULL_REQUEST_TEMPLATE.md`,
отметить все пункты чек-листа. Это заявка (меняет `applications/`), поэтому соглашение издателя нужно подписать
комментарием в PR: `I have read the MikoPBX Publisher Agreement and I hereby sign it` (подписывает только сам
@excla1mmm; пилотная подпись, текст соглашения — черновик). Локально ничего не запускать.

```yaml
schema_version: 1
module_id: ModuleLocalSpeechToText
publisher_id: 144251268
name: Local speech to text
description: Speech recognition for MikoPBX calls, running on the PBX itself.
repo: https://github.com/excla1mmm/ModuleLocalSpeechToText
commit: 8aa9fa1cea8367be31fd3b54470caf4de4fd1395
license: GPL-3.0-or-later
support_email: help@miko.ru
maintainers: [excla1mmm]
compatibility:
  min_pbx_version: "2025.1.1"
  max_pbx_version: null
  architectures: [x86_64, arm64]
```

(`name`/`description` можно уточнить. `support_email` равен адресу в module.json. `min_pbx_version` не ниже
module.json — 2025.1.1.)

## Что будет дальше (для сверки)
- Validate: правила 0.3.0 — проходит (проверено в облаке `tools/validate.py --online`); triage-строка «Licence:
  paid module: lic_product_id 306, lic_feature_id 73».
- Test (x86_64 и arm64): шаг `license` = `license_required` (Core без ключа: «The license key to activate the license
  for the module has not been installed»), затем цикл на тестовой копии с `lic_* = 0`. В облаке на x86_64 (Docker,
  MikoPBX 2026.6.34) так и вышло: `passes`, пакет sha256 `2806540d9801b547…`, копия `c7cc43e51112…`.
- AI review — после зелёных тестов, запуск подтверждает ревьюер Environment `ai-review` (лимит $0.75 на ревью).
- Merge заявки — решение MIKO (в песочнице — пользователь), не облака.

## Зачем
M04: после исправления MJ-005 — реальная заявка платного модуля через весь процесс.

## Ожидаемый результат
Номер PR с заявкой. Облако будет следить за проверками и разберёт результаты.
