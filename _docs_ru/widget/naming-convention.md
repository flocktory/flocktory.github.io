---
layout: doc
title:  "Соглашения по именованию"
section: widget
order: 4
---

### Именование шагов в виджете

Шаг со сбором электронной почты, имени и т. д. – **login**

```html
<div class="widget__screen" data-fl-screen="login">
  <!-- Содержимое -->
</div>
```

<br>

Шаг с пользовательским соглашением – **terms**

```html
<div class="widget__screen" data-fl-screen="terms">
  <!-- Содержимое -->
</div>
```

<br>

Шаг c мотивацией за совершение действия – **reward**

```html
<div class="widget__screen" data-fl-screen="reward">
  <!-- Содержимое -->
</div>
```

<br>

Шаг с экраном спасибо – **success**

```html
<div class="widget__screen" data-fl-screen="success">
  <!-- Содержимое -->
</div>
```

<br>

Если в виджете есть другие шаги кроме условий акции, пользовательского соглашения, шага спасибо, сбора электронной почты или шага с мотивацией, то
они именуются как **screen1**, **screen2** и т. д. в зависимости от номера в цепочке показов.
Если в виджете несколько шагов, которые предшествуют screen-…, то он будет нумероваться  не с 1. Например: **login**, **reward**,  **screen3**


Если в виджете только один шаг, его никак отмечать не нужно.

<br>

### Именование треккинга на крестике закрытия

Треккинг крестика закрытия должен говорить о том, на каком шаге виджет был закрыт.

Формат:``` data-fl-track="click-close-screenName" ```


**Например:**

Трекинг закрытия шага – **спасибо**
```html
<div class="widget__screen" data-fl-screen="success">
  <div class="widget__close" data-fl-close="1800" data-fl-track="click-close-success">
  <!-- Содержимое -->
</div>
```


Трекинг закрытия шага – **пользовательское соглашение**
```html
<div class="widget__screen" data-fl-screen="success">
  <div class="widget__close" data-fl-close="1800" data-fl-track="click-close-success">
  <!-- Содержимое -->
</div>
```

И т. д.

Если в виджете только один шаг, для него явно указывать треккинг не нужно, достаточно конструкции data-fl-close.

Виджет с одним экраном:

```html
<div class="widget__screen">
  <div class="widget__close" data-fl-close="1800">
  <!-- Содержимое -->
</div>
```

<br>


### Именование событий в виджетах


#### Виджеты с ссылками
Клик на ссылку, которая ведет на др страницу - реальный урл (в случае, если эта ссылка единственная на виджете и это не условия на пользовательское соглашение) – **click-link**

```html
<div class="widget__screen">
  <a href="http://prettysite.org/some-article" data-fl-track="click-link">Перейти</a>
</div>
```


Если ссылок несколько, то называем click-link-…  в зависимости от смысла ссылки. Например, если на виджете 2 ссылки "косметика" и "аксессуары" , то называем **click-link-cosmetics**, **click-link-accessories**
```html
<div class="widget__screen">
  <a href="http://prettysite.org/cosmetcis" data-fl-track="click-link-cosmetics" target="_blank">Косметика</a>
  и
  <a href="http://prettysite.org/cosmetcis" data-fl-track="click-link-accessories" target="_blank">Аксессуары</a>
</div>
```

<br>

#### Виджеты с кнопками
Клик на кнопку, которая не попадает под правило для ссылок (например, не ведет на реальный урл, а переключает виджет между состояниями) и не является пользовательским соглашением – **click-button**


Если кнопок несколько, то называем click-button-…  в зависимости от смысла кнопки. Например, если на виджете 2 кнопки "да" и "нет" , то называем click-button-yes, click-button-no.
```html
<div class="widget__screen">
  <h2>Хотите получить подарок?</h2>
  <div>
    <button data-fl-track="click-button-yes">Да</button>
    <button data-fl-track="click-button-no">Неи</button>
  </div>
</div>
```

<br>

#### Виджеты, где есть копирование купона

Кнопка скопировать купон – **copy-coupon**
```html
<div class="widget__screen">
  <div>
    <button data-fl-track="copy-coupon">Скопировать</button>
  </div>
</div>
```

<br>

#### Виджеты, собирающие отзывы

Кнопка отправки отзыва или результата опроса – **click-send-feedback**
```html
<div class="widget__screen">
  <form>
    <button data-fl-track="click-send-feedback" type="submit">Оставить отзыв</button>
  </form>
</div>
```

<br>

#### Виджеты MGM

Кнопки социальных сетей в виджета МГМ – **click-share-fb (vk, fb, ok, tw)**
```html
<div class="widget__screen">
  <button data-fl-track="click-share-vk">VK</button>
  <button data-fl-track="click-share-fb">Facebook</button>
  <button data-fl-track="click-share-ok">OK</button>
  <button data-fl-track="click-share-tw">Twitter</button>
</div>
```

Кнопка отправки формы "Поделись с другом через email" в МГМ – **click-share-email**
```html
<div class="widget__screen">
  <input type="text" placeholder="email друга">
  <textarea></textarea>
  <button type="submit" data-fl-track="click-share-email">Поделись с другом</button>
</div>
```

Копирование урла на МГМ, чтобы поделиться – **click-copy-url**
```html
<div class="widget__screen">
  <div>http://share.flocktory.com/dsfoWsdfodI</div>
  <button data-fl-track="click-copy-url">Скопировать ссылку</button>
</div>
```

<br>

#### Виджеты просмотренных товаров

Клик на виджет в свернутом состоянии – **сlick-widget**
```html
<div class="widget__screen">
  <div data-fl-track="click-widget">Развернуть</div>
</div>
```

Клик на карточку товара на виджете  (схлопываем, на click-product 1,2,3… не делим) – **click-product**
```html
<div class="widget__screen">
  <div data-fl-track="click-product">Товар 1</div>
  <div data-fl-track="click-product">Товар 2</div>
</div>
```

