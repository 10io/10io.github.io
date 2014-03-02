---
layout: post
title: Rails can't scale
tags: ruby
category: dev
---

A few weeks ago, I was presenting a web app to some folks. At the end of the presentation, one random guy showed up and asked:
Great app! What do you use? Java?

I said: No, it's built with Ruby on Rails.

He said: Ah yes, Rails. What a shame that Rails can't scale.

I didn't know what to answer or what to do: should I ignore the answer, laugh, or run.

##Oh really?

I love when people take optimisation to a very high level. Even before actually having a working app, they are thinking about scaling, speed, and maybe concurrency.

It's a pity that people have such difficulties. At application creation stage, they focus on minor problems instead of core problems. They overthink about problems. Pragmatism is such a rare value.

I understand that you can build web apps using many different frameworks, and that's great. But when people fight over scaling or concurrency problems before having a working prototype, they really hurt themselves.

Here is my personal points of view:

* Scaling problems are not only induced by web frameworks. Many factors have to be taken into account: the framework, but also software architecture, hardware, and such.
* If you are facing scaling problems, congratulations! You are probably having a nice amount of traffic in your web application.
* Dealing scaling or concurrency problems before any working prototype is a useless task.
* Rails scale nicely.

    Ah yes! The Twitter argument. If you have the same problems as Twitter, this means that you are in the worldwide top 10 of all web applications. Congratulations, you should have enough money to hire one engineer or two to look at your scaling problems.

* Productivity and community > scaling factor. Any day.

    The random guy presented me his app developed in Scala. I inquired about the community, is it easy to work with and to have nice answers? It turns out that you can easily waste a massive amount of time looking for answers.

    This doesn't happen with Rails. I have yet to encounter a very hard issue that the community can't help to solve.

* I hate fan boys. Web development has many tools. Don't try to choose one for all your problems. Choose the one most suited for your problem.

    Example: next time I want to develop a simple web app quickly, I'll take a look to Sinatra. Rails is not one tool to rule them all.

* I definitely will buy a "Rails can't scale" tshirt.

So please, next time, you are going to say: X can't scale, think about it twice.
