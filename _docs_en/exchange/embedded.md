---
layout: doc
title:  "Embedded"
section: exchange
---

If you need embedded put this on site.

```html
<iframe src="https://flocktory.com/interchange/login?ssid=SITE_ID&bid=BID&email=EMAIL&name=NAME">
</iframe>
```

Variables
* SITE_ID — site id in flocktory
* EMAIL — user email. If unknown use **xname@flocktory.com**
* NAME — user name (optional)
* BID — banner number (optional)

Example

```html
<iframe src="https://flocktory.com/interchange/login?ssid=1234&email=user@gmail.com&name=Mike">
</iframe>
```

## Height correction

After iframe loaded it sends postMessage

```javascript
{ type: 'resize', iframeHeight: 1000 }
```
where iframeHeight — height of iframe (pixels).
It's important to set outer height to this value.

## Scroll correction

Any time time user scroll page, send postMessage to iframe.

```javascript
document.querySelector('iframe').contentWindow.postMessage({ scrollTop: 1000 }, '*');
```

`scrollTop` value should be sum of page scroll and delta. Delta depends on site.
Best way to find delta is to check on site.
