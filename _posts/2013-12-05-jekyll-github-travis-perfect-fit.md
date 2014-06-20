---
layout: post
title: Jekyll, GitHub, Travis - perfect fit
---

Recently I was looking for some alternatives to build a new homepage. My old homepage was
static HTML and I loved it very much but writing blog posts in HTML isn't much fun. So I
decided to search the world wide web for some alternatives and I found [Jekyll][1].

Jekyll is a tool written in Ruby by one of the founder of GitHub. It turns your plain text 
or markdown files into static HTML pages. Jekyll is also blog-aware which means you can 
have permalinks, categories, layouts, etc. You can write all your blog posts in markdown 
so you don't need to store them in a database, isn't that awesome?!

Give it a try and install it on your computer and setup a new page in only three steps:

{% highlight bash %}
    $ gem install jekyll
    $ jekyll new mysite
    $ cd mysite
    $ jekyll serve -w
{% endhighlight %}

**It's getting even better!** GitHub loves Jekyll too, some of the GitHub pages are build with it.
GitHub has a special support for Jekyll users named [GitHub Pages][2]. If you create a repository 
with the syntax &lt;username&gt;.github.io GitHub will automatically build the Jekyll page and 
publish it. You only need to push your changes to this repository. This is a very easy way to host 
your own page. Afterwards your page is available at http://&lt;username&gt;.github.io/

If you own a domain like 'jens-na.de' you can even point a DNS CNAME record to
GitHub pages (`jens-na.github.io`). Afterwards you need to create a file named `CNAME` in your
repository. Put your domain or subdomain in this file:

{% highlight bash %}
    www.jens-na.de
{% endhighlight %}

After a while your Jekyll page is available at http://www.jens-na.de!

You can find information about how to setup GitHub pages and DNS [here][3]. If
you are not very familar with editing DNS entries you should definitly take a
look at [this blog entry from Caleb][4]. 

Testing
=======
It is always good to have tests, right? If you want to test your Jekyll page you 
can use [Travis-CI][5], a continious integration service for open source projects. You need to
add &lt;username&gt;.github.io as a repository which should be tested with Travis. For a more
detailed instruction take a look [here][6]. 

You need a `.travis.yml` if you want to use Travis-CI. My file looks like
that:

{% highlight yaml %}
    language: ruby
    rvm:
      2.0.0
    script:
      - bundle exec jekyll build --trace
      - bundle exec ./script/htmlcheck
{% endhighlight %}

If you haven't already noticed a script named `./script/htmlcheck` is called:

{% highlight ruby %}
    #! /usr/bin/env ruby
    require 'html/proofer'
    HTML::Proofer.new('./_site').run
{% endhighlight %}

This tiny script [checks][7] if all the links are sane and don't end up in a 404.
If the build fails you will receive an email, this way you always know if 1) your Jekyll 
page was built and 2) all links are OK.

Check out Jekyll, GitHub and Travis. It really rocks! If you need some examples or inspiration
you should take a look at [this page][8].

[1]: http://jekyllrb.com/
[2]: http://pages.github.com/
[3]: https://help.github.com/articles/setting-up-a-custom-domain-with-pages
[4]: http://imakewebthings.com/blog/github-pages-email/
[5]: https://travis-ci.org/
[6]: http://about.travis-ci.org/docs/user/getting-started/
[7]: https://github.com/gjtorikian/html-proofer
[8]: https://github.com/mojombo/jekyll/wiki/sites 
