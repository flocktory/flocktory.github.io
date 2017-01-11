---
layout: doc
title:  "Postcheckout"
section: integration
---
**Attention!** Before you can use the Flocktory platform for your website you need to perform the  [main integration]({{ site.baseurl }}{% link _docs_ru/integration/general.md %})

You will need to insert this code on the “thank you” page of your website – the page that confirms the successful order of your shop.

Important: If you have several different pages after a successful transaction --> please insert this code on all your “thank you” pages. This integration code needs to be set up dynamically with the help of your programmers (for example with the use of Java/Ruby/PHP/Perl on the server side).

```html
<script type="text/javascript">
    window.flocktory = window.flocktory || [];
    window.flocktory.push(['postcheckout', {
        user: {
            name: 'Ivan Ivanov',
            email: 'ivan@gmail.com',
            sex: 'm',
        },
        order: {
            id: '77',
            price: 2000,
            custom_field: 'my_custom_id',
            items: [{
                id: '777',
                title: 'Nike Shoes',
                price: 1000,
                image: 'http://path.to.image',
                count: 1
            }]
        }
    }]);
</script>
```

## Variables in the integration code
The integration code includes obligatory and optional variables. In order for our products and algorithms to work the obligatory variables are sufficient. However, we encourage you to integrate a maximum of variables from the below list in order to allow you to use our targeting and recommendation algorithms to their fullest potential.

### Obligatory parameters

#### user → name
Complete name of the user. For example: John Mayer. Try to pass us the precise name of the user as it affects personalization possibilities and hence the conversion.

#### user → email
The user's email address	A unique identification of the user. If for some reason you do not know the user’s email but know his phone number you can just pass the data in the following format `phone_number@unknown.email`

If you do not know anything about the user then you can pass this variable in the following format `id@unknown.email` where id is the identification of the client in your system (i.e. customer ID).

#### order → id
Oder ID. Must be unique for each order that is transferred to us.

#### order → price
Total order amount. Example: 2999,00 or 1000.00	Only value without currency. Without “spaces”. Use can use a dot or a comma as decimals separator.

#### order → items
The basket of products at the moment of purchase	Based on this variable our algorithms will allow you targeting of your campaigns, segmentation and recommendations. Please be sure to correctly pass on all elements of this variable.

The products are passed as a JavaScript Array.


### Optional parameters
#### user → sex
User's gender.	Possible values: m or f (for male or female). If you do not know the gender do not transfer this variable.

#### order → custom_field
Additional identification of the order	At your option. If you don’t need to pass an additional variable then do not transfer this variable.

#### order → items → id
Article number of the product.	On the basis of the article ID we will identify the article in your shop.

#### order → items → price
Price of the product	Transfer the real price of the article. This will allow you to use full segmentation options and will help our algorithms for upselling tasks.

#### order → items → title
Product Name.

#### order → items → image
URL of the product picture.

#### order → items → count
Amount of a certain product in the basket

#### spot
Internal filter for campaigns.
Optional - Spot is a special ID that will allow you to show only a specific campaign of the PostCheckout module. Use this only if you have different "thank you" pages and want to show specific different campaigns on them. Ask you account manager about details.

## Please mind

*	This code should be placed on every thank you page your site has, e.g. different payment options thank you pages / quick order
*	All the obligatory parameters should be passed

* In case of unknown user name please leave the corresponding field blank or omit it.
* In case if unknown email address:
    * when phone number is known, please pass is like this `phone_number@unknown.email` (e.g. 79998887766@unknown.email ).
    * when neither of the two is available please pass user_id + @unknown.email, wheere user_id – is the user's ID in your system e.g. 12345678@unknown.email).
* Regarding rest order data:
    * Product IDs must correspond to your YML/GMF feed.
    * Product names must be human-readable.
    * Quotes inside product names should be mirrored in order not to break the string.



# Integration via GTM

1) In the vatiables section "Variables" create all the variables you are going to need further. Mostly they are just referring to some data on the page, e.g. global JS variabels, DataLayer variables etc. You also may user a simple JS function in order to retrieve the needed data. It depends on the site structure and the way data is stored. Here you can see an example showing a global GTM variable.

![Global variable](https://assets.flocktory.com/uploads/clients/1791/303cacce-fdb8-49d2-86df-ee5e02992786_image14.png)

The set of variables may be different for your site.

![Variables section](https://assets.flocktory.com/uploads/clients/1791/d826d6e3-19b3-4fc6-9370-9ca69b5b1a6b_image15.png)

2) Create a trigger, firing on the TY-page. You may use URL, custom (datalayer) event, global variable value etc. as triggers.

![Trigger example](https://assets.flocktory.com/uploads/clients/1791/a4b66254-91c9-4af7-96b7-99eb3cf823b9_image16.png)

3) In the "Tags" sectino create a tag with the traching code given above. Substiture the variables so that the structure and data types are corresponding. You may as well use variables to set `spot` and `custom_field` in case you need them

![Ready tag](https://assets.flocktory.com/uploads/clients/1791/84ff1cf0-9895-4a60-bb9d-135ce9c2fa83_image17.png)
