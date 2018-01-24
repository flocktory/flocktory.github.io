---
layout: doc
title:  "Bridge API (deprecated)"
section: widget
---

## bridge.js является устаревшей библиотекой. В ближайшее время все используемые виджеты будут переведены на новое api. Новые виджеты должны быть сделаны только через [widget-api]({{ site.baseurl }}{% link _docs_ru/widget/widget-api.md %}).

### Использование bridge.js
На сайте партнёра при срабатывании триггеров кампании виджет загружается в iframe.
Для работы виджета внутри необходимо подключить скрипт
```html
<script type="text/javascript" src="//api.flocktory.com/v2/bridge.js"></script>
```
Этот скрипт инициализирует API и добавляет переменную `window.bridge`, которая используется для вызова методов API. Так же скрипт добавляет обработчики некоторых атрибутов на DOM элементах.

### bridge.onReady()
Метод получает функцию, которая будет выполнена после загрузки виджета и инициализации API. Вся логика, взаимодействующая с bridge должна выполняться после вызова этого метода.

### bridge.$
Инстанс jQuery, который можно использовать для работы с DOM.

### bridge.showWidget
Метод для показа виджета (по умолчанию он спрятан).

### bridge.positionWidget() и bridge.resizeWidget()
Это два метода используются для позиционирования и изменения размера iframe виджета. Оба возвращают Promise после того как изменение завершено.
`bridge.positionWidget` получает объект со свойствами `type` и `css`. Свойство `type` показывает как виджет должен быть отображён на сайте. Может принимать следующие значения:
* popup – виджет будет отображен по центру экрана, с затемняющей подложкой. На мобилках займёт всю ширину,
* fixed – iframe виджета позиционируется фиксировано на странице
* embedded – виджет встраивается в DOM клиентской страницы (смотри раздел "Встроенные виджеты" ниже)
Поле `css` – объект со стилями для оберкти виджета.
`bridge.resizeWidget` получает объект со свойствами width и height. Значения могут быть числами (пиксели), строками с процентами, вида `"100%"` или словом `"auto"`. Пример позиционирования виджета и показа виджета (порядок методов важен):

```clojure
bridge.positionWidget({type: 'fixed', css: {zIndex: 100}})
  .then(function () {
    return bridge.resizeWidget({width: 256, height: 340})
  })
  .then(function() {
    bridge.showWidget();
  });
});
```

### Экраны (скрины)
Для облегчения редактирования виджета и удобства разработки в виджетах есть понятие скринов.
Скрины - это набор блоков, не вложенных друг в друга и обычно на одном уровне. Для того, чтобы отметить блок как скрин, нужно добавить ему атрибут `fl-screen`, значение которого - имя скрина.
Виджет в один момент времени показывает только один скрин. По умолчанию все скрины скрыты. Переключать скрины можно с помощью вызова метода `bridge.setScreen(name)` где `name` – имя скрина.
Наиболее часто скрины используются в формах. Первый скрин – форма с полями имени и мейла, а второй скрин информация о том, что отправка завершена. Обычный способ использования метода `bridge.setScreen` – внутри обработчиков различных эвентов API или от пользователя.
_Особенностью скринов является то, что переключение между ними логируется в аналитику._
Пример виджета со скринами.

