# 016: новая облачная сессия для импорта в mikopbx/module-applications (M05)
Тип: other
Ветка/коммит: excla1mmm/module-applications-work, claude/stoic-goldberg-n01ons @ 36d79b9

## Что нужно
Пользователю: запустить **новую** облачную сессию Claude Code (claude.ai/code) с двумя источниками:
`mikopbx/module-applications` и `excla1mmm/module-applications-work`. Песочницу `excla1mmm/module-applications`
в эту сессию **не** добавлять: у неё то же имя, и тогда целевой репозиторий не подключится (так случилось в текущей
сессии). Если Claude GitHub App ещё не установлен на mikopbx/module-applications — админ организации добавляет его
(docs/setup.md, п. 1.3). Первым сообщением дать текст ниже.

```
Ты продолжаешь M05 (LIC-661): перенос module-applications в mikopbx/module-applications. Весь контекст — в
excla1mmm/module-applications-work: прочитай CLOUD.md, TASK.md, requests/M05-lic-661-production-repo.md, ответы
в requests/*.answer.md (012–015) и module-applications/docs/setup.md. Источник импорта — ветка
claude/stoic-goldberg-n01ons, каталог module-applications/ (коммит 36d79b9 или новее на той же ветке).

Правила: публичный репозиторий MIKO — никаких секретов и внутренних данных; защиту ветки не обходить и не отключать;
автора коммитов не переписывать; Jira — только запросами requests/NNN в main module-applications-work (следующий
свободный номер); pytest, tools/validate.py, tools/check — только в CI целевого репозитория.

Порядок:
1. Проверь, что ветка по умолчанию целевого — main (если master — попроси пользователя переименовать до ruleset).
   Если репозиторий пуст: первый коммит (README.md-заглушка и .gitignore) — прямым push в main.
2. Ветка import/lic-661 от main: содержимое module-applications/ из источника без истории песочницы (чистый
   импорт, кроме __pycache__/.pytest_cache); PR в main. В описании PR — исходный SHA (ветка work) и что импортировано.
3. Ветки, которые нужны workflows: publisher-agreement-signatures (от main) и test-modules/<Name>
   (python3 tests/module_repos.py DIR --push https://github.com/mikopbx/module-applications).
4. Дай пользователю список настроек из docs/setup.md (Actions, Environments ai-review и image-update, переменные,
   GitHub App, ключ ANTHROPIC_API_KEY — значение кладёт он сам). Жди подтверждения.
5. Merge импорта — пользователь/ревьюер (до ruleset). Затем пользователь импортирует docs/ruleset-main.json; CODEOWNERS
   с логинами — отдельным PR, когда пользователь пришлёт логины (код-оунеры и last push approval требуют ≥ 2 ревьюеров).
6. Отрицательные проверки на целевом: прямой push отклонён; красный PR и PR без одобрения не мержатся; сброс одобрения
   после push (второй ревьюер). Пробная заявка по полному SHA проходит на x86_64 и arm64, испорченная — отклонена.
   Ссылки на PR и Actions.
7. Запросы jira-write в LIC-661: итог (исходный и итоговый SHA, PR), ответ на вопрос владельца 2 (что проверено в
   песочнице, что применено и проверено на целевом, что делает владелец).
```

## Зачем
Текущая сессия не может подключить mikopbx/module-applications (конфликт имён с песочницей). Импорт подготовлен
(36d79b9), остальное делается в целевом репозитории.

## Ожидаемый результат
Пользователь запускает сессию; дальше она работает с requests/ сама.
