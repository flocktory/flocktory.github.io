---
layout: doc
title:  "Общая интеграция"
section: integration
order: 1
---

Прежде чем подключить любой из модулей Flocktory, необходимо провести общую интеграцию.

Поместите этот код в тэг `<head>` на каждой странице вашего сайта:

```html
<script type="text/javascript" src="https://api.flocktory.com/v2/loader.js?site_id=YOUR_SITE_ID" async="async"></script>
```

Этот код — общий для всех продуктов Flocktory, и вставлять его нужно только один раз, даже если на одной странице интегрировано несколько модулей (например, exchange и precheckout).

Будьте внимательны: переменную **YOUR_SITE_ID** нужно заменить на настоящий id вашего сайта.


Вы всегда можете уточнить id ваших сайтов в [личном кабинете](https://cabinet.flocktory.com/) — или обратившись к вашему менеджеру.
