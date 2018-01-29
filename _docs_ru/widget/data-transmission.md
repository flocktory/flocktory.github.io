---
layout: doc
title:  "Передача данных"
section: widget
order: 4
---

## Retail Rocket

Бывает нужно из виджета передавать собранные данные в сторонние сервисы.


Ниже описано как передавать собранные данные в рекомендательный сервис Retail Rocket.

<br>

**Пример:** передача email в RR, после успешной передачи во Flocktory.

```javascript
  widget.collectEmail(email).then(function() {
    widget.setScreen('success');

    // передача в RR
    try {
        /* данные в произвольной форме, могу меняться в зависимости от клиента */
        var data = {partner: 'flocktory', stockId: 1};

        (parent.window["rrApiOnReady"] = parent.window["rrApiOnReady"] || []).push(function() {
          window.parent.rrApi.setEmail(email, data);
        });
      }
    catch (e) {}
  });
```

Обратите внимание, что метод **setEmail** принимает на вход два параметра: email и data.

```email (String)``` – email пользователя. Обязательный параметр.

```data (Object)``` – объект с дополнительными параметрами, **ключи могут отличаться в зависимости от клиента**. Необязательный параметр.
