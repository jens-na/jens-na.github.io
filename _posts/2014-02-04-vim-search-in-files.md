---
layout: post
title: Search in multiple files with Vim
---

{{ page.title }}
================
Searching in files in Vim isn't very intuitive because you need to know the
correct syntax of the `:grep` or `:vimgrep` command.

I have written a function which searches the current working directory for a
pattern. Afterwards a quickfix window will show the matches. 

When pressing CTRL+F this input dialog will pop up.\\
![vim-search input dialog](/images/vim-search1.png)

Afterwards you see a list in Vim.\\
You can use `:cn` or `:cp` to navigate to the next and previous match in the 
list.
![vim-search](/images/vim-search2.png)

The function looks like this:
{% highlight vim %}
" open the search dialog with CTRL+F
map <C-f> :call Custom_Vim_Search()<CR>

function! Custom_Vim_Search()
  let pattern = inputdialog('search in files: ', expand('<cword>'))
  if !empty(pattern)
    try
      execute 'noautocmd vimgrep /' . pattern .  '/j **/** | botright copen'
    catch /^Vim\%((\a\+)\)\=:E480/ 
      " do nothing
    endtry
  endif
endfunction
{% endhighlight %}

Paste this code in your `~/.vimrc` and check it out. `:vimgrep` is a native Vim
command so you can also use it on Windows machines.


References
==========
- [Find files in Vim](http://vim.wikia.com/wiki/Find_in_files_within_Vim)
- [vimdoc: vimgrep](http://vimdoc.sourceforge.net/htmldoc/quickfix.html#:vimgrep)
