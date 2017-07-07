---
layout: doc
title:  "Письмо брошенной корзины"
section: emails
---

## Описание
Письмo с брошеной корзиной отправляется клиенту, когда он набрал корзину на сайте, ничего не купил и покинул сайт. В письме перечисляются товары из корзины и есть призыв вернуться.
Также в письме может быть список рекомендованых товаров.

## Требования

В письмах используется шаблонизатор [Liquid](https://shopify.github.io/liquid/).
Письма должны работать в следующих клиентах

* Outlook 2013 и выше
* Gmail web
* Mailru web
* Yandex web
* OSX mail.app
* Gmail IOS
* IOS mail
* Gmail Android

Письма не прогоняются через инлайнеры. Только через ликвид шаблонизатор.

В письме можно использовать liquid переменные таким образом

```clojure
{% raw %}{{ partner_name }}{% endraw %}
```

## Список доступных переменных
* partner_logo – логотип сайта партнёра
* partner_title – название сайта партнёра
* site_url – адрес сайта партнёр
* unsubscribe_url – ссылка "отписаться"
* motivation_reward_url – ссылка на мотивацию. Обычно это ссылка на кнопках вида "Получить скидку"
* motivation_reward_coupon - код купона

## Переменные с товарами
массив объектов
* recommended_items – рекомендованые товары
* cart_items – товары в корзине
* visited_items - посещенные товары
* order_items - купленные товары
* category_top_items - самые покупаемые по количеству за последние две недели товары
* category_trending_items - товары, которые стали покупать чаще в последние две недели
* category_grossing_items - товары, на которые было потрачено больше всего денег за последние две недели
* category_random_items - случайные товары из категории

## Переменные с одним товаром
содержит один объект с товаром
* best_cart_item - лучший (самый дорогой товар из корзины)
* best_visited_item - лучший просмотренный товар
* best_order_item - лучший купленный товар



## Фильтры в письме
В ликвиде есть следующие фильтры
```clojure
{% raw %}{{ partner_name }}{% endraw %}
```


* money – форматирует валюту
* modulo – остаток от деления, использование: ```{% raw %}{{ 12 | module:5 }}{% endraw %} ``` - 2
* round – округление цены
* truncate - обрезает строку до нужного размера

    Пример:

    ```{% raw %}{{ cart_item.description | truncate: 60 }}{% endraw %}``` - обрежет описание товара до 60 символов и добавит многоточие в конце строки.

    ```{% raw %}{{ cart_item.description | truncate: 60, "---" }}{% endraw %}``` - обрежит описание товара до 60 символов и добавит `---` в конце строки.

## Итерирование по товарам
Список **cart_items** можно использовать для итерации по товаром и отображения.
Каждый элемент коллекции содержит следующие поля

* id - идентификатор
* title – название
* price – цена
* url - ссылка на товар
* pic-url – картинка
* description - описание

Пример итерации по товарам:

```clojure
{% raw %}<table>
{% for cart_item in cart_items %}
  <tr>
    <td>{{ cart_item.title }}</td>
    <td><img src="{{ cart_item.pic-url }}" alt="{{ cart_item.title }}"></img></td>
    <td>{{ cart_item.price | money }} РУБЛЕЙ</td>
  </tr>
{% endfor %}
</table>{% endraw %}
```

## Итерирование по рекомендованным товарам
Список **recommended_items** содержит рекомендованные пользователю товары.
Каждый эелемент списка содержит следующие поля:

* title – название
* pic-url - ссылка на картинку
* url - ссылка на элемент

Пример итерации по рекомендованным товарам с разбивкой по три (стили вырезаны)

```clojure
{% raw %}<table width="100%" style="border-spacing:0;border-collapse:collapse;font-family:Arial,'Helvetica Neue',Helvetica,sans-serif;color:#333333;">
  <tbody>
    <tr>{% assign inrow = 3 %}{% for item in recommended_items limit: 6 %}{% assign mod = forloop.index | modulo: inrow %}
      <td width="33%" valign="top" style="padding:0;">
        <div class="column goods_x3_item" style="width:100%;max-width:200px;display:inline-block;vertical-align:top;">
          <table width="100%" style="border-spacing:0;border-collapse:collapse;font-family:Arial,'Helvetica Neue',Helvetica,sans-serif;color:#333333;">
            <tbody>
              <tr>
                <td class="inner" style="padding:10px;">
                  <table class="contents" style="">
                    <tbody>
                      <tr>
                        <td class="image" style="">
                          <a href="{{ item.url}}" target="_blank"><img src="{{ item['pic-url'] }}?&scale=both" class="img" width="170" height="170" border="0" alt="" style="">
                          </a>
                        </td>
                      </tr>
                      <tr>
                        <td class="name" style=""><a href="{{ item.url }}" target="_blank" class="link" style=""><span style="font-size: 12px;">{{ item.other-data.TypePrefix}}</span></a>
                        </td>
                      </tr>
                      <tr>
                        <td class="name" style="">{% if item.model %}<a href="{{ item.url }}" target="_blank" class="link" style=""><span style=""><b>Метро</b> {{ item.model }}</span></a>{% else %}
                          <div style="">&nbsp;</div>{% endif %}</td>
                      </tr>
                      <tr>
                        <td class="price" style=""><span style="font-size: 12px"><b>Цена: {{item.price | round | money}} руб.</b></span>
                        </td>
                      </tr>
                      <tr>
                        <td class="name" style=""><a href="{{ item.url }}" target="_blank" class="link" style=""><span style=""><b>Этаж:</b> {{ item.other-data.floor }} / {{ item.other-data.floors }}</span></a>
                        </td>
                      </tr>
                      <tr>
                        <td class="name" style=""><a href="{{item.url}}" target="_blank" class="link" style=""><span style=""><b>Общая площадь:</b> {{ item.other-data.squareFull}}</span></a>
                        </td>
                      </tr>
                      <tr>
                        <td style=""><a href="{{ item.url }}" target="_blank" style="">ПОСМОТРЕТЬ</a>
                        </td>
                      </tr>
                    </tbody>
                  </table>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </td>{% if mod == 0 and forloop.last != true %}</tr>
    <tr>{% endif %}{% endfor %}</tr>
  </tbody>
</table>{% endraw %}
```



## Загрузка статики
Все картинки и внешние ресурсы должны быть залиты на cdn.
Это можно сделать по [https://cabinet.flocktory.com/upload?site_id=2145](https://cabinet.flocktory.com/upload?site_id=2145). Где номер 2145 надо заменить на ID демошопа или сайта партнёра.
После загрузке будет доступен URL, который можно использовать в письме.

![Upload](https://assets.flocktory.com/uploads/clients/1791/fea56eea-6851-47a7-aa96-e666bb712ea6_EA320104C3A3B5BC769272B6D2CE4511.png)

## Добавление в кабине

Зайти в раздел precheckout

![Precheckout](https://assets.flocktory.com/uploads/clients/1791/beb54f28-3162-4229-8ff6-0c7e19038bbc_7BC69BFA0D6CD929DCC5F531C9472AD1.png)

Выбрать подраздел "Кампании" и добавить новую

![Add campaign](https://assets.flocktory.com/uploads/clients/1791/86c9cd44-be44-40b8-8f2b-eabdebd66c87_B5B73049B774D753A60096113C2B3712.png)

В открывшемся интерфейсе написать название, добавить письмо и создать новый видежт (или выбрать из существующих)

![Create](https://assets.flocktory.com/uploads/clients/1791/e185e544-81c3-4b50-a9c0-d47fa0d5ccd7_850EF3060D0FBCDE19DE07C7A5C4DDD8.png)

В открывшемся редакторе добавить весь код и нажимаем сохранить?

![Save code](https://assets.flocktory.com/uploads/clients/1791/155d36c2-d0d4-44b9-8ea5-f55438e97bd3_1496E22C3C657453D86BB85127C3ED9D.png)

Указать любою тему письма, сохранить кампанию

![Save campaign](https://assets.flocktory.com/uploads/clients/1791/e3993fe3-d383-4561-9cd7-639824d54ddf_F9A4D1E21F24985EC31070AEA6FC2863.png)
