---
layout: doc
title:  "FAQ"
date:   2017-02-08 16:34:18 +0300
section: general
---
### Что нужно, чтобы виджет Flocktory мог проверить, есть ли введенный емейл в клиентской базе
На стороне клиента потребуется:

* Создать HTTP endpoint, принимающий на вход обязательный параметр `email`
* Endpoint должен отвечать либо `{"status": "new"}` , либо `{"status": "exists"}`
* CORS-заголовки должны быть настроены соответствущим образом (например, `Access-Control-Allow-Origin: *`)


### Как передавать Flocktory дополнительные параметры пользователя
Данный способ позволяет передавать и впоследствии использовать при сегментации параметры пользователя, которые специфичны для данного магазина, например:
* уровень карты программы лояльности
* собственный рфм сегмент
* любой другой параметр, не поддерживаемый Flocktory по умолчанию

Для передачи необходимых параметров используйте следующий код при посещении пользователем любой страницы 
```
window.flocktory = window.flocktory || [];
window.flocktory.push(['attachToProfile', {
    data: {
        "parameter1": "value1",
        "parameter2": "value2"
    }
}]);
```
Затем пользователи с нужным параметром можно старгетирвать через инетфейс сегментов
![](https://assets.flocktory.com/uploads/clients/1833/f8f6ddd1-796d-473e-bfe9-7f040672b39f_seg.png)
