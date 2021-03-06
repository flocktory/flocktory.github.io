---
layout: doc
title:  "Триггеры"
section: precheckout
order: 2
---

Триггеры позволяют запускать кампании выборочно, в зависимости от страницы и сессии пользователя.

## По покупателю

### Есть авторизация

**Текущее состояние:** работает

**Ожидаемый ввод** Выбор одного из двух вариантов "да" - если таргетируются только авторизованные и "нет" - если таргетируются только неавторизованные
Принцип работы: пользователь считается авторизованным, если сайт передает нам его email через код авторизации пользователя при приземлении на страницу (Раздел 2 инструкции по интеграции). При этом, если пользователь авторизован, сайт должен передавать его email **на каждой странице** сессии

**Принцип работы:** пользователь считается авторизованным, если сайт передает нам его email через код авторизации пользователя при приземлении на страницу (Раздел 2 инструкции по интеграции). При этом, если пользователь авторизован, сайт должен передавать его email на каждой странице сессии

**Известные проблемы:** бывают сайты, на которых авторизация производится без использования email (например, через телефон). В таких случаях, клиенту необходимо настроить передачу email следующего формата: 79999999999@unknown.email (где 79999999999 - номер телефона). Это важно, так как **передавать имя пользователя недостаточно**, для определения пользователя как авторизованного.

### Впервые на сайте
**Текущее состояние:** работает

**Ожидаемый ввод:** выбор одного из двух вариантов "да" - если таргетируются только пользователи, для кого это первая сессия на сайте и "нет" - если таргетируются только те, кто ранее был на сайте

**Принцип работы:** при приземлении пользователя на сайт наш код проверяет есть ли у него записи по данному сайту в local storage его браузера. Если записи найдено не было, то текущая сессия считается первой на этом сайте. Если же запись по данному сайту имеется, то считается, что пользователь на данном сайте уже был (т.к. другим образом данную запись он получить не мог).  Важное отличие local storage от cookie заключается в том, что local storage не имеет срока действия. Соответственно, если пользователь перешел на сайт, то все последующие переходы на сайт мы будет считать повторными независимо от того сколько времени прошло с последнего визита

**Известные проблемы:** важно понимать, что повторные сессии возникают только у пользователей, которые первый раз посещали сайт уже после установки нашего кода. Другими словами, в первый день работы нашего кода на сайте 99% всех пользователей для нас будут новыми (т.е. будут впервые на сайте с точки зрения нашей системы), а значит, что для использования данного таргетинга наш код должен простоять на сайте достаточное количество времени (зависит от среднего цикла покупки на сайте). Также в данном случае актуальны стандартные проблемы, связанные с идентификацией пользователя по данным записанным в браузере. То есть в следующих случаях мы не можем определить, что

* пользователь до этого уже посещал данный сайт:
* пользователь переходит на сайт с другого устройства
* пользователь переходит на сайт с другого браузера
* пользователь переходит на сайт в браузере в режиме инкогнито
* пользователь чистил браузерную историю, включая "cookies, а также другие файлы сайтов и плагинов"

### Количество просмотров

**Текущее состояние:** работает

**Ожидаемый ввод:** ввод разделяется на две части
Принцип работы: каждый раз, когда наш код прогружается при загрузке страницы сайта, мы записываем, что был просмотр. Причем количество просмотренных страниц обнуляется, когда заканчивается сессия пользователя на сайте (после 30 минут инактивности)
от: число, показывающее, начиная с какого количества просмотренных страниц, следует показывать данную механику
до: число, показывающее, после какого количества просмотренных страниц, следует перестать показывать данную механику
При этом если заполнено только одно из этих двух

**Известные проблемы:** в случаях, когда наш код стоит недостаточно высоко в порядке загрузки, мы можем не успеть прогрузиться до момента, когда пользователь уйдет на следующую страницу. Таким образом. мы можем сосчитать не все страницы, которые пользователь посетил. Соответственно, если требуется достаточно высокая точность в данном таргетинге, **код должен ставиться как можно выше** в приоритете загрузки, чтобы погрешность была минимальной.

