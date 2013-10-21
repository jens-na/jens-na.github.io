--- 
layout: post
title: How to get ~/.vim directory
---

{{ page.title }}
================
If you type `echo $VIM` or `echo $VIMRUNTIME` in Vim you can output the directories
where all the user files of Vim are located. Currently there is no built-in way
to output the `~/.vim` home directory (Windows `~\.vimfiles`).

    if has('win32') || has ('win64')
        let $VIMHOME = $VIM."/vimfiles"
    else
        let $VIMHOME = $HOME."/.vim"
    endif

Now if you type `echo $VIMHOME` you will see an output like `/home/user/.vim`.
