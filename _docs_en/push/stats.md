---
layout: doc
title:  "Статистика push виджетов"
section: push
---

| event | single event per user | description |
|:-----:|:---------------------:|:------------|
| show-widget | no |  Event when conditions of the opt-in campaign have been met by a supported browser and the widget has been shown to user |
| push-unsupported |  no |  Is sent every time user visits a page using a browser not supporting push or using incognito feature and campaign conditions are satisfied |
| push-subscribed | yes | When user subscribes to push inside the system dialog |
| push-denied | yes | When user blocks push inside the system dialog |
| push-already-subscribed | no |   |
| Each | time | when campaign's triggers passed, but user has already subscribed. |
| push-already-denied | no |  Each time when campaign's triggers passed, but user has already blocked notifications. |


Also when using precheckout widgets to get the first step agreement, you should expect to see such events as 

* test-passed - if you have an A/B test between different push subsription widgets, you should see this event when the dice is rolled and a variation to be shown is chosen. This event is sent once per user per A/B campaign.
* click_yes / click_no / close / click_allow_button / click_block_button  etc. - sent when user interacts with a widget according to the settings in the campaign. May vary depending on the widget used.