### Количество просмотренных товаров

**Текущее состояние:** работает

**Ожидаемый ввод:** ввод разделяется на две части

**Принцип работы:** у клиентов есть возможность указывать, что пользователь просмотрел товар. Данная информация сохраняется на клиенте. По этой информации мы может определять сколько всего товаров посмотрел пользователь.

* от: число, показывающее, начиная с какого количества просмотренных товаров (уникальных), следует показывать данную механику
* до: число, показывающее, после какого количества просмотренных товаров (уникальных), следует перестать показывать данную механику

### По странице входа

**Текущее состояние:** работает

**Ожидаемый ввод:** подстрока или regex

**Принцип работы:** триггер срабатывает, если URL первой странице в сессии попал под условие. Триггер будет срабатывать даже если адреса последующих страниц в сессии будут отличаться.

## По событиям

### При попадании на URL страницы

**Текущее состояние:** работает

**Ожидаемый ввод:** при настройке данного условия необходимо заполнить три поля (см скриншот ниже)Принцип работы: наш код регистрирует URL страницы, на которой он загружается. Далее идет простая проверка соответствия данного URL логическим условиям, указанным в таргетинге

1. 	данное поле отвечает за то, включаем ли мы страницы удовлетворяющие условию в таргетинг или исключаем. Пустое значение означает включение, "Не" означает исключение
1. 	данное поле отвечает за то, как проверяется соответствует ли страница, на которой находится пользователь, заданным условиям. В данном поле обязательно нужно выбрать одно из двух значений: match или regex.
  * match проверяет, что указанный в поле 3 кусок url присутствует в URL страницы. на которой находится пользователь. Например, при использовании параметра match и условия /item/ все страницы, содержащие в своем URL /item/ будут подходить под данное условие. Важное отличие match от regex, что при выборе match отсутствуют ограничения на использование специальных знаком (например, "/").
  * regex использует логику регулярных выражений для проверки соответствия страниц условиям. Регулярные выражения позволяют задавать сложные условия, накладываемые на URL страницы. Например, страницы, которые содержат /item/ в url, но при этом не содержат /fashion/, если мы хотим показывать нашу кампанию на всех страницах товаров, кроме товаров, относящихся к категории fashion. Это лишь один пример того какие условия могут быть реализованы в рамках регулярных выражений. Наиболее стандартные примеры возможных регулярных выражений вы можете найти здесь: Использование регулярных выражений
1. 	данное поле содержит "маску", по которой будет проводиться проверка выполнения условия. Т.е. если выбран параметр match, то указанное в данном поле значение будет искаться целиком в URL страницы, на которой находится пользователь. Если выбран regex, то проверяется соответствие URL страницы, на которой находится пользователь регулярному выражению заданному в данном поле

**Известные проблемы:** потенциальные проблемы могут быть вызваны некорректной формулировкой условий, поэтому необходимо подробно изучить какие URL вы хотите таргетировать, и сформулировать условие, которое покрывает данные страницы. После этого необходимо убедиться, что под данное условие не попадают никакие страницы, на которых вы механику показывать не хотите.

### При покидании страницы

**Текущее состояние:** работает

**Ожидаемый ввод:** если вы ставите галочку на "выполнится", то по данной группе условий механика будет показываться только при срабатывания сигнала о потенциальном уходе со страницы

**Принцип работы:** наш код отслеживает местоположение и движения мыши пользователя, и когда она пересекает **верхнюю** границу сайта, передается сигнал о том, что происходит уход с сайта

### При получении фокуса страницы

**Текущее состояние:** работает

**Ожидаемый ввод:** если вы ставите галочку на "выполнится", то по данной группе условий механика будет показываться только, когда страница находится в фокусе. Фокус означает, что вкладка браузера, на которой открыта страница сайта - активна. Например, если пользователь открыл сайт, но потом развернул окно skype, чтобы написать сообщение, то в этот момент страница сайта потеряла фокус. Теперь даже если пользователь по факту сайт видит, механики, требующие фокуса страницы не покажутся до тех пор, пока пользователь не вернется в браузер (например, кликнет на любую его часть)

