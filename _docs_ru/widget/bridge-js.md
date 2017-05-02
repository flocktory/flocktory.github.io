---
layout: doc
title:  "bridge.js"
section: widget
---

**bridge.js** — это библиотека, встраиваемая в виджеты Flocktory, с помощью которой они позиционируют себя на странице, взаимодействуют с нашим JS-API и обеспечивают авторизацию пользователя.

Для подключения bridge нужно добавить в html-код виджета следующий тэг

```html
<script type="text/javascript" src="//api.flocktory.com/v2/bridge.js"></script>
```

## Декларативные настройки виджета

Декларативные настройки виджета задаются через data-атрибуты на теге `meta[name="widget-config"]`.

### Поддерживаемые data-атрибуты

- `data-autoshow`

  При наличии данного атрибута со значением `true` виджет показывается сразу по готовности.
- `data-type`

  Тип виджета: `popup`, `fixed`, `embedded`.
- `data-width`

  Ширина виджета. Может быть задана числом, пикселями, процентами или auto.

  По умолчанию auto.
- `data-height`

  Высота виджета. Может быть задана числом, пикселями, процентами или auto.

  По умолчанию auto.
- `data-top`
  `data-bottom`
  `data-left`
  `data-right`

  **Работает только для `fixed` виджетов.**

  Данные атрибуты отвечают за позиционирование виджета.
- `data-position`

  **Работает только для `fixed` виджетов.**

  Автоматически позиционирует виджет. Поддерживает следующие значения
  - `top-left`
  - `top-center`
  - `top-right`
  - `center-left`
  - `center`
  - `center-right`
  - `bottom-left`
  - `bottom-center`
  - `bottom-right`

  **Наличие любого из свойств top, bottom, left, right отменяет это свойство!**
- `data-z-index`

  Указывает конкретное значение css свойства z-index.

### Примеры использования

```html
<meta name="widget-config"  data-type="fixed" data-autoshow="true" data-position="top-right" width="100" height="20">
```
Такой виджет покажется автоматически в верхнем правом углу

```html
<meta name="widget-config"  data-type="fixed" data-position="top-right" data-left="0" data-bottom="0">
```
Виджет не покажется. Но при вызове bridge.showWidget(), будет показан в левом нижнем углу.
