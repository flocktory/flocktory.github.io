---
layout: doc
title:  "Общая интеграция"
section: integration
---

Прежде чем подключить к проекту любой из продуктов Flocktory, необходима общая интеграция.

Поместите этот код на все страницы вашего сайта. Лучше всего вставить его в тег `<head>` или в конце тега `<body>`

```html
<script type="text/javascript" src="https://api.flocktory.com/v2/loader.js?site_id=YOUR_SITE_ID" async="async"></script>
```

Этот код — общий для всех продуктов Flocktory, и вставлять его нужно только один раз, даже если на одной странице интегрировано несколько механик (например, exchange и precheckout).

Будьте внимательны: переменную YOUR_SITE_ID нужно заменить на настоящий id вашего сайта.

К примеру, для сайта dmitry-manannikov.com код будет выглядеть так:

```html
<script type="text/javascript" src="//api.flocktory.com/v2/loader.js?site_id=1781" async="async"></script>
```

Вы всегда можете уточнить id ваших сайтов в [личном кабинете](http://flocktory.com/) — или обратиться к аккаунт-менеджеру.

