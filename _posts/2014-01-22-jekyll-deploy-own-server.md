---
layout: post
title: Deploy a Jekyll page with Travis to your own server
---

{{ page.title }}
================
Recently I wrote about [how awesome Jekyll, GitHub and Travis fits together][1].
Now, I couple of weeks later I made the decision to deploy my private Jekyll
based home page to my *own* server, because I like to use Jekyll with plugins. 

Jekyll provides a few [instructions][2] how you can submit the built page to your
own server but none of them suit my needs, which are:

- automatic deployment
- source code hosted at GitHub (no separate repository)
- test the page with Travis CI
- transfer the files with SFTP

So I decided to let Travis deploy my page with an `after_success` hook as
described [here][3].

![Deployment workflow](/images/deploy_workflow.png)

Gem travis-custom-deploy
========================

Instead of writing a bunch of shell commands I have have created
[travis-custom-deploy][4] which is capable of transferring files to another server. 
It is available at [RubyGems][5] so you can integrate it in your own ruby 
based projects.

You can setup travis-custom-deploy in only three steps:

#####1. add travis-custom-deploy to your Gemfile
{% highlight ruby %}
  gem "travis-custom-deploy"
{% endhighlight %}

#####2. setup environment variables
{% highlight bash %}
  gem install travis
  cd my_project
  travis encrypt DEPLOY_HOST="your-hostname.com" --add
  travis encrypt DEPLOY_USERNAME="username" --add
  travis encrypt DEPLOY_PASSWORD="password" --add
  travis encrypt DEPLOY_REMOTEDIR="/path/to/deploy/" --add
{% endhighlight %}
Take a look [here][6] if you haven't heard of secure environment variables with
Travis yet.

#####3. add an after_success hook
{% highlight yaml %}
  after_success: bundle exec travis-custom-deploy sftp service:jekyll
                                                   ^     ^
                                                   |     |
                          transfer with SFTP ------+     |
                                                         |
                          deploy a jekyll page ----------+
                                                    
{% endhighlight %}
This setup will automatically transfer `_site/` to your remote server via SFTP if the
tests passed successfully! 

Live example
============
I use it for this page. \\
You can take a look at my [Gemfile][7] and my
[`.travis.yml`][8] to see my setup.

[1]: http://jens-na.de/2013/12/05/jekyll-github-travis-perfect-fit/
[2]: http://jekyllrb.com/docs/deployment-methods/
[3]: http://docs.travis-ci.com/user/deployment/custom/
[4]: https://github.com/jens-na/travis-custom-deploy
[5]: http://rubygems.org/gems/travis-custom-deploy
[6]: http://docs.travis-ci.com/user/build-configuration/#Secure-environment-variables
[7]: https://github.com/jens-na/jens-na.github.io/blob/master/Gemfile
[8]: https://github.com/jens-na/jens-na.github.io/blob/master/.travis.yml
