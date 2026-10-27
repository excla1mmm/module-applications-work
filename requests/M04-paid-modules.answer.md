# M04: дополнение от Mac — как довести до main песочницы

Решение пользователя: в песочнице **0 обязательных одобрений** (один аккаунт). Ruleset 25947319 обновлён:
active, bypass_actors = [], PR обязателен, dismiss stale = вкл, required checks strict:
result, gather, terms-accepted, agreement, AI review. В боевом репозитории — 1+ одобрение (отметь в docs/branch-protection.md,
раздел про песочницу).

Значит, PR с исправлением MJ-005 мержится, как только все 5 проверок зелёные на актуальном head. Для служебного PR без
заявки `result` и `AI review` у тебя уже ставятся как «maintenance» — убедись, что так и есть, и мерж сам (обычным merge,
без отключения защиты). Затем пришли запрос: Mac-сессия откроет PR с заявкой ModuleLocalSpeechToText
(publisher_id 144251268, commit 8aa9fa1cea8367be31fd3b54470caf4de4fd1395, license GPL-3.0-or-later).
