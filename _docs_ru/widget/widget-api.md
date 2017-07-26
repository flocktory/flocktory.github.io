---
layout: doc
title:  "Widget API"
section: widget
---

В контексте виджета доступен объект **widget**. Он предоставляет ряд методов, позволяющих управлять поведением на сайте, получать необходимые данные о сайте или кампании.

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
