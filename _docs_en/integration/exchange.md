---
layout: doc
title:  "Exchange"
section: integration
---

Attention! Before the integration of Exchange you have to perform [main integration]({{ site.baseurl }}{% link _docs_en/integration/general.md %}) and [Post-Checkout integration]({{ site.baseurl }}{% link _docs_en/integration/postcheckout.md %}).

Exchange can be integrated with just one additional step.

On all pages where you want to display banners to access Flocktory Exchange you have to integrate this HTML code:

```html
<div class="i-flocktory" data-fl-action="exchange" data-fl-spot="some_spot" data-fl-user-name="Ivan Ivanov" data-fl-user-email="ivan@gmail.com"></div>
```

After integrating this code and having installed the general integration code you are ready to enable Exchange. You can add this code easily yourself by using [Google Tag Manager](http://www.google.com/tagmanager/) or by asking your front-end developer to add this line of code to your thank you page.

**Please be aware that the Exchange banner will be shown exactly in the place of the site where you will put the code. Please consult with your account manager for an efficient placement.**

## Parameters
The shown HTML code sends the following parameters.

### data-fl-user-name
Full name of the user. Ex.: `John Smith`

### data-fl-user-email
User's email address

### data-fl-spot
Spot - this is the id of the banner you want to show on this page. If you want to show the same banner on all different thank you pages then you do not have to transmit this paramenter. The spot of the banners can be assigned in your Flocktory account.

## Create an Exchange banner
Documentation on creating exchange banners can be found [here]({{ site.baseurl }}{% link _docs_en/exchange/banner.md %})


## Important:
* [Main Flocktory integration] is mandatory ({{ site.baseurl }}{% link _docs_en/integration/general.md %})
* In case e-mail is unknown please pass us phone number like this 79998887766@unknown.email
* In case neither email nor phone number are known pls use xname@flocktory.com


# Integration via GTM

Create a new tag in the rags section and put the following code there:

```html
<div class="i-flocktory" data-fl-action="exchange" data-fl-spot="some_spot" data-fl-user-name="Ivan Ivanov" data-fl-user-email="ivan@gmail.com"></div>
```

Substitute attribute values with the variables used in Postcheckut ( user_name and user_email ).

![](https://assets.flocktory.com/uploads/clients/1791/bad05265-b502-4c42-beae-45271f6f9dfd_image18.png)

To this tag attach a trigger that fires when user is on the page where the banner should be shown.

In case you are willing to use different banners on different pages, you may use the spot parameter for the system to know which one to show.
