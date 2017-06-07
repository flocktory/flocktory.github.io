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

* с каждым запросом к Flocktory API необходимо передавать уникальный идентификатор приложения (в некоторых терминологиях device-uuid). Идентификатор является уникальным и не должен меняться после перезапуска приложения. Данный идентификатор во всех запросах необходимо передавать дополненной переменной ?uuid;

* если при авторизации в мобильном приложение пользователь указывает email адрес, его так же следует передать во Flocktory;



## 3. Описание API

Функционал API состоит из нескольких частей:

* передача событий и информации во Flocktory;
* получение и обработка Push уведомлений;

### Часть 1. Передача информации во Flocktory.

Необходимо передавать следующую информацию и события (в качестве примеров приведены curl запросов; в параметр site необходимо передавать id, соответствующий вашему сайту в системе Flocktory; в папаметр url необходимо передавать deep-link, который позволит Flocktory посадить пользоватля на эту страницу; параметр body должен быть в виде urlencoded):

1. пользователь совершил заказ
```
curl 'https://api.flocktory.com/1/postcheckout/offer.js?uuid=123&body={"site_id":1833,"jsapi_version":"2.0","i":"5805265","e":"johnny.appleseed@gmail.com","n":"Johnny Appleseed","p":16790,"s":"m","t":{"0":{"i":7752795,"t":"Nokia Lumia 800","u":"https://assets.flocktory.com/uploads/clients/1063/5bb944e2-70b8-4912-bc8f-ee43e345be4f_lumia.jpg","c":1,"p":16790,"custom_data":{}}},"ga":{"utmcsr":"flowdock.com","utmccn":"referral","utmcmd":"referral","pageviews":17,"previous_visit_ts":1492714292,"current_visit_at":1492775373}}&callback=flock_jsonp' -H 'pragma: no-cache' -H 'accept-encoding: gzip, deflate, sdch, br' -H 'accept-language: ru-RU,ru;q=0.8,en-US;q=0.6,en;q=0.4' -H 'accept: */*' -H 'cache-control: no-cache' --compressed -g
```

2. пользователь просмотрел товар
```
curl 'https://api.flocktory.com/underworld/tracks/ultimate.js?uuid=123&body={"data":{"action":"customer.item_visit","links":{"yandex_offer":"1","site":1833},"payload":{"url":"http://spreadreward.com/"}}}' -H 'pragma: no-cache' -H 'accept-encoding: gzip, deflate, sdch, br' -H 'accept-language: ru-RU,ru;q=0.8,en-US;q=0.6,en;q=0.4' -H 'accept: */*' -H 'cache-control: no-cache'  --compressed -g
```

3. пользователь просмотрел категорию
```
curl 'https://api.flocktory.com/underworld/tracks/ultimate.js?uuid=123&body={"data":{"action":"customer.category_visit","links":{"yandex_category":"1","site":1833},"payload":{"url":"http://spreadreward.com/"}}}' -H 'pragma: no-cache' -H 'accept-encoding: gzip, deflate, sdch, br' -H 'accept-language: ru-RU,ru;q=0.8,en-US;q=0.6,en;q=0.4' -H 'accept: */*' -H 'cache-control: no-cache'  --compressed -g
```

4. пользователь просмотрел страницу
```
curl 'https://api.flocktory.com/underworld/tracks/ultimate.js?uuid=123&body={"data":{"action":"session.page_visit","payload":{"resolution":"1366x741","ga":{"utmcsr":"","utmccn":"","utmcmd":"","h_utmcsr":"","h_utmccn":"","h_utmcmd":""},"url":"http://spreadreward.com/"},"links":{"site":1833}}}' -H 'pragma: no-cache' -H 'accept-encoding: gzip, deflate, sdch, br' -H 'accept-language: ru-RU,ru;q=0.8,en-US;q=0.6,en;q=0.4' -H 'accept: */*' -H 'cache-control: no-cache' --compressed -g
```

5. пользователь добавил что-то в корзину
```
curl 'https://api.flocktory.com/underworld/tracks/ultimate.js?uuid=123&body={"data":{"action":"customer.add_to_cart","links":{"yandex_offer":"7345265","site":1833},"payload":{"count":1,"custom_data":{"id":"7345265","price":832,"count":1},"url":"http://1833.demoshop.flocktory.com/"}}}' -H 'pragma: no-cache' -H 'accept-encoding: gzip, deflate, sdch, br' -H 'accept-language: ru-RU,ru;q=0.8,en-US;q=0.6,en;q=0.4' -H 'accept: */*' -H 'cache-control: no-cache' --compressed -g
```

