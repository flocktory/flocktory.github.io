---
layout: doc
title:  "Mobile Push Integration"
section: push
---

## 1. Flocktory Mobile Push integration prerequisits

* Using any SDK makes the mobile application heavier, and as a result, developers tend to limit the number of SDKs used;

* to start work with the functionality of Flocktory Mobile Push it is enough to write 5-7 HTTP requests, which is not a problem for mobile developers

Given the above factors, we decided to abandon the implementation of the SDK in favor of providing an API that will allow you to implement all the functionality and without adding dependencies to the project.


## 2. Requirements:

* with each request to the Flocktory API, you must pass a unique application identifier (you mau know it as "device-uuid"). The identifier should be unique and should not change after the application is restarted. This identifier is passed in the "uuid" variable;

* If user authorizes while using the app, you should pass the email used to Flocktory.



## 3. API Description

API functionality consists of

* Passing events and data to Flocktory
* Receiving and processing push notifications

###  3.1 Passing events and data to Flocktory

Here you can see the required curl requests examples.
* The site/site-id/site_id should contain the id of your site in Flocktory system.
* The url parameters should contain a deep-link which will land the user to this very page/screen
* the body parameter should be urlencoded (which is not the case in the examples for the sake of human readability)

1. User makes an order
```curl
curl 'https://api.flocktory.com/1/postcheckout/offer.js?uuid=123&body={"site_id":1833,"jsapi_version":"2.0","i":"5805265","e":"johnny.appleseed@gmail.com","n":"Johnny Appleseed","p":16790,"t":{"0":{"i":7752795,"t":"Nokia Lumia 800","u":"https://assets.flocktory.com/uploads/clients/1063/5bb944e2-70b8-4912-bc8f-ee43e345be4f_lumia.jpg","c":1,"p":16790}}}&callback=flock_jsonp' -H 'pragma: no-cache' -H 'accept-encoding: gzip, deflate, sdch, br' -H 'accept-language: ru-RU,ru;q=0.8,en-US;q=0.6,en;q=0.4' -H 'accept: */*' -H 'cache-control: no-cache' --compressed -g
```
* site_id - Flocktory ID of your site
* i - order ID
* e - user's email
* n - user's name
* p - order price
* t - order products array structured like this {"0":{},"1":{} ... }
* t-*-i - product ID
* t-*-t - product title
* t-*-u - link to the product's image (optional)
* t-*-с - amount of the product ordered
* t-*-p - price of the product

2. Product view event
```curl
curl 'https://api.flocktory.com/underworld/tracks/ultimate.js?uuid=123&body={"data":{"action":"customer.item_visit","links":{"yandex_offer":"1","site":1833},"payload":{"url":"http://spreadreward.com/"}}}' -H 'pragma: no-cache' -H 'accept-encoding: gzip, deflate, sdch, br' -H 'accept-language: ru-RU,ru;q=0.8,en-US;q=0.6,en;q=0.4' -H 'accept: */*' -H 'cache-control: no-cache'  --compressed -g
```
* links.yandex_offer - product ID
* links.site - Flocktory ID of your site
* payload.url - product deep-link

3. Category view
```curl
curl 'https://api.flocktory.com/underworld/tracks/ultimate.js?uuid=123&body={"data":{"action":"customer.category_visit","links":{"yandex_category":"1","site":1833},"payload":{"url":"http://spreadreward.com/"}}}' -H 'pragma: no-cache' -H 'accept-encoding: gzip, deflate, sdch, br' -H 'accept-language: ru-RU,ru;q=0.8,en-US;q=0.6,en;q=0.4' -H 'accept: */*' -H 'cache-control: no-cache'  --compressed -g
```
* links.yandex_category - category ID
* links.site - Flocktory ID of your site
* payload.url - category deep-link

4. Page view
```curl
curl 'https://api.flocktory.com/underworld/tracks/ultimate.js?uuid=123&body={"data":{"action":"session.page_visit","payload":{"resolution":"1366x741","ga":{"utmcsr":"","utmccn":"","utmcmd":"","h_utmcsr":"","h_utmccn":"","h_utmcmd":""},"url":"http://spreadreward.com/"},"links":{"site":1833}}}' -H 'pragma: no-cache' -H 'accept-encoding: gzip, deflate, sdch, br' -H 'accept-language: ru-RU,ru;q=0.8,en-US;q=0.6,en;q=0.4' -H 'accept: */*' -H 'cache-control: no-cache' --compressed -g
```
* payload.resolution - device screen resolution
* payload.ga - leave these values blank, as in the example
* payload.url - deep-link to the page being viewed
* links.site - Flocktory ID of your site

5. User adds a product to cart
```curl
curl 'https://api.flocktory.com/underworld/tracks/ultimate.js?uuid=123&body={"data":{"action":"customer.add_to_cart","links":{"yandex_offer":"7345265","site":1833},"payload":{"count":1,"custom_data":{"id":"7345265","price":832,"count":1},"url":"http://1833.demoshop.flocktory.com/"}}}' -H 'pragma: no-cache' -H 'accept-encoding: gzip, deflate, sdch, br' -H 'accept-language: ru-RU,ru;q=0.8,en-US;q=0.6,en;q=0.4' -H 'accept: */*' -H 'cache-control: no-cache' --compressed -g
```
* links.yandex_offer - product ID
* links.site - Flocktory ID of your site
* payload.count - the amont of product being added
* payload.price - price of the product

