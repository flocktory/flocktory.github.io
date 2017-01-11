---
layout: doc
title:  "Xmail"
section: integration
---


## 1. Main integration code

Install the following code on ALL of the pages of your website. It should be placed either in the `<head>` section or in the end of the `<body>` section.

```html
<script type="text/javascript" src="https://api.flocktory.com/v2/loader.js?site_id=YOUR_SITE_ID" async="async"></script>
```

### Parameters

Parameter | Value
---------:|:---------
SITE_ID   | Your Flocktory Site ID.

This code is common for all Flocktory products and you only have to put it once to your system, no matter how many products you want to activate.


## 2. ID to identify the logged-in user

Place this code on every page of your site.

```html
<div class="i-flocktory" data-fl-user-name="Ivan Petrov" data-fl-user-email="ivan@petrov.ru"></div>
```

### Parameters

| Parameter     | Value             |
|:------------:|:------------------:|
| data-fl-user-name  | User name e.g. "John Doe"	|
| data-fl-user-email | User email e.g. "johndoe@gmail.com"	|


* This tag should be present on all the pages for an authorized user
* In case user name is missing, omit the corresponding attribute
* For users you don't know, leave attribute values empty ("") or don't add the tag to the page.


## 3. Provide Flocktory with the requisites needed to connect to your SMTP server
Such as:

* SMTP host
* Port
* Username
* Password
