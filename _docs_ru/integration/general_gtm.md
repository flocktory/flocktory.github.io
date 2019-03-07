---
layout: doc
title:  "Общая интеграция GTM"
section: integration
order: 1
---
Общая интеграция через [Google Tag Manager](https://www.google.com/intl/ru/tagmanager/).

## Необходимые требования для начала интеграции
Требований к сайту для начала интеграции нет.


## Общая интеграция Flocktory при помощи GTM
Прежде чем подключить любой из модулей Flocktory, необходимо провести общую интеграцию. Чтобы сделать это при помощи GTM, необходимо выполнить следующие действия:
1.	Откройте [Google Tag Manager](https://tagmanager.google.com/#/home) и перейдите в раздел «Теги»:
![screen1](https://i.imgur.com/gYEyy6V.jpg)
 
2.	Откройте окно создания тегов при помощи кнопки «Создать»:
![screen2](https://i.imgur.com/8ymyL3E.jpg)
 
3.	Введите название нового тега:
![screen3](https://i.imgur.com/TflamqX.jpg)
 
4.	Нажмите на поле «Конфигурация тега» и выберите тип тега «Пользовательский HTML»:
![screen4](https://i.imgur.com/7Z4Nkka.jpg)
 
5.	Вставьте следующий код в поле HTML и включите поддержку функции «document.write»:
```html
<script type="text/javascript" src="https://api.flocktory.com/v2/loader.js?site_id=YOUR_SITE_ID" async="async"></script>
```
![screen5](https://i.imgur.com/adEuVVU.jpg)
 
6.	Замените YOUR_SITE_ID на присвоенный вашему сайту ID (узнать его вы можете в [личном кабинете](https://cabinet.flocktory.com/)):
![screen6](https://i.imgur.com/ZLRz2vA.jpg)
 
7.	Нажмите на поле «Триггеры» и выберите из списка триггер «All pages»:
![screen7](https://i.imgur.com/RE1w5UI.jpg)
 
8.	Сохраните внесенные изменения при помощи кнопки «Сохранить»:
![screen8](https://i.imgur.com/axms4af.jpg)
 
9.	Опубликуйте внесённые в контейнер изменения при помощи кнопки «Отправить»:
![screen9](https://i.imgur.com/KsBGHQc.jpg)
 
10.	Введите название версии, описание изменений и опубликуйте новую версию вашего контейнера:
![screen10](https://i.imgur.com/yA1vClZ.jpg)
 

## На что стоит обратить внимание
1.	Site_id должен быть корректным, иначе будут запускаться кампании, предназначенные для других сайтов;
2.	Часто встречается ошибка, когда при вставке id ставят пробел после знака равенства - это неправильно и код работать не будет;
3.	Код обязательно должен срабатывать на каждой странице сайта и иметь статус 200.


## Как проверить, что код работает
1.	Откройте любую страницу Вашего сайта, нажмите F12 или Shift + Ctrl + I чтобы открыть панель инструментов разработчика;
2.	Выберите вкладку Network и обновите страницу:
![screen11](https://i.imgur.com/0QgaWOI.jpg)
 
3.	В поле «Фильтр» введите слово Flocktory:
![screen12](https://i.imgur.com/OGOryd1.jpg)
 
4.	Среди появившихся запросов выберите loader.js и выберите вкладку «Headers»:
![screen13](https://i.imgur.com/qayyvVW.jpg)
![screen14](https://i.imgur.com/3hSCxDc.jpg)
 
5.	Убедитесь, что код имеет статус 200, и прокрутите содержимое вкладки до конца, пока не увидите site_id - он должен совпадать с вашим:
![screen15](https://i.imgur.com/EAUmeaT.jpg)
![screen16](https://i.imgur.com/7NtelhQ.jpg)

6. Если при проверке не было обнаружено ошибок, то общая интеграция завершена.
