---
layout: doc
title:  "Push"
section: integration
---

Attention! In order for the module to function correctly you should integrate [Pre-Checkout]({{ site.baseurl }}{% link _docs_en/integration/precheckout.md %}) и [Post-Checkout]({{ site.baseurl }}{% link _docs_en/integration/postcheckout.md %}) as well as [main integration]({{ site.baseurl }}{% link _docs_en/integration/general.md %})


For the successful integration you need to have the rights and access to do the following things:

* Add/edit DNS records of your domain in order to create a new CNAME record
* Upload provided files into the root folder of your domain(s)


## Step 1: Create a CNAME record for your domain in the DNS settings
In order to create the record you have to do the following:

1.	Login to your DNS manager
2.	Create a new CNAME record
3.	Enter the following information into the record:
    * Host: push
    * Points to: push.flocktory.com
    * TTL: 1 day
4.	Save the settings and deploy the changes.

### Please mind:
* The CNAME DNS cannot be changed for the users that have already subscribed.
* Flocktory strongly recommends you to use “push” name record. In this case your notifications will be sent from  push.yourdomain.com

### How to verify?
In order to verify the correct setup of CNAME you can go to [whatsmydns](https://www.whatsmydns.net) and type in push.yourdomain.ru and verify that the server is linked to push.flocktory.com

## Step 2: Upload manifest.json and flock_push_worker.js into the root folder of your domain

* Receive the two files (manifest.json and flock_push_worker.js) from your Flocktory account manager (or download them from your cabinet in the PushReward settings)
* Upload the two files to the root folder of your domain. The files must be accessible from the root


#### Please mind:

* Files may may be places only in the root if your site. This is a technological limitation. More info: [https://www.w3.org/TR/appmanifest/](https://www.w3.org/TR/appmanifest/)
* Please assure that the two files are uploaded to your main domain (www.) but also to any other subdomain that you want to use to offer push notifications to your users (for example your mobile site m.yourdomain.com)

#### How to verify?

* In order to verify the correct installation of the provided files you can easily go to your browser and type in the file locations: http://yourdomain.com/manifest.json and http://yourdomain.com/flock_push_worker.js In both cases you should see some texts appear on your screen (and no error message).
* Besides this part you need to make sure you have complete the Main Integration and also the Precheckout part for additional functionality.

Congratulations! You have integrated the Flocktory module PushReward! Now you can continue with the setup of your first opt-in scenario together with your Flocktory account manager and launch the module.
