---
layout: doc
title:  "FAQ"
date:   2017-02-08 16:34:18 +0300
section: general
---
### How to check whether the email was already registered on site immediately inside a Flocktory widget?
To have this kind of functionality we need the client requesting to:

* Implement an HTTP interface to receive requests containing an obligatory `email` parameter
* The interface shoud return either `{status: 'new'}` or `{status: 'exists'}`
* The interface should have CORS headers set up accordingly (e.g., `Access-Control-Allow-Origin: *`)
