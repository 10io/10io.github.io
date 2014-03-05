---
layout: post
title: "Ruby Quick Tip: #tap"
tags: [ruby, "quick tip"]
---

In these series of Ruby Quick Tips, I will try to post all the small tips I use in Ruby to improve code readability or productivity.
Use them in a smart way: it's not because you discovered a hammer that you *need* to see nails everywhere.
In today quick tip, I present you one of my favourite functions: introducing mister  [#tap](http://ruby-doc.org/core-2.1.1/Object.html#method-i-tap). Applause.

# Tap, tap tap! Who's there?



Reading its documentation, it may appear to you as a really simple method:

> Yields x to the block, and then returns x.

Easy right? Well what does that mean? It means that you can call `tap` on any Ruby object. It means that you can give a block to this call. This block will receive your Ruby object. The deal is: you can do whatever you want with your object, even modify it, `tap` will return it, no matter what you do with your object.

Is that useful?

# Examples

Oh I see you, over there! You look suspicious! You need examples to understand and see the *true* power of `tap`.

Ok, here we go. These are really simple examples to help you to grasp `tap`.

## Method chaining
Let's say you have an awesome function which process urls as strings:

```ruby
# Process urls
# process("http://www.example.com/page1") -> "www.example.com"
def process(url)
  result = url.split("/")
  result = result.reject(&:empty?)
  result.pop
  result.shift
  result.first
end
```

The problem here comes from [pop](http://www.ruby-doc.org/core-2.1.0/Array.html#method-i-pop) and [shift](http://www.ruby-doc.org/core-2.1.0/Array.html#method-i-shift). These methods modifies the `Array` instance without returning it. So it seems that we can't chain all the methods.

`tap` to the rescue:

```ruby
# Process urls
# process("http://www.example.com/page1") -> "www.example.com"
def process(url)
  url.split("/")
     .reject(&:empty?)
     .tap(&:pop)
     .tap(&:shift)
     .first
end
```

This is more succinct and more readable. We reduced *code noise* by reducing the use of variables. How great is that?

## Bang! and return
Now let's say that in your Rails application you have a `User` model. In a function, you want to update it and save it at the same time, using the [bang!](http://api.rubyonrails.org/classes/ActiveRecord/Persistence.html#method-i-save-21) version of save. Your code could look like this:

```ruby
def update_user_first_name(user, first_name)
  user.first_name = first_name
  user.save!
  user
end
```

`save!` will modify the user and return `nil` if no error occurs. But on the next line, you clearly want to return the user. This could be solved using `tap`:

```ruby
def update_user_first_name(user, first_name)
  user.first_name = first_name
  user.tap(&:save!)
end
```

## Create a collection and return it
Sometimes you want to return a hash from one of your functions with optional keys. Let's say you have a function `user_options` returning a hash with some keys depending on the given user.

```ruby
def user_options(user)
  result = {
    :first_name => user.first_name,
    :last_name => user.last_name
  }
  result[:spam_count] = user.spam_count if user.is_mail_admin?
  result[:inactive_users] = user.inactive_users if user.is_user_admin?
  result
end
```

It works great but let's see how we can improve readability using `tap`:

```ruby
def user_options(user)
  {}.tap do |result|
    result[:first_name] = user.first_name
    result[:last_name] = user.last_name
    result.merge!(:spam_count => user.spam_count) if user.is_mail_admin?
    result.merge!(:inactive_users => user.inactive_users) if user.is_user_admin?
  end
end
```

This version is more compact and you can read it more easily.

I can hear you: these are dummy and stupid examples! Give me the real deal!

Ok, fair enough!

## Custom behavior with Strong Parameters
This example comes from a Rails real [issue](https://github.com/rails/rails/issues/9454).

Strong Parameters in Rails 4 needs you to list all the whitelisted keys of the params hash. The problem is that if one of these keys is a nested hash containing arbitrary data, you may need to whitelist it as well.

Let's say you receive this as params:

```ruby
{
  :product => {
    :name => "product one",
    :data => { :color => :green, :width => 133 }
  }
}
```

Let's say that `:data` can contain any key. Using Strong Parameters, you can whitelist `:product`, `:name` and `:data`. The problem comes from whitelisting `:data` contents. You don't know all the keys in advance, so you can't write something like:

```ruby
def product_params
  params.require(:product).permit(:name, :data => {})
end
```

This will give you an empty hash for `:data`.

Don't worry! You can count on your trusty `tap`.

```ruby
def product_params
  params.require(:production).permit(:name).tap do |whitelisted|
    whitelisted[:data] = params[:product][:data]
  end
end
```

(Credits to [fxn](https://github.com/fxn))

Using `tap`, we modify the whitelisted hash outputted by Strong Parameters and we return it. There is no need to have an auxiliary variable or something else. Again less *code noise* = code more readable.


# Conclusion

I hope that after reading this, you discovered the little gem that is `tap`.

Try to use it in your code, you might get wonders!

Happy coding!
