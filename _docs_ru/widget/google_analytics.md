---
layout: doc
title:  "Google Analytics"
section: widget
---

Существует два способа отправить событие в Google Analytics из виджета.

1. добавить к элементу взаимодействие с которым мы хотим отслеживать атрибут `data-fl-track-ga`
2. принудительно вызвать `widget.track`


# data-fl-track-ga

Добавляем необходимому элементу атрибут `data-fl-track-ga`, значением которого будет имя передаваемого события.

<sup>*</sup> При отправке события клика на ссылку вида `a[target=_top]`, будет произведенна задержка в 300ms для отправки события в Google Analytics. После которой произойдет переход.

**Пример использования**

```html
<button data-fl-track-ga="take-reward">Получить подарок</button>
```

В данном примере при клике на кнопку получить подарок произойдет отправка события `take-reward` в Google Analytics.


# widget.track

Для отправки события в Google Analytics из JavaScript кода необходимо воспользоваться методом `widget.track`.

`widget.track` принимает в качестве параметра строковое значение которое и будет отправленно в Google Analytics.

**Пример использования**

```javascript
document.body.addEventListener('click', function() {
  widget.track('User click on body');
});
```
