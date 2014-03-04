---
layout: post
title: "Ruby Quick Tip: #tap"
category: dev
tags: [ruby]
published: false
---

# Introducing Ruby Quick Tips

In these series of Ruby Quick Tips, I will try to post all those small tips I use in Ruby to improve code readability or productivity. Use them smartly, it's not because you discover a hammer that you *need* to see nails everywhere.

# Tap, tap tap! Who's there?

In today quick tip, I present you one of my favourite functions: M. [#tap](http://ruby-doc.org/core-2.1.1/Object.html#method-i-tap).

Reading its documentation, it may appear to you as a really simple method:

> Yields x to the block, and then returns x.

Simple right? Well what does that mean? It means do you can call `tap` on any Ruby object. It means that you can give a block to this call. This block will receive your Ruby object. The deal is: you can do whatever you want with your object, even modify it, `tap` will return it, no matter what.

Is that useful?

# Examples

Oh I see you over there! You look suspicious! You need examples to understand and to see the *true* power of `tap`.

## Method chaining
Let's say you have a `String` and you need to chain some methods on it:

```ruby
x = "Foobar!"
x.tap{ |e| e.pop }
x.tap(&:pop)
```

## Bang! and return
Example String
## Returning a complex object
Example [{} with optional fields]
