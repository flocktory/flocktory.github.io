---
layout: doc
title:  "Precheckout"
section: integration
---

**Attention!** Before the integration of Precheckout you have to perform [main integration]({{ site.baseurl }}{% link _docs_en/integration/general.md %}) and [Post-Checkout integration]({{ site.baseurl }}{% link _docs_en/integration/postcheckout.md %}).

The main advantage of Pre-Checkout is the fact, that it launches its actions depending on complex signals caused by the user's behavior on the site. Therefore it is very important that the data is transferred correctly.

## Launch of basic actions integrated in the Pre-Checkout module

The basic set of Pre-Checkout mechanics already includes a large array of signals and parameters that will be used to trigger different widgets.

The basic functionality includes the following parameters (triggers): has the user been to the site before; number of visited pages; time spent on the site; time of user inactivity; login status on your site; demographic characteristics; user's purchase history; number of articles in the basket; total amount of the basket; information about the origin of the user (either referrer or utm).

To launch campaigns based on these trigger you have to:

Enter the link to your updated YML or GMF-file (or send this info to your account manager);
Integrate the Flocktory code according to the instruction below - direct integration of integration via Google Tag Manager is possible

For this module to wirk properly you should as well perform [main]({{ site.baseurl }}{% link _docs_ru/integration/general.md %}) and [postcheckout]({{ site.baseurl }}{% link _docs_ru/integration/postcheckout.md %}) integrations.

<a id="auth-user"></a>

## **ID to identify the logged-in user**

Place this code on all your pages:

```html
<div class="i-flocktory" data-fl-user-name="John Doe" data-fl-user-email="johndoe@gmail.com"></div>
```

### in Google Tag Manager

```html
{% raw %}<div class="i-flocktory" data-fl-user-name={{user_name}} data-fl-user-email={{user_email}}></div>{% endraw %}
```

