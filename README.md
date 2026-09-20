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
  "extensions": [],
  "products": [],
  "productSections": []
}
```

- `schemaVersion` – версия схемы. Текущее значение строго `1`.
- `updatedAt` – дата обновления в формате ISO 8601.
- `defaultLocale` – fallback-локаль. Текущее значение строго `en`.
- `author` – информация об авторе.
- `extensions` – список расширений, не более 100 записей.
- `products` – необязательный список остальных продуктов: сайтов, сервисов, приложений, ботов, плагинов и инструментов.
- `productSections` – необязательное декларативное описание секций, в которых показываются `products`.

`products` и `productSections` являются обратно совместимым расширением схемы версии `1`: старые consumers могут их игнорировать. Само по себе добавление новых необязательных полей не требует увеличения `schemaVersion`. Версию схемы следует повышать только при несовместимом изменении существующего контракта: например, если обязательное поле меняет смысл, формат или перестаёт поддерживаться старым consumer.

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

## Продукты

`products` содержит проекты, которые не являются браузерными расширениями. Расширения остаются в отдельном `extensions`, потому что у них есть Chrome-specific поля и правила валидации.

```json
{
  "slug": "flight-alerts",
  "type": "service",
  "audiences": ["consumer"],
  "url": "https://example.com/",
  "icon": "https://maxlozovsky.github.io/panda-extensions/assets/flight-alerts.png",
  "enabled": true,
  "priority": 10,
  "links": [],
  "i18n": {
    "en": {
      "name": "Flight Alerts",
      "description": "Notifications about selected flight prices."
    },
    "ru": {
      "name": "Уведомления об авиабилетах",
      "description": "Уведомления о ценах на выбранные авиабилеты."
    },
    "zh_CN": {
      "name": "Flight Alerts",
      "description": "所选机票价格通知。"
    }
  }
}
```

- `slug` – уникальный стабильный идентификатор продукта в kebab-case.
- `type` – тип продукта. Это классификационный тег, а не жёсткий enum схемы. Рекомендуемые значения: `website`, `service`, `app`, `bot`, `plugin`, `tool`. Новые значения можно добавлять без изменения `schemaVersion`.
- `audiences` – непустой массив целевых аудиторий. Это также теги, а не жёсткий enum. Базовые значения: `consumer` и `developer`; один продукт может относиться сразу к нескольким аудиториям.
- `url` – основной абсолютный HTTPS URL продукта.
- `icon` – абсолютный HTTPS URL изображения внутри каталога assets.
- `enabled` – управляет участием продукта в витрине. `false` исключает продукт из всех секций, включая `includeSlugs`.
- `priority` – целое число. Чем оно меньше, тем раньше продукт располагается внутри секции; при равенстве используется локализованное название.
- `links` – необязательный массив дополнительных ссылок в том же формате, что у автора и расширений.
- `i18n` – обязательные локализации `en`, `ru`, `zh_CN`.

`type` отвечает на вопрос «что это?», а `audiences` – «для кого это?». Эти признаки намеренно независимы. Например, WordPress-плагин может иметь `type: "plugin"` и `audiences: ["developer"]`, а публичный сайт – `type: "website"` и `audiences: ["consumer"]`.

## Секции продуктов

`productSections` определяет не данные продуктов, а способ формирования витрины. Consumer не должен иметь захардкоженные секции `consumer` или `developer`: он читает массив секций из JSON, сортирует его по `priority`, применяет фильтры и рисует локализованный заголовок. Благодаря этому названия, состав и порядок секций можно менять без релиза расширения.

```json
{
  "id": "developer",
  "enabled": true,
  "priority": 20,
  "filters": {
    "audiences": ["developer"],
    "types": ["plugin", "tool"],
    "includeSlugs": [],
    "excludeSlugs": []
  },
  "limit": 4,
  "overflow": "expand",
  "i18n": {
    "en": {
      "title": "For developers"
    },
    "ru": {
      "title": "Для разработчиков"
    },
    "zh_CN": {
      "title": "面向开发者"
    }
  }
}
```

- `id` – уникальный стабильный идентификатор секции.
- `enabled` – позволяет полностью отключить секцию без удаления её настроек.
- `priority` – порядок секций: меньшее значение показывается выше.
- `filters` – набор правил выборки продуктов. Сам объект и отдельные фильтры могут быть пустыми или отсутствовать.
- `filters.audiences` – подходит продукт, у которого есть хотя бы один из перечисленных audience-тегов.
- `filters.types` – подходит продукт, чей `type` входит в список.
- `filters.includeSlugs` – принудительно добавляет перечисленные `enabled`-продукты, даже если они не прошли обычные `audiences`/`types` фильтры.
- `filters.excludeSlugs` – исключает перечисленные продукты из итоговой секции и имеет приоритет над `includeSlugs`.
- `limit` – положительное целое число, задающее количество карточек, видимых до применения overflow-поведения. Поле необязательно.
- `overflow` – поведение для элементов сверх `limit`: `expand`, `hide` или `all`. Если `limit` отсутствует, все продукты показываются независимо от `overflow`.
- `i18n` – обязательные локализации заголовка секции `en`, `ru`, `zh_CN`.

### Семантика фильтрации

Между разными группами фильтров действует `AND`, внутри одного массива – `OR`.

Например:

```json
{
  "audiences": ["developer"],
  "types": ["plugin", "tool"]
}
```

означает: продукт должен иметь аудиторию `developer` **и** иметь тип `plugin` **или** `tool`.

Рекомендуемый порядок формирования секции:

1. Взять только `products` с `enabled: true`.
2. Применить обычные `audiences` и `types` фильтры. Отсутствующая группа фильтра не ограничивает выборку.
3. Добавить продукты из `includeSlugs`, даже если они не прошли шаг 2.
4. Удалить продукты из `excludeSlugs`. Исключение всегда сильнее включения.
5. Удалить дубликаты по `slug`.
6. Отсортировать по `priority`, затем по локализованному `name`.
7. Применить `limit` и `overflow`.

Если после фильтрации секция пуста, consumer должен скрыть её целиком, а не показывать пустой заголовок.

### Поведение `overflow`

- `expand` – показать первые `limit` карточек и кнопку вида «Показать ещё N». По нажатию раскрыть остальные локально, без дополнительного сетевого запроса. Это рекомендуемое значение по умолчанию.
- `hide` – показать только первые `limit` карточек; остальные намеренно не выводить.
- `all` – показать все подходящие карточки; `limit` фактически игнорируется.

Если consumer поддерживает `productSections`, но значение `overflow` отсутствует, следует считать его равным `expand`.

Пример двух независимых витрин:

```json
[
  {
    "id": "consumer",
    "enabled": true,
    "priority": 10,
    "filters": {
      "audiences": ["consumer"],
      "types": ["website", "service", "app", "bot"]
    },
    "limit": 6,
    "overflow": "expand",
    "i18n": {
      "en": { "title": "Other products" },
      "ru": { "title": "Другие продукты" },
      "zh_CN": { "title": "其他产品" }
    }
  },
  {
    "id": "developer",
    "enabled": true,
    "priority": 20,
    "filters": {
      "audiences": ["developer"],
      "types": ["plugin", "tool"]
    },
    "limit": 4,
    "overflow": "expand",
    "i18n": {
      "en": { "title": "For developers" },
      "ru": { "title": "Для разработчиков" },
      "zh_CN": { "title": "面向开发者" }
    }
  }
]
```

## Ссылки `links`

Одинаковый формат используется в `author.links`, `extensions[].links` и `products[].links`:

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

Для автора, каждого расширения, каждого продукта и каждой продуктовой секции обязательны все три ключа:

- `en`;
- `ru`;
- `zh_CN`.

У автора, расширения и продукта каждая локаль содержит непустые `name` и `description`. Максимальная длина имени – 120 символов, описания – 1000 символов. У `productSections[].i18n` каждая локаль содержит непустой `title`. Неизвестная локаль интерфейса получает английский вариант.

Поле `label` у ссылок не локализуется, поэтому для социальных сетей лучше использовать общеупотребимые названия: `GitHub`, `YouTube`, `Instagram`, `VK`. Для сайта продукта можно указать название самого продукта.

## Обновление каталога

При любом содержательном изменении нужно обновить `updatedAt`. Перед публикацией следует проверить:

1. JSON корректно разбирается.
2. Все обязательные локали заполнены.
3. Поля магазина одновременно заполнены или равны `null`; заполненный Store ID совпадает с последним сегментом `storeUrl`.
4. Все изображения существуют внутри `assets/` и совпадают с URL в каталоге.
5. В `links` нет пустых адресов; используются только HTTPS или корректные `mailto:` ссылки.
6. У каждого продукта уникальный `slug`, корректные `type` и `audiences`, а `enabled: false` действительно должен скрывать его из всех секций.
7. У каждой продуктовой секции уникальный `id`; `includeSlugs` и `excludeSlugs` ссылаются только на существующие продукты.
8. `limit`, если задан, является положительным целым числом; `overflow` равен `expand`, `hide` или `all`.
9. Все секции проверены после фильтрации: нет случайных дублей, а пустые секции не должны отображаться consumer-ом.
