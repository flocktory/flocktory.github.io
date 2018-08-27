---
layout: doc
title:  "Mobile Push Integration"
#date:   2017-02-08 16:34:18 +0300
section: push
---
# Flocktory Mobile Push Integration Guide



## 1. Предпосылки технической реализации Flocktory Mobile Push

* использование любого SDK утяжеляют размер мобильного приложения и в связи с этим разработчики как правило стараются ограничить число используемых SDK;

* для работы с функционалом Flocktory Mobile Push достаточно написать 5-7 HTTP запросов, что не предоставляет сложности для мобильных разработчиков;

Учитывая вышеперечисленные факторы нами было принято решение отказаться от реализации SDK в пользу предоставления API, которое позволит реализовать весь функционал и при этом не будет добавлять зависимости в проект.

Процесс интеграции сводится к передаче вашей команде разработчиков данной инструкции.

Все возникающие вопросы в процессе интеграции вы сможете обсудить и решить с нашей командой IT специалистов.



## 2. Требования:

* При первом запуске приложения нужно сделать запрос 7, содержащий только site-id, и сохранить из ответа данные, которые нужно затем передавать при любом запросе к flocktory:
    * Значение куки `__flocktory_web_session2` (будет содержаться в заголовке set-cookie). Эту куку нужно использовать во всех последующих запросах
    * значение `site-session-id` из тела ответа. Ответ будет вида `...({"site-session-id":"*",...})`. этом значение нужно передавать в соответствующем поле каждого запроса (см примеры ниже)

* если при авторизации в мобильном приложение пользователь указывает email адрес, его так же следует передать во Flocktory (см запрос 7);



## 3. Описание API

Функционал API состоит из нескольких частей:

* передача событий и информации во Flocktory;
* получение и обработка Push уведомлений;

### Часть 1. Передача информации во Flocktory.
Все параметры являются обязательными для передачи, если не указано иначе ("опционально").

Необходимо передавать следующую информацию и события (в качестве примеров приведены curl запросов; в параметр site_id необходимо передавать id, соответствующий вашему сайту в системе Flocktory; в параметр url необходимо передавать deep-link, который позволит Flocktory посадить пользоватля на эту страницу; параметр body должен быть в виде urlencoded):

1. пользователь совершил заказ
```curl
curl 'https://api.flocktory.com/1/postcheckout/offer.js?body={"site_id":1833,"jsapi_version":"2.0","i":"5805265","e":"johnny.appleseed@gmail.com","n":"Johnny Appleseed","p":16790,"t":{"0":{"i":7752795,"t":"Nokia Lumia 800","u":"https://assets.flocktory.com/uploads/clients/1063/5bb944e2-70b8-4912-bc8f-ee43e345be4f_lumia.jpg","c":1,"p":16790}},"site-session-id":"123"}&callback=flock_jsonp' -H 'pragma: no-cache' -H 'accept-encoding: gzip, deflate, sdch, br' -H 'accept-language: ru-RU,ru;q=0.8,en-US;q=0.6,en;q=0.4' -H 'accept: */*' -H 'cache-control: no-cache' --compressed -g
```
* **site_id** - id вашего сайта в системе Flocktory
значения в body
* **i** - id заказа
* **e** - email пользователя
* **n** - имя (и фамилия) пользователя (передавать в порядке "Имя Фамилия")
* **p** - общая стоимость заказа
* **t** - массив товаров заказа вида {"0":{},"1":{} ... }
* **t-i** - id товара
* **t-t** - название товара
* **t-u** - ссылка на изображение товара (опционально)
* **t-с** - количество
* **t-p** - цена за единицу

2. пользователь просмотрел товар
```curl
curl 'https://api.flocktory.com/underworld/tracks/ultimate.js?body={"data":{"action":"customer.item_visit","links":{"yandex_offer":"1","site":1833},"payload":{"url":"http://spreadreward.com/"}},"site-session-id":"123"}' -H 'pragma: no-cache' -H 'accept-encoding: gzip, deflate, sdch, br' -H 'accept-language: ru-RU,ru;q=0.8,en-US;q=0.6,en;q=0.4' -H 'accept: */*' -H 'cache-control: no-cache'  --compressed -g
```
* **links.yandex_offer** - id товара
* **links.site** - id вашего сайта в системе Flocktory
* **payload.url** - диплинк на страницу товара

3. пользователь просмотрел категорию
```curl
curl 'https://api.flocktory.com/underworld/tracks/ultimate.js?body={"data":{"action":"customer.category_visit","links":{"yandex_category":"1","site":1833},"payload":{"url":"http://spreadreward.com/"}},"site-session-id":"123"}' -H 'pragma: no-cache' -H 'accept-encoding: gzip, deflate, sdch, br' -H 'accept-language: ru-RU,ru;q=0.8,en-US;q=0.6,en;q=0.4' -H 'accept: */*' -H 'cache-control: no-cache'  --compressed -g
```
* **links.yandex_category** - id категории
* **links.site** - id вашего сайта в системе Flocktory
* **payload.url** - диплинк на страницу категории