**Принцип работы:** наш код получает информацию об активности вкладки, в которой он был загружен и запускает механики только тогда, когда вкладка активна.

**Известные проблемы:** по данному таргетингу проблем на текущий момент не выявлено


## По корзине

### Количество товаров в корзине

**Текущее состояние:** работает, но реализованных на его основе механик пока нет

**Ожидаемый ввод:** в поле "от" вводится минимальное количество товаров, которое должно находиться в корзине пользователя для того, чтобы механика сработала (ограничение работает включительно, т.е. "не менее чем"), в поле "до" вводится максимальное количество товаров, которое должно находиться в корзине для того, чтобы механика сработала (ограничение также работает включительно, т.е. "не более чем"). Если поле "до" оставлено пустым, то ограничения сверху не накладывается, аналогично с полем "от"

**Принцип работы:** через коды изменения состояния корзины (Раздел 5 инструкции по интеграции) сайт передает нашей системе информацию о том, что у пользователя находится в корзине, и мы записываем данную информацию в Local Storage браузера. При добавлении товара в корзину сайт должен передавать параметр, отвечающий за количество единиц товара добавленных в корзину. Далее наш код подсчитывает итоговое количество товаров в корзине и использует это число для проверки текущего состояния пользователя соответствию условиям кампании. Данный инструмент таргетинга крайне чувствителен к качеству интеграции клиента

**Известные проблемы:** информация о корзине хранится в рамках пользовательской сессии (30 минут инактивности пользователя). Таким образом, в случаях когда сайт хранит информацию о корзине более продолжительный срок, возможны ситуации, когда то состояние корзины, которое видит наш код не соответствует тому состоянию корзины, которое реально имеет место быть на сайте. Например, если пользователь добавил товары в корзину и вернулся на сайт на следующий день, магазин может хранить информацию об уже добавленных товарах. Наш же код будет видеть пустую корзину. Соответствие возможны случаи срабатывания механик рассчитанных на другой размер корзины, чем то, что у пользователя есть в данный момент

### Цена всех товаров

**Текущее состояние:** работает, но реализованных на его основе механик пока нет

**Ожидаемый ввод:** в поле "от" вводится минимальная стоимость всех товаров, которая должна находиться в корзине пользователя для того, чтобы механика сработала (ограничение работает включительно, т.е. "не менее чем"), в поле "до" вводится максимальное стоимость всех товаров, которая должна находиться в корзине для того, чтобы механика сработала (ограничение также работает включительно, т.е. "не более чем"). Если поле "до" оставлено пустым, то ограничения сверху не накладывается, аналогично с полем "от"

**Принцип работы:**  принцип аналогичный таргетингу по количеству товаров, единственное отличие заключается в том, что при добавлении товара в корзину сайт должен передавать не количество единиц товара добавленных в корзину, но и цену единицы. Далее наш код подсчитывает итоговую стоимость товаров в корзине и использует это число для проверки текущего состояния пользователя соответствию условиям кампании. Данный инструмент таргетинга также крайне чувствителен к качеству интеграции клиента

**Известные проблемы:** проблемы аналогичны проблемам, связанным с таргетингом по количеству товаров в корзине (см. выше)

## По времени

### Время за текущую сессию

**Текущее состояние:** работает

**Ожидаемый ввод:** два поля "от" и "до" работают аналогично другим случаям (т.е. задают минимальное и максимальное значение для проверяемого парамтра) и также требуют численного ввода. Отличием от предыдущих случаев заключается в том, что помимо пороговых значений необходимо выбрать единицу их измерения. Доступны следующие варианты: "секунды", "минуты", "часы", "дни". Дробные числа следует писать только через точку! Например, "2.5 минуты".

