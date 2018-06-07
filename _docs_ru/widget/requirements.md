---
layout: doc
title:  "Требования к кампании"
section: widget
order: 9
---

Требования к кампании задаются в вёрстке виджета в атрибуте content мета-тега `<meta name="fl-requirements" content="..." />` через точку с запятой.

# Поддерживаемые требования к кампании

- `email-allowed` — можно настроить опциональное письмо.

  *Применение: в кампаниях по сбору емейлов*

- `email-required` – требуется настроить письмо.

  *Применение: в кампаниях по сбору емейлов*

- `cart-threshold-required` — требуется настройка триггеров и порога корзины.

  *Применение: в кампаниях по увеличению среднего чека*

- `motivation-required` — мотивация обязательна.

  *Применение: в кампаниях, показывающих купон или ссылку на лендинг*

- `selector-required` — требуется задать селектор элемента.

  *Применение: для встраиваемых виджетов*


# Примеры использования

```html
<meta name="fl-requirements" content="email-allowed" />
```

```html
<meta name="fl-requirements" content="email-required" />
```

```html
<meta name="fl-requirements" content="cart-threshold-required" />
```

```html
<meta name="fl-requirements" content="cart-threshold-required;motivation-required" />
```

```html
<meta name="fl-requirements" content="motivation-required" />
```

```html
<meta name="fl-requirements" content="selector-required" />
```
