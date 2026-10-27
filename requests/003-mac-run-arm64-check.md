# 003: прогон на arm64 — pytest, tools/check на трёх тестовых модулях, ручной цикл
Тип: mac-run
Ветка/коммит: claude/stoic-goldberg-n01ons @ bda6b66

## Что нужно
На Mac (Apple Silicon, Docker Desktop), в клоне этого репозитория:

```sh
git fetch origin claude/stoic-goldberg-n01ons && git checkout bda6b66
cd module-applications
python3 -m venv .venv && . .venv/bin/activate && pip install -r tools/requirements.txt pytest
python3 -m pytest -q tests                                   # быстрые тесты, ~3 с
docker pull mikopbx/mikopbx@sha256:ff5cc0238a4e170bfbf8c0be9a59b58c8affb6fb24b36d1506f5fc0cb41f7691
docker image inspect mikopbx/mikopbx@sha256:ff5cc0238a4e170bfbf8c0be9a59b58c8affb6fb24b36d1506f5fc0cb41f7691 --format '{{.Architecture}} {{.Os}}'
RUN_DOCKER_TESTS=1 python3 -m pytest -q tests/test_check.py  # три модуля в MikoPBX, ~6–10 мин
```

Затем полные JSON-результаты (зонд и контрольный модуль):

```sh
mkdir -p /tmp/repos && eval "$(python3 tests/module_repos.py /tmp/repos --env)"
printf 'workers: 1\napi: true\n' > /tmp/expect-check.yaml
python3 tools/check tests/fixtures/good/control/applications/ModuleAppCheck \
  --previous-commit eb13e87500abdf6852b3001ed63b2570ea6d0f89 --expect /tmp/expect-check.yaml --output /tmp/check-control.json
printf 'api: true\nprobe_empty: true\n' > /tmp/expect-probe.yaml
python3 tools/check tests/fixtures/good/probe-module/applications/ModuleAppProbe --expect /tmp/expect-probe.yaml --output /tmp/check-probe.json
python3 tools/check tests/fixtures/good/broken-module/applications/ModuleAppBroken --output /tmp/check-broken.json; echo "exit $?"
```

## Зачем
Критерий «tools/check проходит полный цикл в Docker на arm64»; здесь только x86_64. На macOS iptables-правила нет
(Docker Desktop — своя VM): tools/check напишет WARNING, это ожидаемо; зонд покажет, что реально достижимо.

## Ожидаемый результат
- вывод pytest (оба запуска);
- строка `docker image inspect` (ожидается `arm64 linux`);
- файлы `/tmp/check-control.json`, `/tmp/check-probe.json`, `/tmp/check-broken.json` → в `requests/003/`
  (там нет секретов: только шаги, digest, версии, имена переменных окружения контейнера и true/false сети);
- код выхода последней команды (ожидается 1) и время шагов boot/install, если что-то сильно медленнее x86_64 (~20 с старт).
