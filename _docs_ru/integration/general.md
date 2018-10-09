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


В Google Tag Manager 

```html
<script type="text/javascript"src="//api.flocktory.com/v2/loader.js?site_id=SITE_ID" async="async"></script>
```
![GTM int](https://assets.flocktory.com/uploads/clients/1562/0b558f18-e280-4c42-bce7-eec0b0de2446_gtm__general.png)



Будьте внимательны: переменную **YOUR_SITE_ID** нужно заменить на настоящий id вашего сайта.
