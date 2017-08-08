---
layout: doc
title:  "Копирование"
section: utilities
---

### Описание

Часто бывает нужно в precheckout или postcheckout виджет добавить возможность копировать купон или ссылку вознаграждения. Для этого есть функция coreClipboard. Чтобы использовать ее, нужно подключить скрипт с функцией и добавить кнопке data-атрибуты.

Актуальную ссылку на скрипт всегда можно найти в дефолтных precheckout-виджетах. Например в виджете "Повышение среднего чека, положение снизу".

Подключение скрипта:

```html
<script type="text/javascript" src="https://assets.flocktory.com/u_widget/js/shared/coreClipboard-0d8a24717c.js"></script>
```

**ВАЖНО:** брать ссылку на скрипт именно из дефолтного виджета, потому что там содержится самая последняя версия


### Доступные data-атрибуты

 - `data-clipboard-text` - что нужно скопировать (например код купона или ссылка на вознаграждение)
 - `data-clipboard-copied` - текст, что скопировали успешно (показывается 3000 мс)
 - `data-clipboard-unsupported` - текст, если отсутствует поддержка браузера

 Пример html кнопки:

```html
 <button data-clipboard-text="{% raw %}{{ coupon.code }}{% endraw %}"
         data-clipboard-copied="скопировано!"
         data-clipboard-unsupported="скопируйте">скопировать</button>
```

### Поддержка браузеров
Chrome 42+, Firefox 41+, Safari 10+ на macOS и iOS, Opera 29+, MSIE 9+, Edge 12+
