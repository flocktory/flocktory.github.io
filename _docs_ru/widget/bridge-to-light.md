---
layout: doc
title:  "Переход с bridge.js на widget-api"
section: widget
order: 3
---

Данная статья призвана помочь разработчикам с переделкой виджетов c bridge.js на widget-api (легкие).


## bridge.js vs widget-api

bridge.js – старая javascript-библиотека, основанная на AngularJs и jQuery.

widget-api – подход к разработке виджетов основанный на чистом js и data-атрибутах.

<br>

## Декларативные конструкции bridge и аналог в widget-api

Описание | bridge.js | widget-api |
|:------:|:------------:|:------------------:|
Закрытие виджета на время сессии | ``` fl-close="" ``` | ``` data-fl-close="1800" ```
Закрытие виджета до следующего обновления страницы | ``` fl-close="0" ``` | ``` data-fl-close="" ```
Трекинг события на элементе | ``` fl-track-ga="click-close-login" ``` | ``` data-fl-track="click-close-login" ```
Экран виджета | ``` fl-screen="login" ``` | ``` data-fl-screen="login" ```


[Детально про декларативные настройки]({{ site.baseurl }}{% link _docs_ru/widget/widget-config.md %})

<br>

## Методы bridge и аналог в widget-api

Описание | bridge.js | widget-api |
|:------:|:------------:|:------------------:|
Позиционирование виджета | ``` bridge.positionWidget ``` | ``` widget.configure ```
Изменение размера виджета | ``` bridge.resizeWidget ``` | ```widget.configure```
Показать виджет | ```bridge.showWidget()``` |```widget.show()```
Скрыть виджет | ```bridge.hideWidget()``` | ```widget.hide()```
Скрыть виджет на время сессии | ```bridge.hideWidgetOnPeriod()``` | ``` widget.hide(1800) ```
Отправка данных из email-сборщиков | ```bridge.login({email: email, name: name, data: {decision: 'true'})``` | ```widget.collectEmail(email, name, {'subscription': 'true'})```
Отправка данных из виджетов-опроса | ```bridge.customerActions({emai: email, name: name, isTest: 'on'}});``` | ```widget.collectEmail(email, name, {'test': 'true'})```


[Детально про методы widget-api]({{ site.baseurl }}{% link _docs_ru/widget/widget-api.md %})

### Пример:

#### Информационный виджет с одним экраном, ссылкой и крестиком закрытия

**bridge.js**
```
<html>
    <head>
        <script type="text/javascript" src="//api.flocktory.com/v2/bridge.js"></script>
          <script type="text/javascript">
            bridge.onReady(function () {
              bridge.positionWidget({type: 'fixed', css: {bottom: 20, right: 1, zIndex:100500 }})
                .then(function () {
                  return bridge.resizeWidget({width: 280, height: 150})
                })
                .then(function () {
                   bridge.showWidget();
                });
            });
          </script>
    </head>
<body>
    <div class="Widget">
      <div class="Widget-close" fl-close="">×</div>
      <div class="Widget-inner">
        У нас появилась косметика для детей!
        <a href="https://site.org/" class="Link-button" target="_blank" fl-track-ga="click-link">Узнать больше</a>
      </div>
    </div>

</body>
</html>
```

Весь код bridge в данном примере можно описать через [делкаративные настройки]({{ site.baseurl }}{% link _docs_ru/widget/widget-config.md %}).
Также заменяем директивы ```fl-close``` и ```fl-track-ga``` на ```data-fl-close="1800"``` и ```data-fl-track```

**widget-api**

```
<html>
    <head>
        <meta name="widget-config" data-type="fixed" data-width="280px" data-height="150px" data-bottom="20px" data-right="1px" data-z-index="100500" data-autoshow="true">
    </head>
<body>
    <div class="Widget">
      <div class="Widget-close" data-fl-close="1800">×</div>
      <div class="Widget-inner">
        У нас появилась косметика для детей!
        <a href="https://site.org/" class="Link-button" target="_blank" data-fl-track="click-link">Узнать больше</a>
      </div>
    </div>
</body>
</html>
```
