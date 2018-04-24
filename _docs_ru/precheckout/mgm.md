---
layout: doc
title:  "Заведение MGM-кампаний"
section: precheckout
order: 4
---

## Что такое MGM?

MGM – member get member. Механика, когда пользователь (паблишер) делится вознаграждением с другом через социальные сети или email. Если кто-то из его друзей воспользуется
вознаграждением, то паблишер за это получит подарок.

## Заведение кампании

1. Создать Precheckout-кампанию

![Создать кампанию](https://assets.flocktory.com/uploads/clients/1559/4de32b81-e6e3-4b65-acc0-c56cb062e473_create-campaign.jpg)

2. Указать тип механики &laquo;MGM&raquo; и сохранить кампанию
![Указать тип механики и сохранить кампанию](https://assets.flocktory.com/uploads/clients/1559/775c804e-7b63-452d-851d-51bd22891c0d_save-campaign.jpg)


## Привязка мотивация

Для привязки мотиваций паблишеру и другу используем postcheckout-интерфейс редактирования кампании. Для этого

1. Копируем id только что созданной пречекаут кампании
![Скопировать id кампании](https://assets.flocktory.com/uploads/clients/1559/d906c501-f2e3-457e-8ae0-e93cf85e8223_copy-id-campaign.jpg)

2. Открываем кампанию в postcheckout-интерфейсе
![Открываем кампанию в postcheckout-интерфейсе](https://assets.flocktory.com/uploads/clients/1559/1a4d33e6-2437-40ed-a533-4a64a919cc3d_open-postcheckout.jpg)

3. Привязываем нужные мотивации паблишеру и другу.
![Привязываем нужные мотивации](https://assets.flocktory.com/uploads/clients/1559/25877070-1c27-4765-aaa0-ba8ca9114145_publisher-friend-coupons.jpg)

4. Сохраняем кампанию
![Сохранить кампанию](https://assets.flocktory.com/uploads/clients/1559/893fb063-8897-4a71-8c6f-37c88d92bb3a_save-post.jpg)


## Привязка писем

Если нужно использовать кастомные письма, то их также нужно привязать через postcheckout-интерфейс. В мгм используются следующие шаблоны:


**share_email** – паблишер поделился скидкой с другом через письмо

**friend_ordered_email** – письмо паблишеру, если друг сделает заказ на сайте



1. Открываем раздел настройки писем
![Открываем раздел настройки писем](https://assets.flocktory.com/uploads/clients/1559/4aaab84c-4875-45e3-876f-a8c62e951395_email-section.jpg)


2. Выбираем нужное письмо, например share_email
![Выбираем нужное письмо](https://assets.flocktory.com/uploads/clients/1559/5f5cb3bc-55f7-4a0c-ba38-4f5957a1f3f4_choose-share-email.jpg)

3. Переходим к выбору html-шаблона
![Выбрать html-шаблон](https://assets.flocktory.com/uploads/clients/1559/a207cd5e-d4fc-4332-9e34-423186b5d4b5_to-html-template.jpg)

4. Дублируем дефолтный шаблон письма
![Дублируем дефолтный шаблон](https://assets.flocktory.com/uploads/clients/1559/7cb75133-7a80-4680-aafb-4f9efe478cf9_duplicate.jpg)

5. Указываем название письма и вставляем новую верстку, привязываем к кампании
![Редактируем письмо](https://assets.flocktory.com/uploads/clients/1559/0b7e2701-f445-443b-b970-c14a95d4c885_edit-email.jpg)

6. Сохраняем кампанию
![Сохраняем кампанию](https://assets.flocktory.com/uploads/clients/1559/bb0ab5d4-9bc4-4aea-9c2f-b8c8b5845d11_save-email-to-campaign.jpg)

Если нужно, повторяем тот же порядок действий для **friend_ordered_email**.


## Редактирование соц. постов

Соц. посты настраиваются через интерфейс пречекаут, в&nbsp;разделе &laquo;Страница приземления&raquo;. На&nbsp;данный момент в&nbsp;текстах нет поддержки плейсхолдеров (%site_name%, %expires_at% и&nbsp;т.&nbsp;д.). Все значения указываем текстом.

![Редактирование соц. постов](https://assets.flocktory.com/uploads/clients/1559/4364c98f-a975-437f-96fa-4a4ccb941de1_social-texts.jpg)




