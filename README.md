# Panda Extensions Catalog

Статический каталог расширений для раздела «О расширении». Каталог публикуется через GitHub Pages:

`https://maxlozovsky.github.io/panda-extensions/catalog.json`

Структура рассчитана на повторное использование другими расширениями: данные валидируются на стороне расширения, а затем отображаются как обычный текст, ссылки и изображения без загрузки удалённого исполняемого кода.

## Файлы

```text
panda-extensions/
├── catalog.json
└── assets/
    ├── author.png
    ├── kpdb.png
    ├── panda-link-id-copier.png
    ├── panda-screenshoter.png
    ├── panda-speed-dial.png
    └── links/
        └── *.svg
```

Все URL изображений в каталоге должны начинаться с:

`https://maxlozovsky.github.io/panda-extensions/assets/`

Изображения с другого origin или из другой директории не проходят валидацию.

## Корневая структура `catalog.json`

```json
{
  "schemaVersion": 1,
  "updatedAt": "2026-09-17T16:17:08Z",
  "defaultLocale": "en",
  "author": {},
  "extensions": []
}
```

- `schemaVersion` – версия схемы. Текущее значение строго `1`.
- `updatedAt` – дата обновления в формате ISO 8601.
- `defaultLocale` – fallback-локаль. Текущее значение строго `en`.
- `author` – информация об авторе.
- `extensions` – список расширений, не более 100 записей.

## Автор

```json
{
  "id": "maxlozovsky",
  "avatar": "https://maxlozovsky.github.io/panda-extensions/assets/author.png",
  "links": [
    {
      "label": "GitHub",
      "url": "https://github.com/maxlozovsky",
      "icon": "https://maxlozovsky.github.io/panda-extensions/assets/links/github.svg"
    }
  ],
  "i18n": {
    "en": {
      "name": "Max Lozovsky",
      "description": "Browser extension author."
    },
    "ru": {
      "name": "Макс Лозовский",
      "description": "Автор браузерных расширений."
    },
    "zh_CN": {
      "name": "Max Lozovsky",
      "description": "浏览器扩展程序作者。"
    }
  }
}
```

- `id` – стабильный идентификатор автора: от 1 до 100 символов, допустимы буквы, цифры, `.`, `_` и `-`.
- `avatar` – абсолютный HTTPS URL изображения внутри каталога assets.
- `links` – массив ссылок автора, максимум 12 элементов. Поле обязательно, но массив может быть пустым.
- `i18n` – обязательные локализации `en`, `ru`, `zh_CN`.

## Расширения

```json
{
  "slug": "kpdb",
  "chromeWebStoreId": "mcjcbomlnkkkpieicohajbfepndbcbel",
  "icon": "https://maxlozovsky.github.io/panda-extensions/assets/kpdb.png",
  "storeUrl": "https://chromewebstore.google.com/detail/kpdb/mcjcbomlnkkkpieicohajbfepndbcbel",
  "enabled": true,
  "priority": 40,
  "links": [
    {
      "label": "KPDB",
      "url": "https://kpdb.stream/",
      "icon": "https://maxlozovsky.github.io/panda-extensions/assets/links/website.svg"
    }
  ],
  "i18n": {
    "en": {
      "name": "KPDB",
      "description": "Movie and TV ratings."
    },
    "ru": {
      "name": "KPDB",
      "description": "Рейтинги фильмов и сериалов."
    },
    "zh_CN": {
      "name": "KPDB",
      "description": "电影和电视剧评分。"
    }
  }
}
```

- `slug` – уникальный стабильный идентификатор в kebab-case.
- `chromeWebStoreId` – 32-символьный ID из букв `a`–`p` либо `null`, если расширение ещё не опубликовано.
- `icon` – абсолютный HTTPS URL иконки внутри каталога assets.
- `storeUrl` – ссылка вида `https://chromewebstore.google.com/detail/.../{id}` либо `null`, если расширение ещё не опубликовано.
- `enabled` – управляет показом карточки. `false` скрывает расширение, но сохраняет его запись в каталоге.
- `priority` – целое число. Чем оно меньше, тем выше карточка; при равенстве используется локализованное название.
- `links` – необязательный массив дополнительных ссылок, максимум 12 элементов. Отсутствующее поле и `[]` эквивалентны.
- `i18n` – обязательные локализации `en`, `ru`, `zh_CN`.

Текущее расширение исключает собственную карточку по `slug`, а при наличии runtime ID – также по `chromeWebStoreId`.

Поля магазина всегда заполняются парой:

