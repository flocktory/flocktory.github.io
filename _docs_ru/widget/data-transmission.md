---
layout: doc
title:  "Передача данных"
section: widget
order: 5
---

## Во Flocktory

Есть виджеты типа сборщик имейлов, виджеты-опросы, где нужно данные, полученные от пользователя передавать во Flocktory.
Для этого есть метод [widget.collectEmail]({{ site.baseurl }}{% link _docs_ru/widget/widget-api.md %}). Ниже примеры, как его использовать

**Если нет email, name**

```javascript
/*
data – данные опроса, формате ключ - значение
Например: {'sex': 'female', 'age': '18+'}
*/
widget.collectEmail(null, null, data).then(function(){
  widget.setScreen('success');
});
```

**Если есть email, data, но нет name**

```javascript
widget.collectEmail(email, null, data).then(function(){
  widget.setScreen('success');
});
```

**Если только email**

```javascript
widget.collectEmail(email).then(function(){
  widget.setScreen('success');
});
```

**Если в data какое-то поле пустое, то не передаем его**


*Пример: пользователь указал возраст, а пол не указал*
```javascript
widget.collectEmail(email, name, {'age': 18}).then(function(){
  widget.setScreen('success');
});
```

*Пример: пользователь указал возраст и пол*
```javascript
widget.collectEmail(email, name, {'sex': 'female', 'age': 18}).then(function(){
  widget.setScreen('success');
});
```

**Бывает, что в виджете нет поля для ввода имейла, но нужно отправить имейл во flocktory, если клиент передал его в интеграции**

```javascript
var email = null;
try {
  email = parent.flocktory.getData().user.email;
}
catch(e){}

widget.collectEmail(email).then(function(){
  // Действие после удачно отправки
});
```

<br>

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

<br>

## DataLayer


**Пример:** Добавление события в dataLayer клиента, что был оставлен email

```javascript
  widget.collectEmail(email).then(function() {
    widget.setScreen('success');

    try{
      parent.dataLayer.push({
        event: 'fl_collected_email',
        email: email
      });
    }
    catch(e){}
  })
```