6. пользователь удалил что-то из корзины
```
curl 'https://api.flocktory.com/underworld/tracks/ultimate.js?uuid=123&body={"data":{"action":"customer.remove_from_cart","links":{"yandex_offer":"7345265","site":1833},"payload":{"count":1,"custom_data":{"id":"7345265","count":1},"url":"http://1833.demoshop.flocktory.com/"}}}' -H 'pragma: no-cache' -H 'accept-encoding: gzip, deflate, sdch, br' -H 'accept-language: ru-RU,ru;q=0.8,en-US;q=0.6,en;q=0.4' -H 'accept: */*' -H 'cache-control: no-cache' --compressed -g
```

7. пользователь оставил емейл
```
curl 'https://api.flocktory.com/u_shaman/setup-api.js?body={"siteId":"1833","uuid":"123","profile":{"email":"asd@asd.ru","name":"johnny"}}&callback=flock_jsonp_1' -H 'pragma: no-cache' -H 'accept-encoding: gzip, deflate, sdch, br' -H 'accept-language: ru-RU,ru;q=0.8,en-US;q=0.6,en;q=0.4'  --compressed -g
```

8. для пользователя получена Push подписка.
```
curl 'https://api.flocktory.com/u_flockman/attach-push-to-session.js?uuid=123&body={"platform":"chrome","site-id":"1833","token":"https://android.googleapis.com/gcm/send/...","url":"http://spreadreward.com/","provider-meta":[{"key":"auth","value":"..."},{"key":"p256dh","value":"..."}]}&callback=flock_jsonp' -H 'accept-encoding: gzip, deflate, sdch, br' -H 'accept-language: en-US,en;q=0.8' -H 'accept: */*'  --compressed -g
```


### Часть 2. Сбор и обработка пуш уведомлений.

Возможны два варианта:

* Вариант 1. Сбор мобильных уведомлений еще не ведется;

* Вариант 2. Сбор мобильных уведомлений ведется с помощью стороннего функционала;



_Вариант 1. Сбор мобильных Push уведомлений еще не ведется._

В этом случае мы советуем использовать firebase.google.com

Большинство проектов так или иначе уже имеют у себя в зависимостях firebase. Установка данного SDK, позволяет работать со всем стандартным функционалом.

Примечания:

* в случае работы с Flocktory по функционалу Web Push проект в firebase уже будет создан и создавать новый проект для mobile push - не нужно, используется тот же самый проект.

* если у вас уже есть проект в Firebase - об этом необходимо проинформировать вашего аккаунт менеджера для согласования дальнейших шагов по его использованию.

* функционал сбора Push уведомлений, а именно запрос разрешений, переподписка, отписка - лежит полностью на UI части партнера. Flocktory предоставляет только метод 8 для передачи информации о том что подписка собрана.

* функционал deep-link: данная возможность доступна из коробки, вашим разработчикам необходимо предоставить формат deep-link который вы поддерживаете и который  будет учитываться при запуске workflow механик.



_Вариант 2. Сбор мобильных уведомлений ведется с помощью стороннего функционала._

В случае корректной реализации функционала сбора Push подписок согласно спецификации, процесс работы выглядит следующим образом:

Flocktory отсылает Push уведомления в GCM/FCM/APN  в неизмененном виде. Для проверки корректности работы необходима консультация с вашим отделом разработки и проведение серии тестов.

В случае корректной работы в момент сбора подписки необходимо вызвать метод №8 и передать во Flocktory информацию о подписке. Далее работа ведет по стандартной схеме.

Ссылки на спецификации от GCM:

* получение Push подписок https://developers.google.com/cloud-messaging/android/client и https://developers.google.com/cloud-messaging/ios/client

* получение Push уведомлений https://developers.google.com/cloud-messaging/downstream

## 4. Статистика

Вся статистика по сбору Push подписок и проведенным рассылкам будет доступна в личном кабинете Focktory аналогично статистике по Web Push.
