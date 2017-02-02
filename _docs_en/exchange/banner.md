---
layout: doc
title:  "Exchange баннер"
section: exchange
---

## Описание
Эксченж баннеры встраиваются на страницы партнёров внутри iframe. Баннер представляет из себя статичную (обычно) вёрстку.
Перед отрисовкой на странице содержимое всего `body` баннера будет обёрнуто в ссылку.
Поэтому крайне **важно** не делать внутри виджета ссылок, он всё равно будет работать как одна большая ссылка.

## API баннера
Баннер может устанавливать свою ширину с помощью атрибута `data-width` на `body`. Значение может быть либо `100%` - занимает всю ширинут, либо `Xpx` - - занимает X пикселов в ширину. Примеры:

```
<body data-width="500px"> ... </body>
<body data-width="100%"> ... </body>
```

Высоту баннера можно устанавливать через css-свойство `height` для тега `body`. Пример: Высота должна быть `300px`
```
<style>
  body {
    height: 300px;
  }
</style>
```

## Добавление баннера в кампанию
Для создания баннера нужно зайти в кабинет по ссылке https://cabinet.flocktory.com/sites/1781/exchange/banners/ , заменив 1781 на ваш ID. Нажимаем кнопку "Добавить баннер"

![Add banner](https://assets.flocktory.com/uploads/clients/1791/7a4a82fe-5a48-42b8-968e-32921e8ffb9f_D9D335E80BEE4B8A6F10F6CDA265F80E.png)

Выбираем любой дефолтный баннер

![Choose default](https://assets.flocktory.com/uploads/clients/1791/85724d1d-6e55-48f6-8dc1-93e14a3ddc7b_84608641803398E6B2903C5ACDBCE325.png)

Меняем код на необходимый, и сохраняем баннер

![Save banner](https://assets.flocktory.com/uploads/clients/1791/c1b597dc-9dcd-4f39-bf8a-238c162c005b_43F6AAF11A505FF5E59172EDF67AC019.png)

Включаем созданный баннер и выключаем остальные

![Enable banner](https://assets.flocktory.com/uploads/clients/1791/9b3d2faa-f47a-43b8-8db9-9a482aece198_63FC1ECA0C3BBF4AA041635485546151.png)

Баннер отобразиться на демошопе <http://1781.demoshop.flocktory.com/> (надо заменить 1781 на ваш ID) под кнопкой submit

![Demoshop](https://assets.flocktory.com/uploads/clients/1791/f15a73e4-7a84-4238-bfd8-176a6ad33024_3465F48706EC672DF0DA2481C39ADE43.png)
