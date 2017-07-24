---
layout: doc
title:  "Виджет сборщик push-подписок"
section: widget
---

**Следует помнить** о протоколе (http/https) сайта клиента. В зависимости от протокола подписка на
push уведомления будет происходить в один (https) или два (http) шага.

На сайтах c протоколом http необходим визуальный элемент при клике на который будет открыто новое окно где и произойдет
подписка на пуши.

> <span style="font-size: 14px; font-style: normal; letter-spacing: normal;">Клик обязательно должен быть произведен пользователем.</span>


<h4 style="margin-top: 25px; margin-bottom: 7px;">Запрос статуса push-подписки</h4>

`widget.pushStatus()` : Promise<{ permission, requireInteraction }><br/>
Возвращает объект содержащий текущий статус push-подписки и протокол сайта.

- `permission` : string<br/>
  Текущий статус push-подписки, возможные значения:
  <table style="margin-bottom: 7px;">
    <tr><td>'default'</td><td>по умолчанию</td></tr>
    <tr><td>'denied'</td><td>запрещена</td></tr>
    <tr><td>'granted'</td><td>разрешена</td></tr>
    <tr><td>'unsupported'</td><td>не поддерживается</td></tr>
  </table>
- `requireInteraction` : boolean<br/>
  Нужно ли открывать второе окно (виджет запущен на http сайте).

<h4 style="margin-top: 25px; margin-bottom: 7px;">Запрос разрешения push-подписки</h4>

`widget.pushSubscribe()` : Promise<{ permission }><br/>
Запрашивает разрешение у пользователя и возвращает объект содержащий статус подписки.

- `permission` : string<br/>
  Текущий статус push-подписки, возможные значения:
  <table style="margin-bottom: 7px;">
    <tr><td>'default'</td><td>по умолчанию</td></tr>
    <tr><td>'denied'</td><td>запрещена</td></tr>
    <tr><td>'granted'</td><td>разрешена</td></tr>
    <tr><td>'unsupported'</td><td>не поддерживается</td></tr>
  </table>

<h4 style="margin-top: 25px; margin-bottom: 7px;">Пример использования</h4>

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <style>
      body {
        background-color: #fff;
        display: flex;
        align-items: center;
        justify-content: center;
        height: 150px
      }
      .Button {
        padding: 3px 7px;
        color: blue;
        border: 1px solid rgba(0, 0, 0, 0.1);
        cursor: pointer;
      }
    </style>
    <script>
      widget.ready(function() {
        widget.configure({
          type: 'popup',
          width: '150px',
          height: '150px'
        });

        var body = document.querySelector('body');
        body.classList.remove('is-preview');
        
        widget.pushStatus().then(function(status) {
          if (status.permission !== 'default') { return; }
          if (status.requireInteraction) {
            widget.show();
            return;
          }
          return widget.pushSubscribe().then(function() { widget.hide(1800); });
        })
        
        
        var button = document.querySelector('.Button');
        button.addEventListener('click', function() {
          widget.pushSubscribe().then(function() { widget.hide(1800); });
        })
      });
  	</script>
  </head>
  <body class="is-preview">
      <div data-fl-track="click-subscribe-button" class="Button">Подписаться</div>
  </body>
</html>
```