4. пользователь просмотрел страницу
```curl
curl 'https://api.flocktory.com/underworld/tracks/ultimate.js?body={"data":{"action":"session.page_visit","payload":{"resolution":"1366x741","ga":{"utmcsr":"","utmccn":"","utmcmd":"","h_utmcsr":"","h_utmccn":"","h_utmcmd":""},"url":"http://spreadreward.com/"},"links":{"site":1833}},"site-session-id":"123"}' -H 'pragma: no-cache' -H 'accept-encoding: gzip, deflate, sdch, br' -H 'accept-language: ru-RU,ru;q=0.8,en-US;q=0.6,en;q=0.4' -H 'accept: */*' -H 'cache-control: no-cache' --compressed -g
```
* **payload.resolution** - разрешение экрана устройства
* **payload.ga** - здесь оставляйте значения пустыми, как указано в примере
* **payload.url** - диплинк на просматриваемую страницу
* **links.site** - id вашего сайта в системе Flocktory

5. пользователь добавил что-то в корзину
```curl
curl 'https://api.flocktory.com/underworld/tracks/ultimate.js?body={"data":{"action":"customer.add_to_cart","links":{"yandex_offer":"7345265","site":1833},"payload":{"count":1,"custom_data":{"id":"7345265","price":832,"count":1},"url":"http://1833.demoshop.flocktory.com/"}},"site-session-id":"123"}' -H 'pragma: no-cache' -H 'accept-encoding: gzip, deflate, sdch, br' -H 'accept-language: ru-RU,ru;q=0.8,en-US;q=0.6,en;q=0.4' -H 'accept: */*' -H 'cache-control: no-cache' --compressed -g
```
* **links.yandex_offer** - id товара
* **links.site** - id вашего сайта в системе Flocktory
* **payload.count** - количество добавленных единиц товара
* **payload.price** - цена за единицу

6. пользователь удалил что-то из корзины
```curl
curl 'https://api.flocktory.com/underworld/tracks/ultimate.js?body={"data":{"action":"customer.remove_from_cart","links":{"yandex_offer":"7345265","site":1833},"payload":{"count":1,"custom_data":{"id":"7345265","count":1},"url":"http://1833.demoshop.flocktory.com/"}},"site-session-id":"123"}' -H 'pragma: no-cache' -H 'accept-encoding: gzip, deflate, sdch, br' -H 'accept-language: ru-RU,ru;q=0.8,en-US;q=0.6,en;q=0.4' -H 'accept: */*' -H 'cache-control: no-cache' --compressed -g
```
* **links.yandex_offer** - id товара
* **links.site** - id вашего сайта в системе Flocktory
* **payload.count** - количество удаленных единиц товара
* **payload.price** - цена за единицу

7. пользователь оставил емейл<br>
используйте данный код при авторизации пользователя в приложении и других случаях, когда пользователь оставляет емейл (например, при подписке на новостную рассылку)
```curl
curl 'https://api.flocktory.com/u_shaman/setup-api?body={"siteId":"1833","profile":{"email":"asd@asd.ru","name":"johnny"},"site-session-id":"123"}' -H 'pragma: no-cache' -H 'accept-encoding: gzip, deflate, sdch, br' -H 'accept-language: ru-RU,ru;q=0.8,en-US;q=0.6,en;q=0.4'  --compressed -g
```
* **siteId** - id вашего сайта в системе Flocktory
* **profile.email** - емейл пользователя
* **profile.name** - имя пользователя 

