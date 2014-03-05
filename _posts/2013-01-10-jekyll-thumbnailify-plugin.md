---
layout: post
title: Jekyll thumbnailify plugin
tags: ruby
---

##Presenting the thumbnailify plugin!
Let's say you have a beautiful static site powered by [Jekyll](http://jekyllrb.com/), and then you would like to add some pictures to your posts or pages. Let me introduce you the Jekyll thumbnailify plugin! Tadaaaa!

The basic idea behind this plugin is quite simple: it's a new Liquid tag that will take an image filename as a parameter. During the site creation, the plugin will take this image, reduce it to make a thumbnail version and then render this kind of html:

```html
<a class="image" href="/images/foobar.png"><img src="/images/foobar_t.png" /></a>
```

So basically, it's an image link to your image. Your post/page will have the thumbnail version of the image and when you click on it, you go to the original version of the image.

Then, you can add some JS goodness like [fancyBox 2](http://fancyapps.com/fancybox/) to produce a zoom effect!

The awesome demo:

![sample](/assets/images/posts/sample.jpg)

Image from [~Eireen-stock](http://eireen-stock.deviantart.com/).

##Source, installation and cookie
See the repository on [github](https://github.com/10io/jekyll-thumbnailify).

As for the cookie.

Well, there is no cookie.
