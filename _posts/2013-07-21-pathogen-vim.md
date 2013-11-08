---
layout: post
title: Vim&#58; How to use pathogen.vim
---

{{ page.title }}
================
[pathogen.vim](http://github.com/tpope/vim-pathogen) is a plugin for 
the Vim editor where you can manage your runtime path in a very easy 
way. This makes the installation of other Vim plugins easier.

Benefit
-------
As you can see in the picture below with pathogen.vim you have an 
additional directory named `bundle/` where you install all your custom 
Vim plugins. This makes the `~/.vim/` directory clearer and helps you 
installing, updating and deleting other plugins.

![vim-pathogen directory structore](/images/vim-pathogen-1.png)

Installation
------------
The installation of pathogen.vim is super easy. You simply have to create 
the bundle/ directory and install the file pathogen.vim in the autoload 
folder of your `~/.vim/` directory. If you are using Windows and you like 
to install pathogen you have to install it in your `~\vimfiles` directory. 
This piece of code is copy-pasted from the pathogens README and does 
everything what is neccessary to install pathogen on your system:

{% highlight bash %}
    mkdir -p ~/.vim/autoload ~/.vim/bundle; \
    curl -Sso ~/.vim/autoload/pathogen.vim \
    https://raw.github.com/tpope/vim-pathogen/master/autoload/pathogen.vim
{% endhighlight %}

You can simply copy those three lines and paste them into your terminal. 
If you have installed pathogen successfully you have to include the 
following line into your `~/.vimrc` (Windows: `~\_vimrc`):

    execute pathogen#infect()

Installing a custom plugin
--------------------------
To install a custom plugin you can clone a repository directly into your 
new bundle/ directory. To install NERDTree (a very powerful and useful file 
browser for the Vim editor) you can do it like that:

{% highlight bash %}
    cd ~/.vim/bundle
    git clone "https://github.com/scrooloose/nerdtree.git"
{% endhighlight %}

Removing a custom plugin
------------------------
If you like to remove a custom plugin because you have found a better one 
you can remove the directory of your custom plugin. That's it!

References
==========
 * [README.md](https://github.com/tpope/vim-pathogen/blob/master/README.markdown) 
   of pathogen.vim