8. для пользователя получена Push подписка
* вы используете **firebase**
  * вместо многоточия подставьте полученный при подписке токен ([android](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId.html#getToken(java.lang.String, java.lang.String),[ios](https://firebase.google.com/docs/cloud-messaging/ios/client#access_the_registration_token))))
  * в поле os нужно передавать "android" или "ios"
```curl
curl 'https://api.flocktory.com/u_flockman/attach-push-to-session?body={"from-mobile-app":true,"platform":"firebase","os":"android","site-id":"1833","token":"https://android.googleapis.com/gcm/send/...","site-session-id":"123"}' -H 'accept-encoding: gzip, deflate, sdch, br' -H 'accept-language: en-US,en;q=0.8' -H 'accept: */*'  --compressed -g
```
В случае если вы решили использовать отдельный firebase проект для работы с нотификациями в приложении, нужно при подписке передавать в запросе используемый gcm-sender-id следующим образом:
```curl
curl 'https://api.flocktory.com/u_flockman/attach-push-to-session?body={"from-mobile-app":true,"platform":"firebase","os":"android","site-id":"1833","token":"https://android.googleapis.com/gcm/send/...","provider-meta":{"gcm-sender-id":"321"},"site-session-id":"123"}' -H 'accept-encoding: gzip, deflate, sdch, br' -H 'accept-language: en-US,en;q=0.8' -H 'accept: */*'  --compressed -g
```
* вы используете **appmetrica**
  * в поле os нужно передавать "android" или "ios"
  * в поле token надо передавать значение appmetrica_device_id
```curl
curl 'https://api.flocktory.com/u_flockman/attach-push-to-session?body={"from-mobile-app":true,"platform":"app-metrica","os":"android","site-id":"1833","token":"","site-session-id":"123"}' -H 'accept-encoding: gzip, deflate, sdch, br' -H 'accept-language: en-US,en;q=0.8' -H 'accept: */*' -g --compressed 
```

#### Дополнительные запросы, которые также могут пригодиться. В случае необходимости о деталях Вам сообщит менеджер flocktory.

* Передача кастомного события
```curl
curl 'https://api.flocktory.com/u_shaman/custom-events.js?body={"event":"test","site-id":"1833","site-session-id":"a7ee8521-80bf-43bf-9df1-d0d85f5de7c7-4"}&callback=flock_jsonp' --compressed -g
```

* Передача произвольных параметров пользователя
```curl
curl 'https://api.flocktory.com/u_flockman/set-profile-custom-meta.js?body={"site-id":"1833","meta":{"test":"1"},"site-session-id":"a7ee8521-80bf-43bf-9df1-d0d85f5de7c7-3"}&callback=flock_jsonp' --compressed -g
```

### Часть 2. Сбор и обработка пуш уведомлений.

Возможны два варианта:

* Вариант 1. Сбор мобильных уведомлений еще не ведется;

* Вариант 2. Сбор мобильных уведомлений ведется с помощью стороннего функционала;



_Вариант 1. Сбор мобильных Push уведомлений еще не ведется._

В этом случае мы советуем использовать [firebase.google.com](https://firebase.google.com)

Большинство проектов так или иначе уже имеют у себя в зависимостях firebase. Установка данного SDK позволяет работать со всем стандартным функционалом.

Примечания:

* в случае работы с Flocktory по функционалу Web Push проект в firebase уже будет создан и создавать новый проект для mobile push - не нужно, используется тот же самый проект.

* если у вас уже есть проект в Firebase - об этом необходимо проинформировать вашего аккаунт менеджера для согласования дальнейших шагов по его использованию.

* функционал сбора Push уведомлений, а именно запрос разрешений, переподписка, отписка - лежит полностью на UI части партнера. Flocktory предоставляет только метод 8 для передачи информации о том что подписка собрана.

* функционал deep-link: данная возможность доступна из коробки, вашим разработчикам необходимо предоставить формат deep-link который вы поддерживаете и который  будет учитываться при запуске workflow механик.



_Вариант 2. Сбор мобильных уведомлений ведется с помощью стороннего функционала._

В случае корректной реализации функционала сбора Push подписок согласно спецификации, процесс работы выглядит следующим образом:

**Flocktory отсылает Push уведомления в GCM/FCM/APN  в неизмененном виде. Для проверки корректности работы необходима консультация с вашим отделом разработки и проведение серии тестов.**

В случае корректной работы в момент сбора подписки необходимо вызвать метод №8 и передать во Flocktory информацию о подписке. Далее работа ведет по стандартной схеме.

Ссылки на спецификации от FCM:

* настройка клиента: [android](https://firebase.google.com/docs/cloud-messaging/android/client) , [ios](https://firebase.google.com/docs/cloud-messaging/ios/client)

* получение токена подписки: [android](https://firebase.google.com/docs/cloud-messaging/android/client#sample-register) , [ios](https://firebase.google.com/docs/cloud-messaging/ios/client#access_the_registration_token)

* получение уведомлений: [android](https://firebase.google.com/docs/cloud-messaging/android/receive) , [ios](https://firebase.google.com/docs/cloud-messaging/ios/receive)



##### **Стандартные данные, передаваемые flocktory в пуш-сообщении**
* landingUrl - ссылка для приземления пользователя по клику
* messageTitle - текст верхней части уведомления
* body - текст основной части уведомления
* iconUrl - ссылка на картинку

Важно:
* landingUrl и iconUrl - ссылки, которые произведут один или несколько редиректов.
* Использование этих ссылок необходимо для корректного сбора статистики. Даже если вы не планируете открывать landingUrl или показывать пришедшую картинку, необходимо сделать запросы по этим адресам (iconUrl при показе,  landingUrl при клике)

Если вы используете библиотеку firebase для работы с пуш-уведомлениями, эти данные получаются следующим образом ([документация](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/RemoteMessage)):

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

Также есть возможность передавать в remote message data произвольные пары ключ-значение, что позволяет решить проблему лендинга на конкретный экран приложения. Это можно настроить при создании рассылки в личном кабинете.

```java
  Map<String, String> data = remoteMessage.getData();
  String v1 = data.get("k1");
```

## 4. Статистика

Вся статистика по сбору Push подписок и проведенным рассылкам будет доступна в личном кабинете Focktory аналогично статистике по Web Push.