**Принцип работы:** при загрузке нашего кода первый раз за визит пользователя открывается новая сессия и запускается таймер, который отсчитывает время, которое пользователь провел на сайте за время своей сессии. Таймер обнуляется, когда закрывается его сессия на сайте. При этом, если страница теряет фокус (вкладка браузера неактивна), таймер останавливается.

**Известные проблемы:** время загрузки нашего кода достаточно важно для корректной работы данного таргетинга. Так, например, в случаях, если наш код загружается только на 20й-30й секунде нахождения на странице, сессия может быть открыта не на первой странице, а уже спустя какое-то время нахождения на сайте, что может приводить к тому, что реальное время на сайте в момент показа кампании может существенно отличаться от заданного в таргетинге (особенно, при небольших пороговых значениях). Соответственно, при реализации механики, использующей данный таргет, необходимо коммуницировать клиенту важность перемещения нашего кода выше в очереди загрузки

### Время на странице с начала ее открытия

**Текущее состояние:** работает

**Ожидаемый ввод:** формат ввода аналогичен вводу "времени за текущую сессию", единственное отличие заключается в том, что отсчитывается время на за всю сессию, а только время нахождения на текущей странице. Дробные числа следует писать только через точку!

**Принцип работы:** аналогично времени за текущую сессию, после загрузки нашего кода запускается таймер, который отсчитывает время нахождения на данной странице и таймер обнуляется, когда пользователь переходит на новую страницу. Как и в случае со временем за текущую сессию, если страница теряет фокус (вкладка браузера неактивна), таймер останавливается.

**Известные проблемы:** в данном случае чувствительность к времени загрузки нашего кода еще выше, чем в предыдущем случае, так как потенциально пользователь проводит не такое продолжительное время на каждой из страниц (в большинстве случаев в среднем не больше пары минут), а значит погрешности вызванные временем, когда наш код загружается, может внести значительные коррективы в работу данной механики. Соответственно, при реализации механики, использующей данный таргет, необходимо коммуницировать клиенту важность перемещения нашего кода выше в очереди загрузки

### Время неактивности пользователя

**Текущее состояние:** работает
Ожидаемый ввод: формат ввода аналогичен вводу для других таргетингов по времени, отличие в том, что отсчитывается время, которое пользователь был неактивен. Дробные числа следует писать только через точку!

**Принцип работы:** наш код отслеживает два фактора активности пользователя: движение мыши и ввод с клавиатуры. В случае, если пользователь неактивен (то есть нет движений мыши, и пользователь не осуществляет никакого ввода с клавиатуры), запускается таймер, который отслеживает сколько времени пользователь находится в данном состоянии

**Известные проблемы:** в настоящий момент выявленных проблем с данным таргетингом нет

## Источники

### utm_source,  utm_campaign, utm_medium

**Текущее состояние:** работает, есть особенности, зависящие от клиентского сайта

**Ожидаемый ввод:** ввод аналогичен тому, как в Google Analytics вы находите интересующие вас источники. Для каждого параметра источника: source, campaign, medium дано отдельное поле, в каждом из которых ввод проверяется по принципу точного соответствия. То есть код будет проверять, что в параметре источника будет стоять именно такое значение. При этом можно ограничиваться заполнением только одного из трех параметров. При этом, если вы заполняете два и более, то как и в общем случае будет проверяться, что все настроенные параметры равны параметрам пользователя, находящегося на сайте

**Принцип работы:** принцип работы нашего кода кардинально отличается в зависимости от наличия классической версии Google Analytics на сайте клиента
В случае наличия классической версии Google Analytics: классическая версия Google Analytics проставляет параметры источника трафика в cookie пользователю. Соответственно, в таком случае, мы используем те значения, которые были проставлены кодом Google Analytics и используем их
В случае наличия только Universal Analytics: Universal Analytics в отличие от классической версии Google Analytics не пишет никакой информации об источнике трафика в явном виде в cookie. Соответственно, в таком случае срабатывает наш собственный JavaScript код, который определяет источник, откуда перешел пользователь и для таргетинга используется то, что наш код определил.