```clojure
<!doctype html>
<html><head>
<meta charset="utf-8">
  <script type="text/javascript" src="//api.flocktory.com/v2/bridge.js"></script>
  <script type="text/javascript">
    bridge.onReady(function () {
      bridge.setScreen('login');
      bridge.positionWidget({type: 'popup'}).then(function () {
        bridge.resizeWidget({width: 566, height: 396}).then(function () {
          bridge.showWidget();
        });
      });

      bridge.events.on('logged', function () {
        bridge.setScreen('success');
      });

      bridge.$('.js-back').on('click', function () {
        bridge.setScreen('login');
      });

      bridge.$('.js-conditions').on('click', function () {
        bridge.setScreen('conditions');
      });
    });

  </script>
</head>
  <body class="is-preview" current-edited-element="current-edited-element">
    <div class="Widget">
      <div fl-screen="login" class="Widget-screen">
        <div fl-close="" fl-track-ga="click-close-login" class="Widget-close">×</div>
        <div class="Widget-container u-clearfix">
          <div class="Widget-col Widget-colLeft">
            <div class="Widget-reward"></div>
          </div>
          <div class="Widget-col Widget-colRight">
            <div class="Widget-title">ОСТАВЬ СВОЙ НОМЕР ТЕЛЕФОНА И ПОЛУЧИ СКИДКУ 10% НА ВСЕ МОДЕЛИ ESSENCE</div>
            <div class="Widget-form">
              <form fl-login-form="" class="Login-form">
                <div class="Login-formItem">
                  <div class="Input-inner">
                    <input type="text" fl-login-form-data="" name="telephone" class="Input">
                       <div fl-login-form-error="email" class="Error">
                      <div class="Error-arrow"></div>
                      <div class="Error-inner">Проверьте корректность электронной почты</div>
                    </div> -->
                  </div>
                  <div class="Comment">Продолжая, вы принимаете<span class="Conditions-link js-conditions"> условия продаж</span></div>
                </div>
                <div class="Login-formItem Login-formItem--withButton">
                  <button type="submit" class="Button">
                  Жду звонка</button>
                  <input type="hidden" fl-login-form-data="" name="decision" value="true">
                   <input type="hidden" fl-login-form-email="" name="em" value="xname@flocktory.com">
                </div>
              </form>
            </div>
          </div>
        </div>
      </div>
      <div fl-screen="success" class="Widget-screen">
        СПАСИБО, В ТЕЧЕНИИ ПОЛУЧАСА С ВАМИ СВЯЖЕТЬСЯ ВАШ ПЕРСОНАЛЬНЫЙ МЕНЕДЖЕР
      </div>
      <div fl-screen="conditions" class="Widget-screen">
        Текст пользовательского соглашения
        <div class="Widget-back"><span fl-track-ga="click-back" class="js-back">Назад</span></div>
      </div>
    </div>
</body></html>
```

### Закрытие виджета
В виджете могут быть элементы закрывающие виджет. Обычно это крестик в правом верхнем углу.
Так же виджет может быть закрыт согласно какому то событию.

#### Закрытие кликом на элемент
Если на элемент добавить атрибут `fl-close`, то при клике на этот элемент виджет будет закрыт. При этом будет отправлено событие **close-widget**. Если нужно поменять называние события (напрмиер, когда есть несколько закрывающих эоементов), то можно добавить атрибут `fl-track-ga` со значением события. Пример:
```html
<span fl-close fl-track-ga="close-by-span">Close</span>
```
**Важно**: если `fl-close` используется на ссылку, которая никуда не должна вести, на ней не должно быть атрибута `href`

#### Закрытие через JS-API
Для второго случая подходит вызов метода `bridge.hideWidget()`. Который делает тоже самое.
Можно подписаться на событие закрытия виджета.
```javascript
bridge.events.on('hide', function() { ... });
```
После закрытия виджета, кампания не будет срабатывать, а виджет показыватьв течение полчучаса. Для того, чтобы продолжить разработку рекомендуется очистить localStorage.



### Отправка формы сбора мейлов (подходит для сбора телефонов)
Сбор мейла является логином пользователя. Когда пользователь заполнил форму и нажал отправить нужно вызвать метод `bridge.login(options)`. options содержит следующие поля
* name - имя пользователя или пустая строка
* email – меил пользователя или строка `xname@flocktory.com` если собираются телефоны
* subscribe – булево значение, указывающее отметил ли человек галочку про подписку (если такой нет, то `true`)
* data – объект, дополнительные данные. Содержит обязательный булев параметр userInteract, указывающий сам ли пользователь залогинился заполнив форму, или это сделали без его ведома. Так же это поле может содержать любые дополнительные данные (вложеные объекты не разрешены). Например, может присутствовать поле `phone` для указания номера телефона, оставленого пользователем.
Можно подписаться на успешную отправку логина
```javascript
bridge.events.on('logged', function() { ... });
```

