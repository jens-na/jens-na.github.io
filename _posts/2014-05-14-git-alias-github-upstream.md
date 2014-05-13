---
layout: post
title: Git alias to add a GitHub upstream repo
---

{{ page.title }}
================
Maybe there are people who have a similar `alias` but I think this is
pretty useful for people who work with GitHub every day.

In my `~/.gitconfig` I have added the following alias:
{% highlight ini %}
[alias]
upstream = "!sh -c 'git remote add upstream \"https://github.com/$1.git\" && git fetch upstream' -"
{% endhighlight %}

Now you can use `git upstream user/repo` to add user/repo as upstream
repository.
