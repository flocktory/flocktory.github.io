---
layout: doc
title:  "Виджет с несколькими шагами"
section: widget
---

**Screens** - механим переключений состояний (шагов) виджета. Состоит из двух смысловых единиц:

1. Состояния виджета (далее скрины). Данные блоки помечаются атрибутом `data-fl-screen="screenName"`. Где `screenName` любая уникальная строка, являющаяся именем скрина.
2. Метод переключения скринов `widget.setScreen(screenName)`. Где `screenName` имя необходимого к показу скрина.

# Механизм работы

При запуске виджета всем скринам кроме первого по коду задается `display: none;`. В результате мы видим только первый скрин. Дальнейшее переключение скринов осуществляется через метод `widget.setScreen`.

`widget.setScreen(screenName)` задает всем скринам кроме `screenName` `display: none;`, а `screenName` задается `display: block;`.


# Пример работы

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
    <meta name="widget-config" data-type="popup" data-autoshow="true"/>
    <style>
      body { background-color: #fff; }
    </style>
    <script>
      document.addEventListener('DOMContentLoaded', function() {
        var button = document.querySelector('.js-to-reward');
        button.addEventListener('click', function() {
          widget.setScreen('reward');
        });
      });
    </script>
  </head>
  <body>
    <div class="Widget" data-fl-screen="welcome">
      <h1>Welcome</h1>
      <button class="js-to-reward">Go to reward screen</button>
    </div>
    <div class="Widget" data-fl-screen="reward">
      <h1>Reward</h1>
    </div>
  </body>
</html>
```

1. Всем скринам кроме `welcome` задается `display: none;`
2. При клике на `button.js-to-reward` скрину `welcome` задастся `display: none;`, а скрину `reward` `display: block;`
