---
layout: doc
title:  "Увеличение среднего чека"
section: precheckout
---

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

