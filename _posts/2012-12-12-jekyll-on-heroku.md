---
layout: post
title: Jekyll on Heroku
tags: ruby jekyll
---

##Hero... what?

After two years using [Jekyll][1] for this tiny blog, I am still on it and loving it. But I spent enough time managing a litlle vserver on which I deployed my blog using git hooks. Everything was working as expected. But managing a vserver, securing it and keeping it up to date is a real time sink. Many time I said to myself: you should be coding (and not upgrading your distro).

Some time ago, I decided to give [Heroku][2] a try. Damn! What they did with their platform is pure genius! Basically you can build and deploy your app using a simple [git][3] push command. They work using the abstraction of a dyno defined [as][4]:
> A dyno, the basic unit of composition on Heroku, is a lightweight container running a single user-specified command
So a dyno will take care of your web request or your background job or anything you throw at it. You want a more scalable app? Increase the number of dynos and you're done. Now the best part, Heroku offers you one dyno for free each month.

So I settled on the plan of deploying this Jekyll blog using the free Heroku dyno. As you can imagine, I am not the first having this idea, see:
* [Rack Jekyll][5]
* [Heroku custom buildpack][6]
* [Octopress][7]

... and many more I saw. These guys are all awesome and their projects are great. But what I had in mind for this blog deployment was to keep it simple, using this idea: I can generate a static site locally using the jekyll command, I just want to be able to upload this static site on Heroku. I don't see why the site generation should take place on Heroku. So I settle to upload a static site on Heroku and it was dead simple after reading [this][8].

##The 2min step-by-step ultimate guide for deploying a Jekyll site on Heroku without any pain

You don't like long titles?

* In the root of your site, add a Gemfile with

```ruby
source :rubygems
gem 'rack-contrib'
```

* Run

```bash
$ bundle install
```

* Add a config.ru file with
```ruby
require 'rack/contrib/try_static'

use Rack::TryStatic,
    :root => "_site",
    :urls => %w[/],
    :try => ['.html', 'index.html', '/index.html']

run lambda { [404, {'Content-Type' => 'text/html'}, ['Not Found']]}
```

* I like setting the exact command that will be run by the dyno, add a Procfile with

```bash
web: rackup -p $PORT
```

You're done!
How to test? Actually you have two ways of testing your config:
* Using foreman (which is part of the Heroku [toolbelt][9])

```bash
$ foreman start
```

* Using the rackup command

```bash
$ rackup -p 4000
```

Once everything is ok, deployment is a piece of cake, once you have [initialized][10] the app on Heroku:

```bash
$ git push heroku master
```

You're done! You can watch the result

```bash
$ heroku open
```

Tadaaaaaa! Cookies time!


[1]:http://jekyllrb.com/
[2]:http://www.heroku.com/
[3]:http://git-scm.com/
[4]:https://devcenter.heroku.com/articles/dynos
[5]:https://github.com/bry4n/rack-jekyll
[6]:https://github.com/mattmanning/heroku-buildpack-ruby-jekyll
[7]:http://octopress.org/
[8]:https://devcenter.heroku.com/articles/static-sites-ruby
[9]:https://toolbelt.heroku.com/
[10]:https://devcenter.heroku.com/articles/ruby
