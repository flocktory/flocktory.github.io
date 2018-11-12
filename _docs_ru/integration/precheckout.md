---
layout: doc
title:  "Precheckout"
section: integration
---

Внимание! Перед подключением модуля необходимо произвести [общую интеграцию]({{ site.baseurl }}{% link _docs_ru/integration/general.md %}) и интеграцию [Post-Checkout]({{ site.baseurl }}{% link _docs_ru/integration/postcheckout.md %})

## Запуск базового набора механик, включенных в модуль Pre-Checkout

Базовый набор механик Pre-Checkout уже включает большой выбор сигналов и параметров, по которым могут показываться различные виджеты.

В базовый функционал входят следующие параметры (триггеры): был ли пользователь на сайте ранее; количество просмотренных страниц; время на сайте; время на странице; время неактивности пользователя; статус авторизации пользователя на вашем сайте; его демографические характеристики; его история покупок; количество товаров в корзине; общая стоимость корзины; источник, откуда пользователь пришел (либо по referer, либо по utm-меткам).

Для запуска кампаний, основанных на этих триггерах, необходимо:

передать вашему аккаунт-менеджеру или самостоятельно ввести в личном кабинете ссылку на ваш обновляемый YML- или GMF-файл;
установить коды Flocktory в соответствии с приведенными ниже инструкциями — вручную или с помощью Google Tag Manager.

Для работы модуля Precheckout должна быть произведена
[Общая интеграция]({{ site.baseurl }}{% link _docs_ru/integration/general.md %})

<a id="auth-user"></a>

## **Код идентификации авторизованных пользователей**

Поместите этот код на все страницы вашего сайта:

```html
<div class="i-flocktory" data-fl-user-name="Ivan Petrov" data-fl-user-email="ivan@petrov.ru"></div>
```

В Google Tag Manager

```html
{% raw %}<div class="i-flocktory" data-fl-user-name={{user_name}} data-fl-user-email={{user_email}}></div>{% endraw %}
```

