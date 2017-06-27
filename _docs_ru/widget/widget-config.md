---
layout: doc
title:  "Декларативные настройки виджета"
section: widget
---

Декларативные настройки виджета позволяют задавать без использования JavaScript:

- тип виджета
- размеры виджета
- позицию виджета
- включать автоматический показ виджета

Декларативные настройки виджета задаются через data-атрибуты на теге `meta[name="widget-config"]`.

# Поддерживаемые data-атрибуты

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

  Указывает конкретное значение css свойства z-index. Работает для виджетов типа fixed и embedded. Для типа виджета popup z-index проставляется равным 2000000000

- `data-overlay-background`

  Цвет фона попапа (hex, rgb, rgba)

  **Работает только для `popup` виджетов.**


# Примеры использования

```html
<meta name="widget-config"  data-type="fixed" data-autoshow="true" data-position="top-right" data-width="100" data-height="20">
```
Такой виджет покажется автоматически в верхнем правом углу


```html
<meta name="widget-config"  data-type="fixed" data-position="top-right" data-left="0" data-bottom="0">
```
Виджет не покажется. Но при вызове widget.show(), будет показан в левом нижнем углу.