**Известные проблемы:** в случае классической версии Google Analytics на текущий момент проблем не выявлено. Следующие проблемы имеют отношение только для клиентов, у которых на сайте отсутствует классическая версия Google Analytics и присутствует только Universal analytics:
Данный таргетинг крайне чувствителен ко времени загрузки нашего кода, так как в случае, если наш код не успел загрузиться на первой странице сессии пользователя, мы никаким образом не можем восстановить реальный источник перехода. Связанно с это с тем, что информация об источнике перехода пользователя на сайт хранится только на первой странице в сессии, будто то utm-метки или referrer, с которого приходил запрос на загрузку страницы. Таким образом, в случаях если у клиента стоит только Universal Analytics, и он хочет использовать таргетинг по источнику перехода, необходимо перемещение нашего пречекаут кода как можно выше в приоритете загрузки
Наш код практически в 100% случаев корректно распознает utm-метки из URL страницы. Однако с источниками, которые не проставляют utm-метки потенциально могут возникать проблемы, выраженные в том, что не весь трафик, приходящий с этого источника будет правильно таргетироваться. К таким источникам относятся следующие источники:
поисковые системы второго эшелона (то есть за исключением yandex и google, которые наш код распознает в большей части случаев)
реферральные источники (переходы по ссылкам размещенным, например, на блогах и других сайтах, без проставленных utm-меток)

### referrer

**Текущее состояние:** работает

**Ожидаемый ввод:** ввод предполагает фрагмент URL, при переходе с которого вы хотите показывать данную механику (например, это может быть babyblog.ru, если вы хотите таргетировать весь трафик приходящий с babyblog). По сути это альтернатива использованию таргетингов по источнику, реплицирующих параметры источников трафика в Google Analytics (см. выше). Важно отметить, что в случае, если у клиента стоит классическая версия Google Analytics, то предпочтительно использовать таргетинги, описанные выше. Если же стоит только universal analytics и необходимо таргетировать трафик с определенного сайта, то предпочтительно использование параметра referrer, т.к. он использует более широкий тип соответствия (а именно, ищет наличие приведенной текстовой строки в URL, с которого был переход).
Принцип работы: при переходе на сайт с другого сайта, наш код при загрузке записывает информацию о том, с какого URL был переход и использует его для проверки соответствия условиям запуска кампании

**Известные проблемы:** в зависимости от сайта, браузера, использования http или https информация о URL перехода, может быть недоступна, в таких случаях данный таргетинг может не срабатывать

### Посетивший url

**Текущее состояние:** работает

**Ожидаемый ввод:** фрагмент URL, страницу с которым пользователь должен был посетить в ходе своей сессии (например, если все страницы, связанные с корзиной у клиента содержат /basket/, и вы хотите таргетировтать пользователей, которые проходили через корзину в ходе текущей сессии, то в поле ввода необходимо указать /basket/ и вы добьетесь желаемого результата)

**Принцип работы:** в ходе сессии, мы записываем историю просмотров пользователя (т.е. страницы, на которых прогрузился наш код) и затем идет проверка на наличие заданного фрагмента хотя бы в одном из URL уже посещенных страниц
Известные проблемы: если среднее время пользователей на странице, по посещению которой мы хотим таргетировать пользователя, небольшое, необходимо добиться перемещения нашего кода вверх в порядке загрузки на страница, т.к. в противном случае мы не всегда будем записывать данную страницу в историю, если код на странице загружается поздно

## Дополнительные параметры

### spot

**Текущее состояние:** работает

**Ожидаемый ввод:** значение параметра spot, который клиент передает для запуска данной механики. В таком случае необходимо прописывать точно такое же значение, как и значение параметра "data-fl-spot" в коде интеграции (Часть 2 Раздела "Custom Mechanics" инструкции по интеграции)

