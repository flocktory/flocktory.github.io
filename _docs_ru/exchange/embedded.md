---
layout: doc
title:  "Встроенная витрина"
section: exchange
---

Для установки витрины на сайт добавьте следующий код

```html
<iframe src="https://flocktory.com/interchange/login?ssid=SITE_ID&bid=BID&email=EMAIL&name=NAME">
</iframe>
```

Замените следующие переменные на нужные данные
* SITE_ID — идентификатор вашего сайта в системе flocktory
* EMAIL — email пользователя. Если Неизвестен, то должен быть равен **xname@flocktory.com**
* NAME — имя пользователя (опционально)
* BID — номер баннера (опционально)

Пример

```html
<iframe src="https://flocktory.com/interchange/login?ssid=1234&email=user@gmail.com&name=Mike">
</iframe>
```

## Корректировка высоты

Для корректировки высоты iframe используется postMessage. После загрузки и при изменении размера
iframe отправит сообщение такого вида

```javascript
{ type: 'resize', iframeHeight: 1000 }
```
где 1000 — высота в пикселах.
Важно правильно обрабатывать это сообщение и устанавливать высоту iframe.

## Корректировка прокрутки

Для лучшего пользовательского опыта мы рекомендуем отправлять postMessage в
iframe с указанием прокрутки относительно верхней границы сайта, с учётом отступа самого
iframe. Сообщение должно содержать поле `scrollTop` указывающее отступ.
Например так

```javascript
document.querySelector('iframe').contentWindow.postMessage({ scrollTop: 1000 }, '*');
```

Значение `scrollTop` равно сумме значения прокрутки страницы и некого отступа.
Размер отступа зависит от сайта. Лучше всего узнать его экспериментально.
