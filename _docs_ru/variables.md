---
layout: doc
title:  "Placeholders for html templates"
section: general
---

TODO translate to russian

In the html-templates for emails & landings you can use special variables for showing certain content information that is based on the user or the campaign.
The list of variables is available above the html editor field in the cabinet and you can see examples of the variable contents when hovering your cursor above the variable name.
Example, where to find the variables in a Postcheckout campaign email:

![Vars](https://assets.flocktory.com/uploads/clients/1791/6d9bea36-5a46-4f0c-a173-0a5a9713e0eb_49et7.png)

_Inside the html code_ the variable can be inserted by using the following syntax:
```
{{ variableName }}
```

Here you see the list of available variables for the different products.

## Partner Program

### Xmail

* {{ customer_name }} - Customer name
* {{ partner_title }} - Name of the traffic partner
* {{ partner_logo }} - Logo of the traffic partner
* {{ site_title }} - Name of the client web shop
* {{ site_logo }} - Logo of the client web shop
* {{ motivation_value }} - Motivation
* {{ motivation_reward_url }} - Link to the website / with coupon

### Xpush (Feature)

* {{ site_title | escape_quotes }} - Name of the client web shop, making sure that quote signs are shown correctly


## Exchange

### Texts for Emails (in the HTML Editor interface only)
* {{ publisher_name }} - User name
* {{ site_name }} - Site name (name equals account name in our cabinet)
* {{ valid_until }} - Date until the offer expires
* {{ site_name_url }} - Site name in URL style (i.e. lamoda.ru)
* {{ friend_reward }} - Motivation


**IMPORTANT**
For using these variables in the **Email subject line OR in the preview window** use %varName% instead of the {{ varName }} syntax ! (i.e. %publisher_name%).

### Banners
* {{ user_name }} - User name


## Postcheckout

### Texts for Social Posts (for use in cabinet interface - different syntax!)

* %short_url% - Unique link to the offer
* %publisher_name% - Name of the initial user (publisher)
* %valid_until% - Offer validity date
* %left_until% - Hours left until the end of the offer
* %maximum_publisher_reward% - Мax. possible reward for the user (in case of limitation)
* %publisher_reward% - Reward for the publisher / initial user
* %friend_reward% - Motivation for the friend
* %site_name%  - Name of the web shop
* %customer_service_contact% - Email for customer support (from settings!)
* %customer_email% - Email of the user
* %order_number% - Order number

### Texts for widgets
* {{ publisher_name }} - Name of the publisher
* {{ publisher_reward }} - Reward for the publisher
* {{ friend_reward }} - Motivation for the friend

### Texts for Emails (in the HTML Editor interface only)
* {{ publisher_name }} - Name of the publisher
* {{ site_name_url }} - Site name in URL style (i.e. lamoda.ru)
* {{ site_name }} - Site name (cabinet style)
* {{ publisher_reward }} - Reward for the publisher
* {{ friend_reward }} - Motivation for the friend
* {{ valid_until }} - Date of validity

**IMPORTANT**
For using these variables in the **Email subject line OR in the preview window** use `%varName%` instead of the `{{ varName }}` syntax ! (i.e. `%publisher_name%`)

## Push

* {{customer_name}}  - User name
* {{motivation_value}} - Motivation (i.e.: “10 EUR”, “Free Delivery”, "Present from our partner")

## Workflow

### WF-Push
* {{customer_name}}  - User name
* {{motivation_value}} - Motivation (i.e.: “10 EUR”, “Free Delivery”, "Present from our partner")
* {{ best_cart_item['title'] | default: 'Hello' }} - for abandoned baskets - shows the name of the most expensive item from the basket. If this item cannot be found in the product feed (YML / GMF) then the default value will be shown (i.e. "Hello")

### WF-Email

* {{ customer_name }} - User name
* {{ motivation_reward_url }} - Lind to the motivation
* {{ motivation_value }} - Motivation (i.e.: “10 EUR”, “Free Delivery”, "Present from our partner")
* {{ site_title }} - Name of the site (cabinet style)
* {{ unsubscribe_url }} - Unsubscribe Link
