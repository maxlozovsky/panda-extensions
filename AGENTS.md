## Общие инструкции

Перед началом работы прочитай `/home/max/.codex/AGENTS.md` и соблюдай его инструкции.

Инструкции этого проекта дополняют общие и имеют приоритет при конфликте.

## GitHub

GitHub integration для проекта включена.

- Repository: `maxlozovsky/panda-extensions`
- GitHub account: `maxlozovsky-bot`

При работе по issue сначала прочитай issue и его комментарии. Существенные результаты и решения фиксируй в issue; технический шум отдельно не комментируй.

## Проект и проверки

- `catalog.json` и его правила описаны в `README.md`; README является контрактом каталога для consumer-расширений.
- При содержательном изменении каталога обновляй `updatedAt` и выполняй checklist из раздела «Обновление каталога».
- Перед handoff как минимум проверь `jq empty catalog.json` и отсутствие битых локальных asset-paths.

## Git и публикация

- GitHub Pages публикует содержимое корня ветки `main`; push в `main` является production publication.
- Отдельного manual deploy/sync механизма у Pages нет: после push проверь статус Pages build и публичные изменённые URL.