![GTM int](https://assets.flocktory.com/assets/help/user_auth-fc36e6c6cc4290cb89e9c5b86730fc31.png)

### Parameters

| Parameter    | HTML               | GTM variable    |         Value                                  |
|:------------:|:------------------:|:---------------:|:----------------------------------------------:|
| user → name  | data-fl-user-name	| user_name       | Full user name e.g. "John Doe" 								 |
| user → email | data-fl-user-email	| user_email      | User email e.g. "johndoe@gmail.com"            |

<br>

#### Please mind:

* For unauthorized users the tag should not be present.
* For authorized users:
    * The code should be added not only immediately after user logs in, but on all visited pages.
    * Both email and name should be passed if known.

* If user name is missing the corresponding attribute should be omitted.
* If email is missing, you should replace it with phone_number@unknown.email (for example 79998887766@unknown.email).
*	If none of an authorized user's email or phone number is known, pass id@unknown.email, where id is the id of the user in your system.

Important ! You can create new parameters to transfer name and email address of the user or use the existing ones. However the parameter for Flocktory should not be the default values and not be empty. If existing parameters already have default values then you need to create new ones.


<a id="category-id"></a>

## **Category ID**

On all category pages place the following code:

```html
<div class="i-flocktory" data-fl-action="track-category-view" data-fl-category-id="123"></div>
```

### in Google Tag Manager

```html
{% raw %}<div class="i-flocktory" data-fl-action="track-category-view" data-fl-category-id={{category_id}}></div>{% endraw %}
```

![GTM int](https://assets.flocktory.com/assets/help/catalogue_pages-9cf0327686020aa421e1630d783b5a96.png)

### Parameters

| Parameter     | HTML                | GTM variable    |         Value                                  |
|:-:|:-:|:-:|:-:|
| category → id | data-fl-category-id	| category_id | category ID that corresponds to the category ID used in your YML or GMF file |

<br>

This code can identify your cagtegory either by the structure of the URL (i.e. if it contains /catalog/) or it can receive a parameter (it could be from your Data Layer - pagetype=catalog).

To determine the pages on which the given tag will by launched by variables, you need to create a trigger of the type 'PageView' in the section 'Triggers'.

#### Please mind:

* This tag should be present on every category pages.
* The passed ID should correspond to your product feed.
* There may be only one such tag on a page.
* The code shoudn't be placed on product pages.

<a id="product-id"></a>

## **Product ID**

On all product pages place the following code:

```html
<div class="i-flocktory" data-fl-action="track-item-view" data-fl-item-id="123" data-fl-item-category-id="1" data-fl-item-vendor="Nike" data-fl-item-available="true"></div>
```

### in Google Tag Manager

```html
{% raw %}<div class="i-flocktory" data-fl-action="track-item-view" data-fl-item-id={{item_id}} data-fl-item-category-id={{item_category_id}} data-fl-item-vendor={{item_category_id}} data-fl-item-available={{item_availability}}></div>{% endraw %}
```

![GTM int](https://assets.flocktory.com/uploads/clients/1833/7b776de6-4d4b-46bb-9050-525413eb8a5d_Monosnap%202017-05-29%2020-33-28.png)

### Parameters

| Parameter | HTML            | GTM variable    |         Value                                  |
|:---------:|:---------------:|:---------------:|:----------------------------------------------:|
| item → id | data-fl-item-id	| item_id         | category ID that corresponds to the category ID used in your YML or GMF file |
| item → category → id | data-fl-item-сategory-id   | item_category_id | **optional** product category id|
| item → vendor | data-fl-item-vendor   | item_vendor         | **optional** product vendor name|
| item → available | data-fl-item-available   | item_availability         | **optional** Product availability|

<br>

### Please mind:
* This tag shoud be present only on product pages
* ID should correspond to your product feed and the IDs passed in the cart code (see below) and Postcheckout.
* If you decide not to implement optional parameters, don't add the corresponding attributes to the tag
* If an optional parameter is absent for some part of products, the patameter's value should be an empty string in these cases

<a id="cart-code"></a>

## **Cart code**

In order for the Pre-Checkout Module to know the correct contents of the shopping basket you need to transfer its contents upon each change.

For each added item to the basket you need to call this JavaScript-code:

```javascript
window.flocktory = window.flocktory || [];
window.flocktory.push(['addToCart', {
	item: {
		"id": "123", // product id
		"price": 1200, // product price
		"count": 1 // quantity of this product added
	}
}]);
```
And for each deleted item you need to call the following:

```javascript
window.flocktory = window.flocktory || [];
window.flocktory.push(['removeFromCart', {
	item: {
		id: '124', // product id
		count: 2 // quantity of this product removed
	}
}]);
```

### Parameters

| Parameter    |         Value                                                     |
|:------------:|:-----------------------------------------------------------------:|
| item → id    | product ID that corresponds to the product ID in your YML or GMF file |
| item → price | Price of added product                                            |
| item → count | Count of added products                                           |


<br>

### Please mind:

* The price passed should correspond to the price stated on the page;
*  It is very important that the product id corresponds 100% to the id in your GMF/YML file in order for the system to work correctly.
* Make sure that all the changes in basket state are passed, including:
	* Quantity changes (buttons +/- ) made on product/basket page.
	* Manual quantity changes (type-in, Enter).
	* Product(s) removal.
	* All basket additions done through supplementary elements ("You may also like", "Recommendations" etc.) should also be considered.
	* If a product has variations (different colors, sizes) with different prices, you should pass different IDs correspondingly.
	* When a user is being authorized add all the products that have preserved from previous session.

<br>

<a id="gtm"></a>

### in Google Tag Manager

#### Case 1. You have Enhanced Ecommerce data in your Data Layer

If you have implemented [Enhanced Ecommerce](https://support.google.com/analytics/answer/6014841?hl=en) Data Layer integration in accordance with [this instruction](https://developers.google.com/tag-manager/enhanced-ecommerce#cart), you mey set up tags and create all required variables as described below. All the data required is already in the Data Layer.

##### **Add to cart**

```html
<script type="text/javascript">
  window.flocktory = window.flocktory || [];
  window.flocktory.push(['addToCart', {
    item: {% raw %}{{ecommerce.add.products}}{% endraw %}
  }])
</script>
```

![Add to cart](https://assets.flocktory.com/assets/help/add_to_cart_with_data_layer-3f45b4d31ffafc4ed48136bd2d7500ef.png)

##### **Remove from cart**

```html
<script type="text/javascript">
  window.flocktory = window.flocktory || [];
  window.flocktory.push(['removeFromCart', {
    item: {% raw %}{{ecommerce.remove.products}}{% endraw %}
  }])
</script>
```
![Remove from cart](https://assets.flocktory.com/assets/help/remove_from_cart_with_data_layer-4c185ce517ada29a3288c333b487bb17.png)

{% raw %}Parameters {{ecommerce.add.products}} and {{ecommerce.remove.products}}  correspond to how you want to configure the variables in the section Variables. For example, the variable {{ecommerce.add.products}}:{% endraw %}
![Params](https://assets.flocktory.com/assets/help/add_to_cart_with_data_layer-3f45b4d31ffafc4ed48136bd2d7500ef.png)


Rules for running the script are configured for the events 'addToCart' and 'removeFromCart'. Example for the event 'addToCart':

![Addtocart example](https://assets.flocktory.com/assets/help/add_to_cart_trigger-e72accacf69f1a08594f79bc6e7131be.png)

### Case 2: Enhanced Ecommerce Data is NOT transferred to your data layer

Although not mandatory, we strongly recommend that you configure the transfer of Enhanced Ecommerce data into the Data Layer, in accordance with [this instruction](https://developers.google.com/tag-manager/enhanced-ecommerce#cart) , Otherwise, you must add all of the following variables manually (using one of the methods offered by Google Tag Manager) and send them to us using the following scripts.

#### **Add to cart**
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

#### Parameters

|Parameter|GTM variable|Value|Obligatory parameter|
|:-:|:-:|:-:|:-:|
|products → name|products.name|Name of the added product|
|products → id|products.id|product ID that corresponds to the product ID in your YML or GMF file|✓|
|products → price|products.price|Price of added product|✓|
|products → brand|products.brand|Brand of the added product|
|products → category|products.category|Category of the added product|
|products → variant|products.variant|Variant of the added product (for example for different colors of one model)|
|products → count|products.count|Count of added products|✓|

<br>

#### **Remove from cart**
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

#### Parameters

|Parameter|GTM variable|Value|Obligatory parameter|
|:-:|:-:|:-:|:-:|
|products → id|products.id|product ID that corresponds to the product ID in your YML or GMF file|✓|
|products → count|products.count|Count of added products|✓|

<br>
This concludes the integration of the basic Pre-Checkout functionality.
