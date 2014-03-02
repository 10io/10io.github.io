---
layout: post
title: GMail SMTP settings for Rails 3.2
tags: ruby rails
category: dev
---

Here is a quick tip for correctly configuring Rails Mailers to use with a GMail account with a custom domain name.

In development.rb

{% highlight ruby %}
config.action_mailer.raise_delivery_errors = true
config.action_mailer.delivery_method = :smtp
config.action_mailer.smtp_settings = {
  :address              => "smtp.gmail.com",
  :port                 => 587,
  :domain               => "mydomain.com",
  :user_name            => "myuser@mydomain.com",
  :password             => "mypassword",
  :authentication       => "plain",
  :enable_starttls_auto => true
}
config.action_mailer.default_url_options = { :host => 'localhost' }
{% endhighlight %}

In test.rb

{% highlight ruby %}
config.action_mailer.default_url_options = { :host => 'dummy' }
{% endhighlight %}

In production.rb

{% highlight ruby %}
config.action_mailer.raise_delivery_errors = true
config.action_mailer.delivery_method = :smtp
config.action_mailer.smtp_settings = {
  :address              => "smtp.gmail.com",
  :port                 => 587,
  :domain               => "mydomain.com",
  :user_name            => "myuser@mydomain.com",
  :password             => "mypassword",
  :authentication       => "plain",
  :enable_starttls_auto => true
}
config.action_mailer.default_url_options = { :host => 'www.mydomain.com' }
{% endhighlight %}

Take note that you still need a host for the test environment.