6. User removes a product from cart
```curl
curl 'https://api.flocktory.com/underworld/tracks/ultimate.js?uuid=123&body={"data":{"action":"customer.remove_from_cart","links":{"yandex_offer":"7345265","site":1833},"payload":{"count":1,"custom_data":{"id":"7345265","count":1},"url":"http://1833.demoshop.flocktory.com/"}}}' -H 'pragma: no-cache' -H 'accept-encoding: gzip, deflate, sdch, br' -H 'accept-language: ru-RU,ru;q=0.8,en-US;q=0.6,en;q=0.4' -H 'accept: */*' -H 'cache-control: no-cache' --compressed -g
```
* links.yandex_offer - product ID
* links.site - Flocktory ID of your site
* payload.count - the amont of product being removed
* payload.price - price of the product

7. User leaves an email<br>
Use this request when user authorizes in the app, subscribes to a mailing list and any other case when they leaves and email.
```curl
curl 'https://api.flocktory.com/u_shaman/setup-api.js?body={"siteId":"1833","uuid":"123","profile":{"email":"asd@asd.ru","name":"johnny"}}&callback=flock_jsonp_1' -H 'pragma: no-cache' -H 'accept-encoding: gzip, deflate, sdch, br' -H 'accept-language: ru-RU,ru;q=0.8,en-US;q=0.6,en;q=0.4'  --compressed -g
```
* siteId - Flocktory ID of your site
* profile.email - user's email
* profile.name - user's name

8. User's subscription has been acquired
* replace ... with the [token received](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId.html#getToken(java.lang.String, java.lang.String))
* in the os field pass either "android" or "ios" depending on the device's OS
```curl
curl 'https://api.flocktory.com/u_flockman/attach-push-to-session.js?uuid=123&body={"from-mobile-app":true,"platform":"firebase","os":"android","site-id":"1833","token":"https://android.googleapis.com/gcm/send/..."}&callback=flock_jsonp' -H 'accept-encoding: gzip, deflate, sdch, br' -H 'accept-language: en-US,en;q=0.8' -H 'accept: */*'  --compressed -g
```
In case you decided to use a separate project for app pushes, please also pass the corresponding gcm-sender-id in the request like this
```curl
curl 'https://api.flocktory.com/u_flockman/attach-push-to-session.js?uuid=123&body={"from-mobile-app":true,"platform":"firebase","os":"android","site-id":"1833","token":"https://android.googleapis.com/gcm/send/...","provider-meta":{"gcm-sender-id":"321"}}&callback=flock_jsonp' -H 'accept-encoding: gzip, deflate, sdch, br' -H 'accept-language: en-US,en;q=0.8' -H 'accept: */*'  --compressed -g
```

### 3.2 Push supscription and notifications processing.

There are two opitons:

* Option 1. You do not collect push subscriptions yet;

* Option 2. You have already collected some subscriptions via an external service;

_Option 1. You do not collect push subscriptions yet._

In this case we recommend using [firebase.google.com](https://firebase.google.com)

Most projects already have firebase dependencies in one way or another.
Installing this SDK allows you to work with all the standard features.

* If you already work Flocktory Web Push, the project is already created, and you don't have to create another one

* If you already have a Firevase project, it is necessary to inform your Flocktory account manager on order to agree on the further steps

* Push collection functionality (subscription, resubscription, unsubscription) is on your app dev team. Flocktory only provides a way of passing the subscription event and data (request 8)

* Deep-link functionality is accessible out of the box. We will only need yout dev team to provide us with the structure of your links.


_Option 2. You have already collected some subscriptions via an external service._

In case of correctly implemented push subscription functionality, the work process is as follows:

Flocktory sends notification to GCM/FCM/APN unchanged. To make sure everything works fine we should consult with your dev team and conduct a series of tests

If everything works fine you should implemeny calling method No. 8 to send subscription information to Flocktory.

GCM specifications:

* subscribing users: [android](https://developers.google.com/cloud-messaging/android/client) , [ios](https://developers.google.com/cloud-messaging/ios/client)

* [receiving notifications](https://developers.google.com/cloud-messaging/downstream)


##### **Standard data set, passed by Flocktory in a push message**
* landingUrl - link for a user to land on click
* messageTitle - message title, upper push notification part text
* body - general push message text
* iconUrl - icon url

Please mind that landing and icon urls may perform one or more redirects.

In case you are using the firebase library to deal with push notifications,  the data mey be obtained as follows ([google documentation](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/RemoteMessage)):
```java
public void onMessageReceived(RemoteMessage remoteMessage){
  Map<String, String> data = remoteMessage.getData();
  RemoteMessage.Notification notification = remoteMessage.getNotification();

  String landingUrl = data.get("url");
  String iconUrl = notification.getIcon();
  String messageTitle = notification.getTitle();
  String messgeBody = notification.getBody();

  // ...
}
```

## 4. Stats

All subscription and delivery stats is available in the Flocktory cabinet just like Web Push stats.