![GTM int](https://assets.flocktory.com/assets/help/user_auth-fc36e6c6cc4290cb89e9c5b86730fc31.png)

### Параметры

| Параметр     | HTML               | Переменная GTM  |         Значение                               |
|:------------:|:------------------:|:---------------:|:----------------------------------------------:|
| user → name  | data-fl-user-name	| user_name       | Полное имя пользователя. Например: Ivan Petrov |
| user → email | data-fl-user-email	| user_email      | Email пользователя. Например: ivan@petrov.ru   |

#### На что обратить внимание:
Для не авторизованных пользователей:

* Тег должен отсутствовать на странице

Для авторизованных пользователей:

* Код должен передаваться не только после авторизации, а на всех страницах, где есть интеграция.
* Должны передаваться данные сессии текущего пользователя. Не должно быть передачи чужих авторизационных данных.
* Должны передаваться и имя пользователя, и e-mail, если они известны.

Передача name:

* Имя нужно передавать в порядке "Имя Фамилия".
* Если имя неизвестно, соответствующий атрибут должен отсутствовать.

Передача e-mail:

* E-mail по умолчанию должен передаваться НЕ в хешированном виде, а в нормальном.
* Если email неизвестен, нужно передавать телефонный номер в виде phone_number@unknown.email (например, 79998887766@unknown.email ).
*	Если пользователь авторизуется, но нет ни почты, ни телефона, должно передаваться нечто вида id@unknown.email, где id – идентификатор пользователя в вашей системе.

Важно! Вы можете создать новые переменные, передающие имя и email пользователя, или использовать уже имеющиеся. Однако в переменных, используемых для Flocktory, не должно быть значений по умолчанию, отличных от пустых. Если уже существующие переменные имеют определенное значение по умолчанию, создать новые — необходимо.

Важно! Если вы не знаете имя или почту пользователя, не передавайте нам их.

<a id="category-id"></a>

## **Код идентификации категорий товаров**

На всех страницах товарного каталога вставьте следующий код

```html
<div class="i-flocktory" data-fl-action="track-category-view" data-fl-category-id="123"></div>
```

### В Google Tag Manager

```html
{% raw %}<div class="i-flocktory" data-fl-action="track-category-view" data-fl-category-id={{category_id}}></div>{% endraw %}
```

![GTM int](https://assets.flocktory.com/assets/help/catalogue_pages-9cf0327686020aa421e1630d783b5a96.png)

### Параметры

| Параметр      | HTML                | Переменная GTM  |         Значение                               |
|:-------------:|:-------------------:|:---------------:|:----------------------------------------------:|
| category → id | data-fl-category-id	| category_id     | ID категории, соответствующий ID категории из вашего YML- или GMF-файла|

Настройка страниц, на которых выполняется данный код, может производиться как по содержанию URL (например, URL содержит /catalog/, либо с использованием регулярного выражения), так и по переменной (в самом простом случае — по переменной из вашего Data Layer, например pagetype = catalog).

Для определения страниц, на которых будет запускаться данный тэг через переменные, необходимо создать триггер типа PageView в разделе Triggers.

#### На что обратить внимание:

* Просмотр категории передается на всех страницах категорий.
* Передаваемый id соответствует записи в YML файле.
* Должен передаваться один тег категории со страницы.
* Код нужно передавать только на страницах категорий, не на карточках товаров или других страницах.

<a id="product-id"></a>

## **Код идентификации товара**

На всех страницах товаров на вашем сайте вставьте следующий код:

```html
<div class="i-flocktory" data-fl-action="track-item-view" data-fl-item-id="123" data-fl-item-category-id="1" data-fl-item-vendor="Nike" data-fl-item-available="true"></div>
```

### В Google Tag Manager

```html
{% raw %}<div class="i-flocktory" data-fl-action="track-item-view" data-fl-item-id={{item_id}} data-fl-item-category-id={{item_category_id}} data-fl-item-vendor={{item_category_id}} data-fl-item-available={{item_availability}}></div>{% endraw %}
```

![GTM int](https://assets.flocktory.com/uploads/clients/1833/7b776de6-4d4b-46bb-9050-525413eb8a5d_Monosnap%202017-05-29%2020-33-28.png)

### Параметры

| Параметр  | HTML            | Переменная GTM  |         Значение                               |
|:---------:|:---------------:|:---------------:|:----------------------------------------------:|
| item → id | data-fl-item-id	| item_id         | ID товара, соответствующий ID товара из вашего YML- или GMF-файла |
| item → category → id | data-fl-item-сategory-id   | item_category_id | **опциональный параметр** ID категории товара|
| item → vendor | data-fl-item-vendor   | item_vendor         | **опциональный параметр** Название вендора товара|
| item → available | data-fl-item-available   | item_availability         | **опциональный параметр** Наличие товара (есть на складе (true или false))|



### На что обратить внимание:
* Информация передается только на соответствующих страницах - карточках товара.
* data-fl-item-id товара, соответствующий записи в YML файле
* id должен соответствовать данным, передаваемым при добавлении.
* Если вы решили не поддерживать описанные опциональные параметры, соответствующий атрибуты нужно опустить.
* Если опциональный параметр отсутствует для части продуктов, в этом случае нужно передавать пустую строку как значение соответствующего атрибута

<a id="cart-code"></a>

## **Код состояния корзины**

Чтобы модуль Pre-Checkout имел актуальную информацию о состоянии корзины, необходимо передавать нам данные о ее состоянии, когда оно меняется.

При добавлении товара в корзину необходимо вызывать следующий JavaScript-код:

```javascript
window.flocktory = window.flocktory || [];
window.flocktory.push(['addToCart', {
	item: {
		"id": "123", // product id
		"price": 1200, // product price
		"count": 1, // quantity of this product added
		"brand": "smth inc", // product brand name
		"categoryId": "123123" // product category id
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
Также существует метод, позволяющий просто передать все содержимое корзины, изменения мы вычислим сами

```javascript
window.flocktory = window.flocktory || [];
window.flocktory.push(['updateCart', {
  cart: {
    items:[
      {id:'1001', price: 100, count: 1},
      {id:'1011', price: 200, count: 1}
    ]
  }
}]);
```

### Параметры

| Параметр          |         Значение                                                  |
|:-----------------:|:-----------------------------------------------------------------:|
| item → id         | ID товара, соответствующий ID товара из вашего YML- или GMF-файла |
| item → price      | Цена добавленного товара                                          |
| item → count      | Количество добавленных единиц товара                              |
| item → brand      | Бренд добавленного товара                                          |
| item → categoryId | ID категории добавленного товара из вашего YML- или GMF-файла     |

### На что обратить внимание:
* Цена должна соответствовать цене на карточке товара;
* ID, передаваемый Flocktory, должен соответствовать id, передаваемому в теге просмотра карточки товара;
* ID товара должен обязательно соответствовать id в вашем YML- или GMF- файле, чтобы система смогла его определить.
* Должны передаваться следующие изменения количества товаров в корзине:
	* Изменения количества  (кнопки +/- ) на страницах товаров/товара и в Корзине.
	* Изменение количества товаров вручную (ввод числа в поле, Enter).
	* Удаление из корзины одного/нескольких наименований.
	* Добавление товара из всех дополнительных блоков (похожие товары и т.п.) должно также учитываться.
	* При наличии скидки (например, с определенной суммы), она также должна учитываться при передаче цены.
	* Если у товара имеются вариации (цвет, размер) с различными ценами,  необходимо передавать вариации товаров c различающимися id в соответствии с YML-файлом. Если нет YML, дописывать к id параметры, отвечающие за выбранные опции товаров.
	* При авторизации пользователя должно передаваться добавление товаров, которые уже имеются у него в Корзине.


### Как правильно передать эти изменения?
* необходимо передать нам соответствующее событие (addToCart, removeFromCart), а в поле count записать количество добавленного или удаленного товара
* При полном удалении наименования из корзины в поле count нужно указать, сколько единиц товара удалено.

<br/>

<a id="gtm"></a>

### В Google Tag Manager

#### Случай 1. Данные Enhanced Ecommerce передаются в ваш Data Layer

Если вы передаете [Enhanced Ecommerce](https://support.google.com/analytics/answer/6014841?hl=en) в ваш Data Layer в соответствии с [инструкцией](https://developers.google.com/tag-manager/enhanced-ecommerce#cart), вам необходимо установить тэги и создать все нужные переменные в соответствии с инструкцией ниже. Все необходимые данные при этом уже находятся в Data Layer.

##### **Добавление в корзину**

```html
<script type="text/javascript">
  window.flocktory = window.flocktory || [];
  window.flocktory.push(['addToCart', {
    item: {% raw %}{{ecommerce.add.products}}{% endraw %}
  }])
</script>
```

![Add to cart](https://assets.flocktory.com/assets/help/add_to_cart_with_data_layer-3f45b4d31ffafc4ed48136bd2d7500ef.png)

##### **Удаление из корзины**

```html
<script type="text/javascript">
  window.flocktory = window.flocktory || [];
  window.flocktory.push(['removeFromCart', {
    item: {% raw %}{{ecommerce.remove.products}}{% endraw %}
  }])
</script>
```
![Remove from cart](https://assets.flocktory.com/assets/help/remove_from_cart_with_data_layer-4c185ce517ada29a3288c333b487bb17.png)

{% raw %}Параметры {{ecommerce.add.products}} и {{ecommerce.remove.products}} соответствуют тому, каким образом необходимо настроить переменные в разделе Variables. На примере переменной {{ecommerce.add.products}}:{% endraw %}
![Params](https://assets.flocktory.com/assets/help/add_to_cart_with_data_layer-3f45b4d31ffafc4ed48136bd2d7500ef.png)

### Случай 2. Данные Enhanced Ecommerce НЕ передаются в ваш Data Layer

Хотя это и не обязательно, мы настоятельно рекомендуем настроить передачу данных Enhanced Ecommerce в Data Layer в соответствии с [этой инструкцией](https://developers.google.com/tag-manager/enhanced-ecommerce#cart), т.к. в противном случае необходимо добавить все перечисленные далее переменные вручную (используя один из методов, предлагаемых Google Tag Manager) и передавать их нам, используя следующие скрипты.

#### **Добавление в корзину**

```html
{% raw %}<script type="text/javascript">
  window.flocktory = window.flocktory || [];
  window.flocktory.push(['addToCart', {
   'products': [{
      'name': {{products.name}},
      'id': {{products.id}},
      'price': {{products.price}},
      'brand': {{products.brand}},
      'category': {{products.category}},
      'variant': {{products.variant}},
      'count': {{products.count}}
     }]
  }])
</script>{% endraw %}
```

![Addtocart case 2 example](https://assets.flocktory.com/assets/help/add_to_cart_no_ee-020f0e2fc94571c5ec81e6ff1797f2e8.png)

#### Параметры

|Параметр|Переменная GTM|Значение|Обязательный параметр|
|:-:|:-:|:-:|:-:|
|products → name|products.name|Название добавленного товара|
|products → id|products.id|ID товара, соответствующий ID товара из вашего YML- или GMF-файла|✓|
|products → price|products.price|Цена добавленного товара|✓|
|products → brand|products.brand|Бренд добавленного товара|
|products → category|products.category|Категория добавленного товара|
|products → variant|products.variant|Вариант добавленного товара (например, разные цвета одной модели)|
|products → count|products.count|Количество добавленных единиц товара|✓|


#### **Удаление из корзины**

```html
{% raw %}<script type="text/javascript">
  window.flocktory = window.flocktory || [];
  window.flocktory.push(['removeFromCart', {
   'products': [{
      'id': {{products.id}},
      'count': {{products.count}}
     }]
	}]);
</script>{% endraw %}
```

![Removefromcart case 2 example](https://assets.flocktory.com/assets/help/remove_from_cart_no_ee-353148420738ed6f504182b9311b0015.png)

#### Параметры

|Параметр|Переменная GTM|Значение|Обязательный параметр|
|:-:|:-:|:-:|:-:|
|products → id|products.id|ID товара, соответствующий ID товара из вашего YML- или GMF-файла|✓|
|products → count|products.count|Количество добавленных единиц товара|✓|

<br/>
<br/>

## Как проверить, что интеграция Pre-Checkout проведена корректно?

## **1. Код интеграции Flocktory**

Чтобы проверить эту часть интеграции, нужно пройтись по страницам сайта с открытой консолью. В ней нужно выбрать вкладку Network и в фильтре указать "ultimate". На каждой странице должна появляться запись следующего вида:

![Loader example](https://assets.flocktory.com/uploads/clients/2106/2e3b2697-3806-4a5b-af97-310bff148c24_precheck_loader.jpg)

### На что обратить внимание:
* Status code 200: OK
* Site: 1234 – должен соответствовать ssid вашего сайта в личном кабинете (можно уточнить у Вашего менеджера)


## **2. Код идентификации авторизованных пользователей**

Чтобы проверить эту часть интеграции, нужно пройтись по страницам сайта с открытой консолью. В ней нужно выбрать вкладку Network и в фильтре указать "ultimate". На каждой странице должна появляться запись следующего вида:

### На что обратить внимание:
* Status code 200: OK
* В пункте Query String Parameters передается email и имя пользователя

![Auth example](https://assets.flocktory.com/uploads/clients/2106/9c316acd-89a2-4ce1-ba0a-d7c12d393e1a_pre_howtocheck2.jpg)

* Если имя передается, оно должно передаваться в порядке «Имя Фамилия»
* Если имя не передается или неизвестно, запись "name" должна отсутствовать
* Если пользователь авторизуется, но его email неизвестен, в поле email должен передаваться номер телефона следующим образом: 79998887766@unknown.email
* Если пользователь авторизуется, но нет ни почты, ни телефона, должно передаваться нечто вида id@unknown.email, где id – идентификатор пользователя в вашей системе
* Для неавторизованного пользователя записей "email" или "name" быть не должно


## **3. Код идентификации категорий товаров**

В данном пункте нужно пройтись по страницам категорий товаров, аналогичным образом фильтруя во вкладке network по слову "ultimate". На этот раз нужно искать записи, содержащие в Request Payload action: "customer.category_visit".

![CategoryId example](https://assets.flocktory.com/uploads/clients/2106/b7d8ee5c-2a50-4ab0-85e4-5cd572993cf9_pre_howtocheck3.jpg)

### На что обратить внимание:
* Status code: 200 OK
* Просмотр категории передается на всех страницах категорий
* Передаваемый id соответствует записи в YML файле
* Просмотр категории не передается на других страницах


## **4. Код идентификации товара**

Здесь необходимо пройтись по карточкам товаров, фильтруя во вкладке network по слову "ultimate". Нужно искать записи, содержащие в Request Payload action: "customer.item_visit".

![ItemId example](https://assets.flocktory.com/uploads/clients/2106/417b8759-fe1d-4844-9eb3-da064dae99e1_pre_howtocheck4.jpg)

### На что обратить внимание:
* Status code: 200 OK
* Yandex_offer – id товара, соответствующий записи в YML файле
* Информация передается только на соответствующих страницах


## **5. Код состояния корзины**
Чтобы проверить эту часть интеграции, необходимо проверить все возможные варианты добавления / удаления товара / изменения его количества в корзине в любую сторону.

#### Какие условия должны выполняться:
* При каждом изменении количества товара в корзине в большую сторону во вкладке network должна появляться новая запись ultimate, содержащая в request payload корректный id и количество добавленных единиц товара. ID (yandex_offer) также должен соответствовать записи в YML.

![Cart1 example](https://assets.flocktory.com/uploads/clients/2106/e9a705ea-87c4-4c78-8cbb-5da231c44269_pre_howtocheck5.jpg)

* В любой момент времени Flocktory имеет корректную информацию о состоянии корзины.
Как это проверить:
Открыть вкладку Console и воспользоваться следующими командами:

```html
flocktory.getData().cart.items
```

возвращает список товаров в корзине на основе переданной информации.

![Cart2 example](https://assets.flocktory.com/uploads/clients/2106/bd6435c9-4d30-46f5-864c-34fab8ae2028_pre_howtocheck6.jpg)

```html
flocktory.getData().cart.getTotalPrice()
```
возвращает сумму всех товаров в корзине 

![Cart3 example](https://assets.flocktory.com/uploads/clients/2106/b379bccc-6a28-4ed4-9437-5e34185f9592_pre_howtocheck7.jpg)

```html
<script type="text/javascript">
  window.flocktory = { push: function(){ alert(JSON.stringify(arguments[0])); } }
</script>
```

В случае, если информация не соответствует состоянию корзины, для поиска момента, когда что-то не передается или передается неверно, можно переобъявить объект flocktory.
Например, так

![Cart4 example](https://assets.flocktory.com/uploads/clients/2106/8906f75e-5731-4164-8d9a-c6b84bcedae3_pre_howtocheck8.jpg)
