---
layout: doc
title:  "Частые вопросы"
section: push
---

### Before any problem

Check in cabinet settings→tracking→subdomain.
Check cabinet settings→SSL 
For chrome, firefox check manifest added to site code head block:

```html
<link rel="manifest" href="/manifest.json">
```

### Does browser X support push?

Check [Browsers support]({{ site.baseurl }}{% link _docs_ru/push/support.md %}). If browser not in list it not supported.

### Why I don't see push request on page?

Check [Browsers support]({{ site.baseurl }}{% link _docs_ru/push/support.md %}).
Clean cookies and cache.
Remove from notification. Safari settings→notifications. Chrome chrome://settings/contentExceptions#notifications. Firefox TBD.

### I open page in incognito mode. I don't see push request. Why?

Push messages don't work in incognito mode.

### Which events widget sends?

Check [Push stats glossary]({{ site.baseurl }}{% link _docs_ru/push/stats.md %}).

### Push opt-in works on my computer, but client see empty/wrong page. What happens?

This happens when client have outdated DNS cache on his computer or network hardware.
To clear cache on windows computer follow [this guide](https://technet.microsoft.com/en-us/library/64b84fc3-a7a1-44b4-b26b-596a643d066e).

### В хроме на http сайте открывается новое окно, но нет запроса разрешение на пуши и окно не закрывается. Что делать?
Иногда сторонние расширения и настройки браузера могут приводить к такому поведению. Нужно создать новый профиль в браузере и подписаться там.