**Принцип работы:** наш код проверяет наличие соответствующего кода на странице, отвечающего за передачу параметра spot. Если на странице есть код передачи данного параметра со значением, соответствующим настроенному в кампании, то данное условие считается выполненным, и в случае, если остальные условия запуска также были выполнены, кампания покажется

**Известные проблемы:** в настоящий момент проблем с данным триггером не выявлено

### С мобильного телефона

**Текущее состояние:** работает

**Ожидаемый ввод:** "да" - если таргетируются только пользователи с мобильных телефонов, "нет" - если пользователи с мобильных телефонов исключаются из таргетинга по данной кампании

**Принцип работы:** наш код при загрузке получает широкий спектр данных о пользовательском устройстве в момент загруки: тип браузера, его версия, операционная система, разрешение экрана и т.д. Один из параметров (user_agent) используется для определения типа устройства: мобильное или нет. При этом для установки соответствия между user_agent и типом устройства используется внешняя библиотека.
Известные проблемы: в библиотеке могут содержаться неверные данные относительно типа устройства для некоторых user_agent, однако процент ошибки должен быть крайне мал.

### Кастомное событие

**Текущее состояние:** работает

**Ожидаемый ввод:** название события которое клиент передает для запуска данной механики. В таком случае необходимо прописывать точно такое же значение, как и значение параметра "data-fl-event" в коде интеграции (Часть 1 Раздела "Custom Mechanics" инструкции по интеграции)

**Принцип работы:** когда клиент передает информацию о том, что событие наступило, кампания показывается, если все остальные условия группы выполнены. Если же по какой-то причине кампания показана не была, например, есть ограничение на минимальное время на сайте в рамках данной сессии, которое не было выполнено, то даже после выполнения этого условия кампания уже показана не будет. Так как наш код не сохраняет события произошедшие ранее в ходе сессии. Таким образом, желательно использовать события без дополнительных ограничений на время на сайте, либо на количество просмотренных страниц
**Известные проблемы:** в настоящий момент проблем с данным триггером не выявлено

### С установленной cookie

**Текущее состояние:** работает

**Ожидаемый ввод:** в поле, необходимо указать имя cookie, при наличии которой механика будет запускаться

**Принцип работы:** наш код проверяет какие cookie установлены у пользователя и в случае, если имя одной из них совпадает с тем, что указано в настройках кампании, и условие считается выполненным. Если это единственное условие показа кампании - кампания покажется. При этом, наш код не смотрит в то, что содержится в данной cookie. Соответственно, таргетинг по значению сохраненному в ней - невозможен

**Известные проблемы:** в настоящий момент проблем с данным триггером не выявлено

### В режиме инкогнито

**Текущее состояние:** работает

**Ожидаемый ввод:** выбор одного из двух вариантов "да" - если таргетируются только пользователи в режиме инкогнито и "нет" - пользователи не в режиме инкогнито
Принцип работы: для разных браузеров есть разные признаки, позволяющие определить, что используется инкогнито

**Известные проблемы:** на данный момент проблем нет, но так как сама идея инкогнито в том, чтобы со страницы этого понять было нельзя, можно ожидать, что какие-то признаки будут исчезать, и триггер может начать сбоить

### Прокрутка страницы (в процентах)
**Текущее состояние:** работает

**Ожидаемый ввод:** в поле "от" вводится минимальное значение прокрутки страницы в процентах, в поле "до" вводится максимальное значение прокрутки страницы. Значение 0 соответствует верхнему положению, когда верхняя граница окна браузера совпадает с верхней границей страницы. Значению 100 соответствует полной прокрутке страницы, когда нижняя граница окна браузера совпадает с нижней границей страницы.

**Принцип работы:** значение текущей позиции вычисляется по следующей формуле: [количество пикселов от верхней границы страницы до верхней границы окна] * 100 / ([высота страницы] - [высота окна браузера])

**Известные проблемы:** для большинства такой подход работает отлично, но могут быть страницы, на которых скролл работает не со странице, а с отдельным блоком. В таком случае при скролле двигается содержимое блока, а не содержимое страницы.
