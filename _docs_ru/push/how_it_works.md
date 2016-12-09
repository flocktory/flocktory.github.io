---
layout: doc
title:  "Как это работает"
section: push
---

## Obtaining campaigns

Push campaigns work similar to prechekout campaigns. When JSAPI requires prechekout-campaigns server respond with list of campaigns. If browser satisfy requirements and current user included in segments then list contain push campaigns. If some campaign satisfy its triggers and mutual exclusions it create widget.

Two ways are possible. First – when partner site is secure (https), second in other case (http).

## Requesting permissions on secure site

This step depends on browser. Also before show native window to user widget can show some visual element to user, but it isn't necessary.

### Safari

Safari allow JSAPI to check push status (subscribed, denied, or nothing). If user doesn't block or allow notifications widget make request to our server. Our server respond with push package. This package contain pictures, and title. If package correct then browser show user native window with question. If user agree with notifications then browser send request to our server. This request contain browser token, user session and some additional info. All this data allow us to identify user and send him a message later.

### Others

Widget start service worker. Widget can check if user has permissions. If widget can subscribe user it ask for Notification permissions and browser show user native window. If user agree browser return to JSAPI user token. JSAPI send this token + session to our servers. This data allow us to send notification to user later. 

## Requesting permissions on insecure site

### Safari

Works same way as on secure age.

### Chrome

Specs don't allow browsers to request permissions on secure pages. We have workaround for it. In this case we always show user visual element and user should click it. When clicked widget open special https page. Typically this is a partner domain with alias to our server. JSAPI create this window and communicate with it by postMessage API. Inside this window script ask user permissions and send result to JSAPI. Then JSAPI send data to our server.

This way has one issue: widget can ask browser about subscription only on second step page. This mean it should show visual element even if user allowed or denied pushs. We use small workaround: before showing visual element JSAPI ask server about subscription to check user status. It doesn't work when user clean his cookies. In this case widget show visual element to user, user click it, JSAPI opens second step window and immediately close it. 

