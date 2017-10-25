---
layout: doc
title:  "FAQ"
date:   2017-02-08 16:34:18 +0300
section: general
---
### How to check whether the email was already registered on site immediately inside a Flocktory widget?
To have this kind of functionality we need the client requesting to:

* Implement an HTTP interface to receive requests containing an obligatory `email` parameter
* The interface shoud return either `{"status":"new"}` or `{"status":"exists"}`
* The interface should have CORS headers set up accordingly (e.g., `Access-Control-Allow-Origin: *`)


### How to pass additional data to Flocktory
A situation may arise when you would want to use additional customer data that only the client shop may have, for example,
* priority card type
* client RFM segment
* any other data not supported by Flocktory by default

The way of doing this is by using this bit of JS
```
window.flocktory = window.flocktory || [];
window.flocktory.push(['attachToProfile', {
    data: {
        "parameter1": "value1",
        "parameter2": "value2"
    }
}]);
```
Then the customer may be targeted In a segment 
![](https://assets.flocktory.com/uploads/clients/1833/f8f6ddd1-796d-473e-bfe9-7f040672b39f_seg.png)
