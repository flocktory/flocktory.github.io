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

1. Создать Precheckout-кампанию, выбрать тип MGM

![Создать кампанию]( https://assets.flocktory.com/uploads/clients/2708/1348546c-ea04-4c26-ac94-8883717a7fe6_create_mgm.png)

2. Выбрать аудиторию, либо просто нажать далее
![Шаг 1](https://assets.flocktory.com/uploads/clients/2708/949d3812-c5a2-4617-b1ea-adb376d6bda2_step-2.png)

3. Проставить лимиты срабатывания, выбрать условия срабатывания, нажать далее
![лимиты условия](https://assets.flocktory.com/uploads/clients/2708/27a0e5c6-02bc-4859-bf2d-812688d8e35c_step-3.png)

4. Выбрать готовый виджет, либо вставить новый через редактор, нажать далее
![виджет](https://assets.flocktory.com/uploads/clients/2708/5136fa21-42a5-4651-8fda-641522e3f8bf_widget.png)

5. Настроить посты в соцсетях, нажать далее
![social](https://assets.flocktory.com/uploads/clients/2708/23e11694-f271-42ab-9ee0-b141bcdd4665_social.png)

5.1 Доступные переменные можно посмотреть на шаге настройки постов в соцсетях по кнопке на скриншоте

![var](https://assets.flocktory.com/uploads/clients/2708/41cd0619-769e-4ee0-b82c-76e7d9805b10_variable.png)

6. Заполнить имя кампании, сохранить и запустить
![final](https://assets.flocktory.com/uploads/clients/2708/da297bc9-34fc-46ad-84fb-f5ce135a14df_final.png)


## Привязка мотивация

Для привязки мотиваций паблишеру и другу используем postcheckout-интерфейс редактирования кампании. Для этого

1. Копируем id только что созданной пречекаут кампании
![Скопировать id кампании](https://assets.flocktory.com/uploads/clients/2708/55ecb2c4-651a-4742-84f5-98bc935f6d10_copy_id.png)

2. Открываем любую кампанию в postcheckout-интерфейсе, подставляем вместо ID кампании скопированный ID из MGM кампании
![Открываем кампанию в postcheckout-интерфейсе](https://assets.flocktory.com/uploads/clients/2708/91f7bc40-789f-4665-8a36-04d2128e3b2c_insert_id.png)

3. Привязываем нужные мотивации паблишеру и другу.
![Привязываем нужные мотивации](https://assets.flocktory.com/uploads/clients/2708/75e38398-5ece-431c-be05-6e04e1b65609_rewards.png)

4. Заполняем пользовательское соглашение, сохраняем кампанию
![Сохранить кампанию](https://assets.flocktory.com/uploads/clients/2708/596d72d1-1b73-42cc-9de2-3f23de23b060_savereward.png)


## Привязка писем

Если нужно использовать кастомные письма, то их также нужно привязать через postcheckout-интерфейс. В мгм используются следующие шаблоны:


**share_email** – паблишер поделился скидкой с другом через письмо

**friend_ordered_email** – письмо паблишеру, если друг сделает заказ на сайте



1. Открываем раздел настройки писем в настройке интерфейса Postcheckout, по аналогии с привязкой мотивации
![Открываем раздел настройки писем](https://assets.flocktory.com/uploads/clients/2708/f25f142b-8c71-4a6d-bd28-6fb13fd930c1_emailmanu.png)


2. Выбираем нужное письмо, например share_email
![Выбираем нужное письмо](https://assets.flocktory.com/uploads/clients/2708/78da4ac6-38dd-429b-a731-7ee8673c12fb_chooseemail.png)

3. Переходим к выбору html-шаблона
![Выбрать html-шаблон](https://assets.flocktory.com/uploads/clients/2708/7ec898f6-e75e-446c-a6c8-7d5df8a7ff7b_emailtemplate.png)

4. Дублируем дефолтный шаблон письма
![Дублируем дефолтный шаблон](https://assets.flocktory.com/uploads/clients/2708/3205de0e-264b-4173-8cc0-f33f6354a7e3_dupe.png)

5. Указываем название письма и вставляем новую верстку, нажимаем "использовать", чтобы привязать письмо к кампании
![Редактируем письмо](https://assets.flocktory.com/uploads/clients/2708/4cffa0b3-510c-4508-a3e5-277dcf24a19a_saveemail.png)

6. Сохраняем кампанию по кнопке в самом низу формы
![Сохраняем кампанию](https://assets.flocktory.com/uploads/clients/2708/bc33d1e2-ced8-4614-abc6-f68b3a4d83d3_savemgm.png)

Если нужно, повторяем тот же порядок действий для **friend_ordered_email**.
