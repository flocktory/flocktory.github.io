---
layout: doc
title:  "Widget API"
section: widget
---

В контексте виджета доступен объект **widget**. Он предоставляет ряд методов, позволяющих управлять поведением виджета на сайте, получать необходимые данные о сайте или кампании.

## widget.ready

```javascript
widget.ready(callback);
```

**Описание**

Метод получает callback-функцию, которая будет выполнена после загрузки и инициализации виджета. Срабатывает `widget.ready` после наступления **load** события объекта `window`. К этому моменту все ресурсы виджета загружены (стили, js, картинки).

**Вся логика взаимодействия с виджетом должна выполняться после вызова ```widget.ready```.**

**Пример:**
```javascript
widget.ready(function() {
  var $conditions = document.querySelector('.js-conditions-container');

  document.querySelector('.js-toggle-conditions').addEventListener('click', function() {
    $conditions.classList.toggle('is-visible');
  });

  // позиционируем виджет
  widget.configure({
    type: 'popup',
    height: 500,
    width: 500
  });
  widget.show();
});

```

## widget.configure

```javascript
widget.configure(object)
```

**Описание**

Управляет положением на сайте, позволяет указать тип и размеры виджета


**Параметры**

Объект в формате ключ/значение

- `type (String)` - тип виджета: `'popup'`, `'embedded'`, `'fixed'`


- `position (String)` - положение виджета на сайте


  Доступные значения:
     - `'top-left'`
     - `'top-center'`
     - `'top-right'`
     - `'center-left'`
     - `'center'`
     - `'center-right'`
     - `'bottom-left'`
     - `'bottom-center'`
     - `'bottom-right'`


  **Работает только для fixed виджетов. Наличие любого из свойств `top`, `bottom`, `left`, `right` отменяет это свойство!**


- `width (Number|String)` - ширина виджета. Может быть задана числом, пикселями, процентами или auto


- `height (Number|String)` - высота виджета. Может быть задана числом, пикселями, процентами или auto


- `top`, `right`, `bottom`, `left` - данные атрибуты отвечают за позиционирование виджета


  **Работает только для fixed виджетов.**


- `zIndex` - управляет z-index виджета


  **Работает для виджетов типа fixed и embedded. Для типа виджета popup z-index проставляется равным 2000000000.**


- `overlayBackground` - цвет оверлея виджета


  **Работает для виджетов типа popup.**





**Пример:** Попап-виджет шириной 400px, высотой 300px

```javascript
widget.configure({
  type: 'popup',
  width: 400,
  height: 300,
  overlayBackground: 'rgba(0,0,0,0.8)'
});
```


**Пример:** Виджет с `position: fixed`, выровненный по вертикале, расположен в левом углу
```javascript
widget.configure({
  type: 'fixed',
  width: 450,
  height: 360,
  position: 'center-left',
  zIndex: 100500
});
```


## widget.getData

```javascript
var widgetData = widget.getData(); // {cid: "100500", siteId: 1559}
```

**Описание**

Возвращает объект, содержащий id сайта и кампании.

- `cid  (String)` - id кампании
- `siteId (Number)` - id сайта


## widget.collectEmail

```javascript
  widget.collectEmail(email, name, data).then(function(){})
```


**Описание**

Используется для сбора имейла, имени, дополнительных данных, оставленных пользователем в виджете.


**Параметры**

- `email (String)` - имейл пользователя. Обязательный параметр.
- `name (String)` - имя пользователя. Необязательный параметр.
- `data (Object)` - объект в формат ключ/значение. Необязательный параметр.

**Возвращаемое значение**

Promise-объект

**Пример: отправить имейл, имя, дополнительные данные**
```javascript
  widget.collectEmail('keks@gmail.com', 'Эдуард', {phone: '88002000600', decision: 'true'}).then(function() {
    widget.setScreen('thank-you');
  });
```


**Пример: отправить только имейл**
```javascript
  widget.collectEmail('gunman@mailbox.net').then(function() {
    widget.setScreen('thank-you');
  });
```


## widget.show

```javascript
widget.show();
```


**Описание**

Позволяет показать виджет.

**Важно**: при вызове метода в аналитику отправляется событие `show-widget`, поэтому достаточно одного вызова ```widget.show()``` на виджет.


## widget.hide

```javascript
widget.hide(seconds);
```

**Параметры**

- `seconds (Number)` - время закрытия кампании в секундах. Необязательный параметр.

**Описание**

Позволяет скрыть виджет. Полезен, если перед закрытием нужно добавить анимацию или другую логику. Если виджет должен закрываться по клику на элеменет
без дополнительной логики -- испоьзуйте атрибут `data-fl-close`.

**Пример:** Добавить css-класс html-элементу, после чего скрыть виджет на 30 минут.
```javascript
$closeEl.addEventListener('click', function() {
  $widgetEl.classList.add('fade-out');

  setTimeout(function(){
    widget.hide(1800);
  }, 300)
});
```

**Пример:** Добавить css-класс html-элементу, после чего скрыть виджет. При обновлении страницы виджет покажется снова, если выполнятся настройки срабатывания кампании.
```javascript
$closeEl.addEventListener('click', function() {
  $widgetEl.classList.add('fade-out');

  setTimeout(function(){
    widget.hide();
  }, 300)
});
```

## widget.setScreen

```javascript
  widget.setScreen(screenName);
```

**Параметры**

- `screenName (String)` - название состояния виджета (скрин), указывается через атрибут `data-fl-screen`

**Описание**

Позволяет переключаться между состояниями виджета (скринами). Более детально в разделе [Виджет с несколькими шагами](/ru/widget/screen/).

**Пример: показать скрин пользовательского соглашения по клику на псевдоссылку**

```javascript
$agreementPseudo.addEventListener('click', function() {
  widget.setScreen('agreement');
});
```


## widget.track

```javascript
  widget.track(eventName);
```

**Параметры**

- `eventName (String)` - имя события

**Описание**

Отправляет событие в аналитику. Более детально в разделе [Аналитика](/ru/widget/analytics/).