### Дополнительные события
Если на виджете есть кнопки, ссылки или другие элементы с которыми пользователь может взаимодействовать, то нужно отправлять статистику с этих элементов. Если событием является клик, то можно добавить атрибут `fl-track-ga="event-name"`, где `event-name` нужно заменить на уникальное имя события. Имена выбираются произвольно. Желательно давать краткие, но понятные имена. Например: click-facebook-link, hover-header, и так далее.



# Bridge-виджет "Увеличение среднего чека"

## Описание
Виджет показывает пользователю мотивацию с целью увеличения размера корзины.

### Пример
Пользователь набрал товаров на сумму 10000 рублей, срабатывает кампания (соответствующий триггер включен). Виджет показывает пользователю информацию, что если он наберёттоваров на 15000 то получит бесплатную доставку.

## Реализация
Виджет работает как обычный пречекаут. Но с небольшим добавлением.
Нужно подключить скрипт брошеной корзины.

```
<script type="text/javascript" src="https://assets.flocktory.com/u_widget/js/widgets/precheckout_general/check/cart-watcher-284375deb4.js"></script>
```

После чего использовать глобальный объект `AmountWatcher` во время инициализации виджета.

```
bridge.onReady(function () {
  bridge.positionWidget({type: 'fixed', css: {left: 0, bottom: 0}})
    .then(function () {
      return bridge.resizeWidget({width: 'auto', height: 200});
    })
    .then(function () {
      new AmountWatcher(bridge);
    });
});
```

При этом в виджете нет вызова `bridge.showWidget`. Так как он вызавается внутри `AmountWatcher` в момент когда пользователь наберет нужное количество товара.
Так же необходимо соответствующим образом реализовать HTML виджета.
Содержимое виджета должно быть разбито на два скрина.
Первый с именем `offer` будет показан когда пользователь не добрал до нужного значения. На этом скрине показывается призыв к действию.
Второй с именем `reward` будет показан когда пользователь набрал нужное количество товаров. На этом скрине показывается вознаграждение.

## Важные детали
На первом скрине `offer`. Могут присутствовать кнопки, который должны закрыть виджет. Обычно это крестик или кнопка "спасибо, я понял". Однако тут нельзя использовать атрибут `fl-close`, так как он заблокирует кампанию. Вместо этого на элементе, который должен скрыть виджет до момента, когда пользователь наберет нужное количество товара нужно указать атрибут `fl-watcher-hide-offer`. Так же на такой эелемент нужно добавить атрибут `fl-track-ga` со произвольным значением события (например, стандартный вариант `close-offer`).
На втором скрине `reward` обычно подходит `fl-close`, который просто закрывает виджет и блокирует кампанию.

##  Пример

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" href="https://assets.flocktory.com/u_widget/css/widgets/precheckout_general/check/uservoice-v2-40426cfd34.css">
    <script type="text/javascript" src="//api.flocktory.com/v2/bridge.js"></script>
    <script type="text/javascript" src="https://assets.flocktory.com/u_widget/js/widgets/precheckout_general/check/cart-watcher-284375deb4.js"></script>
    <script>
      bridge.onReady(function () {
        bridge.positionWidget({type: 'fixed', css: {right: 0, top: '50%', marginTop: -120}}).then(function () {
          bridge.resizeWidget({width: 270, height: 'auto'}).then(function () {
            new AmountWatcher(bridge);
          });
        });
      });
    </script>
  </head>
  <body class="is-preview">
    <div fl-screen="offer" class="Floating">
      <div class="Heading"><span>Добавь в корзину товаров еще&nbsp;на&nbsp;</span><span id="till-reward">1000</span><span>&nbsp;руб.</span></div>
      <div class="Description">Получи скидку 10% на&nbsp;текущий заказ:</div>
      <div fl-watcher-hide-offer fl-track-ga="close-offer" class="PseudoButton">Спасибо, я понял</div>
    </div>
    <div fl-screen="reward" class="Floating">
      <div class="Heading"><span>Поздравляем, вы получили скидку 10%</span></div>
      <div class="Description"></div>
      <div fl-close fl-track-ga="close-reward" class="PseudoButton">Спасибо, я понял</div>
    </div>
  </body>
</html>
```