```json
{
  "chromeWebStoreId": null,
  "storeUrl": null
}
```

После публикации оба значения обязательны, а ID в конце `storeUrl` должен совпадать с `chromeWebStoreId`. При `storeUrl: null` кнопка Chrome Web Store не показывается; карточка всё ещё может содержать дополнительные `links`. Нельзя заполнять только одно из двух полей или использовать пустую строку.

## Ссылки `links`

Одинаковый формат используется в `author.links` и `extensions[].links`:

```json
{
  "label": "YouTube",
  "url": "https://www.youtube.com/@example",
  "icon": "https://maxlozovsky.github.io/panda-extensions/assets/links/youtube.svg"
}
```

- `label` – обязательная непустая строка до 80 символов. Она используется как tooltip, доступное название и текстовый fallback.
- `url` – обязательный абсолютный HTTPS URL без credentials либо корректный `mailto:` URL, до 2048 символов.
- `icon` – необязательный URL изображения внутри каталога assets.

Если `icon` указан, ссылка отображается компактной кнопкой-иконкой. Если изображение не загрузилось, интерфейс заменяет её текстовой кнопкой с `label`. Без `icon` сразу отображается текстовая кнопка.

Пустые значения `url: ""` и `url: null` не допускаются: одна невалидная ссылка делает невалидным весь каталог. Пока адреса нет, элемент нужно убрать из массива. Пустой список задаётся как `"links": []`; у расширения поле также можно полностью опустить.

### Доступные иконки

Имена не являются программным enum: каталог передаёт полный URL файла. Таблица фиксирует доступные сейчас варианты и рекомендуемые подписи.

| Вариант | Рекомендуемый `label` | URL иконки |
| --- | --- | --- |
| Boosty | `Boosty` | `https://maxlozovsky.github.io/panda-extensions/assets/links/boosty.svg` |
| Email / контакты | `Email` | `https://maxlozovsky.github.io/panda-extensions/assets/links/email.svg` |
| Facebook | `Facebook` | `https://maxlozovsky.github.io/panda-extensions/assets/links/facebook.svg` |
| GitHub | `GitHub` | `https://maxlozovsky.github.io/panda-extensions/assets/links/github.svg` |
| Instagram | `Instagram` | `https://maxlozovsky.github.io/panda-extensions/assets/links/instagram.svg` |
| Ko-fi | `Ko-fi` | `https://maxlozovsky.github.io/panda-extensions/assets/links/kofi.svg` |
| VK | `VK` | `https://maxlozovsky.github.io/panda-extensions/assets/links/vk.svg` |
| Сайт | `Website` или название проекта | `https://maxlozovsky.github.io/panda-extensions/assets/links/website.svg` |
| YouTube | `YouTube` | `https://maxlozovsky.github.io/panda-extensions/assets/links/youtube.svg` |

Для `email.svg` можно использовать как HTTPS-страницу контактов, так и прямую почтовую ссылку:

```json
{
  "label": "Email",
  "url": "mailto:dev@example.com",
  "icon": "https://maxlozovsky.github.io/panda-extensions/assets/links/email.svg"
}
```

В `mailto:` обязателен корректный адрес получателя. Допускаются несколько адресов через запятую и query-параметры, например `subject`, если они не содержат переводы строк.

Чтобы добавить новый вариант:

1. Положить SVG в `assets/links/`.
2. Использовать его полный GitHub Pages URL в поле `icon`.
3. Добавить вариант в таблицу выше.

## Локализация

Для автора и каждого расширения обязательны все три ключа:

- `en`;
- `ru`;
- `zh_CN`.

Каждая запись содержит непустые `name` и `description`. Максимальная длина имени – 120 символов, описания – 1000 символов. Неизвестная локаль интерфейса получает английский вариант.

Поле `label` у ссылок не локализуется, поэтому для социальных сетей лучше использовать общеупотребимые названия: `GitHub`, `YouTube`, `Instagram`, `VK`. Для сайта продукта можно указать название самого продукта.

## Обновление каталога

При любом содержательном изменении нужно обновить `updatedAt`. Перед публикацией следует проверить:

1. JSON корректно разбирается.
2. Все обязательные локали заполнены.
3. Поля магазина одновременно заполнены или равны `null`; заполненный Store ID совпадает с последним сегментом `storeUrl`.
4. Все изображения существуют внутри `assets/` и совпадают с URL в каталоге.
5. В `links` нет пустых адресов; используются только HTTPS или корректные `mailto:` ссылки.
