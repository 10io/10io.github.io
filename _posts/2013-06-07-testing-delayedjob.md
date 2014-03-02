---
layout: post
title: "Testing DelayedJob"
category: dev
tags: [ruby rails]
published: false
---

Almost any serious web app has background jobs. The main advantage here is to alleviate the processing of the incoming web requests by off-loading cpu intensive tasks. Such tasks can be:
* Send emails in bulk.
* Transform or generate images.
* Generate PDFs.

The idea here is that your users request a heavy task and on the server side, you delegate this task to a background job system and you return a nice "in progress..." view to your users. This way, your app is not slowed down by processing a heavy task.

In the Rails world, you have many alternatives for such background jobs. One of them is [DelayedJob](https://github.com/collectiveidea/delayed_job). This system was extracted and open sourced from [Shopify](http://www.shopify.com/).

#My beautiful job
Let's say you have your awesome job