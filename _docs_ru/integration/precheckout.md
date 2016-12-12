---
layout: doc
title:  "Precheckout"
section: integration
---

Главная ценность модуля Pre-Checkout заключается в том, что он позволяет запускать механики взаимодействия с пользователями по сложным сигналам, основанным на поведении клиента на сайте. Поэтому крайне важно передавать нужные данные корректно.

## Запуск базового набора механик, включенных в модуль Pre-Checkout

Базовый набор механик Pre-Checkout уже включает большой выбор сигналов и параметров, по которым могут показываться различные виджеты.

В базовый функционал входят следующие параметры (триггеры): был ли пользователь на сайте ранее; количество просмотренных страниц; время на сайте; время на странице; время неактивности пользователя; статус авторизации пользователя на вашем сайте; его демографические характеристики; его история покупок; количество товаров в корзине; общая стоимость корзины; источник, откуда пользователь пришел (либо по referer, либо по utm-меткам).

Для запуска кампаний, основанных на этих триггерах, необходимо:

передать вашему аккаунт-менеджеру или самостоятельно ввести в личном кабинете ссылку на ваш обновляемый YML- или GMF-файл;
установить коды Flocktory в соответствии с приведенными ниже инструкциями — вручную или с помощью Google Tag Manager.

Для работы модуля Precheckout должна быть произведена
[Общая интеграция]({{ site.baseurl }}{% link _docs_ru/integration/general.md %})

## Код идентификации авторизованных пользователей

Поместите этот код на все страницы вашего сайта:

```html
<div class="i-flocktory" data-fl-user-name="Ivan Petrov" data-fl-user-email="ivan@petrov.ru"></div>
```

В Google Tag Manager

```html
<div class="i-flocktory" data-fl-user-name={{user_name}} data-fl-user-email={{user_email}}></div>
```

![GTM int](https://assets.flocktory.com/assets/help/user_auth-fc36e6c6cc4290cb89e9c5b86730fc31.png)

### Параметры

| Параметр     | HTML               | Переменная GTM  |         Знычение                                   |
|:------------:|:------------------:|:---------------:|:----------------------------------------------:|
| user → name  | data-fl-user-name	| user_name       | Полное имя пользователя. Например: Ivan Petrov |
| user → email | data-fl-user-email	| user_email      | Email пользователя. Например: ivan@petrov.ru   |


Важно! Вы можете создать новые переменные, передающие имя и email пользователя, или использовать уже имеющиеся. Однако в переменных, используемых для Flocktory, не должно быть значений по умолчанию, отличных от пустых. Если уже существующие переменные имеют определенное значение по умолчанию, создать новые — необходимо.

Важно! Если вы не знаете имя или почту пользователя, не передавайте нам их.

## Код идентификации категорий товаров

На всех страницах товарного каталога вставьте следующий код

```html
<div class="i-flocktory" data-fl-action="track-category-view" data-fl-category-id="123"></div>
```

### В Google Tag Manager

```html
<div class="i-flocktory" data-fl-action="track-category-view" data-fl-category-id={{category_id}}></div>
```

![GTM int](https://assets.flocktory.com/assets/help/catalogue_pages-9cf0327686020aa421e1630d783b5a96.png)

Настройка страниц, на которых выполняется данный код, может производиться как по содержанию URL (например, URL содержит /catalog/, либо с использованием регулярного выражения), так и по переменной (в самом простом случае — по переменной из вашего Data Layer, например pagetype = catalog).

Для определения страниц, на которых будет запускаться данный тэг через переменные, необходимо создать триггер типа PageView в разделе Triggers.

| Параметр      | HTML                | Переменная GTM  |         Значение                                   |
|:-------------:|:-------------------:|:---------------:|:----------------------------------------------:|
| category → id | data-fl-category-id	| category_id     | ID категории, соответствующий ID категории из вашего YML- или GMF-файла|

## Код идентификации товара

На всех страницах товаров на вашем сайте вставьте следующий код:

```html
<div class="i-flocktory" data-fl-action="track-item-view" data-fl-item-id="123"></div>
```

### В Google Tag Manager

```html
<div class="i-flocktory" data-fl-action="track-item-view" data-fl-item-id={{item_id}}></div>
```

![GTM int](https://assets.flocktory.com/assets/help/product_pages-7d492acef5bc9be86b7a900095727bf2.png)

| Параметр  | HTML            | Переменная GTM  |         Значение                                   |
|:---------:|:---------------:|:---------------:|:----------------------------------------------:|
| item → id | data-fl-item-id	| item_id         | ID товара, соответствующий ID товара из вашего YML- или GMF-файла |

## Код состояния корзины

Чтобы модуль Pre-Checkout имел актуальную информацию о состоянии корзины, необходимо передавать нам данные о ее состоянии, когда оно меняется.

При добавлении товара в корзину необходимо вызывать следующий JavaScript-код:

```javascript
window.flocktory = window.flocktory || [];
window.flocktory.push(['addToCart', {
	item: {
		"id": "123", // product id
		"price": 1200, // product price
		"count": 1 // quantity of this product added
	}
}])
```

А при удалении товара из корзины — следующий

```javascript
window.flocktory = window.flocktory || [];
window.flocktory.push(['removeFromCart', {
	item: {
		id: '124', // product id
		count: 2 // quantity of this product removed
	}
}]);
```

### Параметры

| Параметр     |         Значение                                                  |
|:------------:|:-----------------------------------------------------------------:|
| item → id    | ID товара, соответствующий ID товара из вашего YML- или GMF-файла |
| item → price | Цена добавленного товара                                          |
| item → count | Количество добавленных единиц товара                              |

<br/>
### В Google Tag Manager

#### Случай 1. Данные Enhanced Ecommerce передаются в ваш Data Layer

Если вы передаете [Enhanced Ecommerce](https://support.google.com/analytics/answer/6014841?hl=en) в ваш Data Layer в соответствии с [инструкцией](https://developers.google.com/tag-manager/enhanced-ecommerce#cart), вам необходимо установить тэги и создать все нужные переменные в соответствии с инструкцией ниже. Все необходимые данные при этом уже находятся в Data Layer.

