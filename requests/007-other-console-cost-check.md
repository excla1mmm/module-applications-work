# 007: сверить фактическую стоимость AI-прогонов по Anthropic Console
Тип: other
Ветка/коммит: claude/stoic-goldberg-n01ons @ 9a9c08a

## Что нужно
В Anthropic Console (workspace песочницы, ключ из Environment `ai-review` excla1mmm/module-applications) открыть Usage/Cost
за 2026-10-27 и сверить с оценкой SDK. Все AI-запросы этого дня — только эти четыре job'а (UTC):

| Run | Что | Начало – конец (UTC) | Оценка по токенам |
|---|---|---|---|
| https://github.com/excla1mmm/module-applications/actions/runs/40472280398 | калибровка №1 (6 модулей, effort medium) | 08:22:33 – 08:24:21 | $0.25 |
| https://github.com/excla1mmm/module-applications/actions/runs/40473524214 | калибровка №2 (6 модулей, effort high) | 08:34:21 – 08:38:46 | $0.75 |
| https://github.com/excla1mmm/module-applications/actions/runs/40475753763 | AI-ревью PR #6 | 08:49:23 – 08:49:58 | $0.05 |
| https://github.com/excla1mmm/module-applications/actions/runs/40475810198 | AI-ревью PR #7 | 08:49:41 – 08:50:16 | $0.05 |
| | **итого** | | **≈ $1.10** |

Модель `claude-sonnet-5-5` (цены в config/ai-review.yaml: $2/$10 за 1M, cache read $0.20, cache write $2.50).

## Зачем
M02: «Фактическую стоимость по Anthropic Console проверит пользователь»; оценка SDK не биллинг.

## Ожидаемый результат
Фактическая сумма за день (и по запросам/времени, если Console показывает), остаток кредита, расхождение с ≈ $1.10.
Без ключей и скриншотов с ключами.
